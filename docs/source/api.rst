API docs
========

WORK IN PROGRESS! This will list all API commands available in BarterDEX. WORK IN PROGRESS!

Introduction
------------

BarterDEX uses a custom-made peer-to-peer network and has Remote Procedure Calls (RPC) to talk with and get information from the BarterDEX network. If you installed BarterDEX by following the <INSERT LINK TO THAT GUIDE HERE> CLI manual installation guide, you'll have a lot of these commands ready to use in the ``~/SuperNET/iguana/dexscripts`` folder. These API docs will explain what each command does and what the possible arguments for each method are.

``marketmaker`` is the process that is started on a machine.

You need to set a strong passphrase (prior to starting marketmaker) and userpass to be able to talk to BarterDEX using RPC. The passphrase is what determines the addresses and the userpass value, and will be needed in the startup arguments when starting the marketmaker process on your machine (see below).

Curl can be used to send commands, see the following example taken from the ``dexscripts`` folder:

.. code-block:: shell

   #!/bin/bash
   source userpass
   curl --url "http://127.0.0.1:7783" --data "{\"userpass\":\"$userpass\",\"method\":\"autoprice\",\"base\":\"KMD\",\"rel\":\"BTC\",\"margin\":0.0001}"

The userpass is sourced from the ``dexscripts`` folder, so that file has to be created in the ``dexscripts`` folder prior to sending a command. The userpass is there to prevent bad actors from issuing RPC commands. Marketmaker will not work without it set. 
Each command to BarterDEX will need this userpass value as value of the ``userpass`` key in the JSON.

``127.0.0.1:7783`` is the ip address and port where BarterDEX is listening for commands at default, when no rpcport is defined in the startup arguments.

The json code in all the methods below is the data that is needed for each method described (mind the escape characters when using shell). For example:

.. code-block:: json

   {
   	"userpass":"<userpass>",
   	"method":"autoprice",
   	"base":"KMD",
   	"rel":"BTC",
   	"margin":0.0001
   }

Replace everything that is shown as <this> with your own data.

Starting marketmaker
--------------------

There are two different nodes in BarterDEX: a full relay node (Liquidity Provider, or LP, node) and a node that doesn't relay (client node). In the ``dexscripts`` folder, these are started by ``./run`` or ``./client``, respectively. Normally, only Liquidity Providers will prefer a full relay node, as they can respond to incoming requests one hop sooner. Others will use ``./client``, since it doesn't require as much bandwidth as a full relay node and can therefore be perfect for regular traders on BarterDEX.

Startup arguments
^^^^^^^^^^^^^^^^^

The following startup arguments need to be provided when starting the marketmaker binaries.

A full relay node (LP node) is started by running marketmaker with the following arguments string:

.. code-block:: json

   {
	"gui":"<name of gui>", 
	"profitmargin":0.01,
	"userhome":"<userhome> + /",  
	"passphrase":"<passphrase>", 
	"coins":["<coins>"] 
   }

A node that doesn't relay (client node) has the following marketmaker startup arguments:

.. code-block:: json

   {
	"gui":"<name of gui>",
	"client":1,
	"userhome":"<userhome> + /", 
	"passphrase":"<passphrase>", 
	"coins":["<coins>"]
   }

- ``gui`` is the codename for the GUI used to start marketmaker with. If you are the developer of a GUI, you need to define a codename for your GUI. Share this in the Komodo Platform slack and you will get paid for every trade a user makes using your GUI. 
- ``profitmargin`` is the default profitmargin that this LP node will use when placing orders in orderbooks using the ``autoprice`` method. 
- ``client``: when set to 1, it defines a client node.
- ``userhome`` is the location of the userhome.
- ``passphrase`` is the passphrase that is needed by ``marketmaker`` to determine the userpass and all smartaddresses that BarterDEX is going to use. 
- ``coins`` needs a JSON of all BarterDEX-enabled coins. Not all cryptocurrencies are able to do atomic swaps, because they lack CheckLockTimeVerify (BIP65) or one of the necessary Bitcoin API methods (See :ref:`how-to-get-listed` for details).

Optional:

- ``wif`` when set to 1, the ``setpassphrase`` API call will show WIF addresses for all smartaddresses.

After ``marketmaker`` started successfully, the first RPC to be issued will always return a ``getcoin``  <REF TO GETCOIN> call for all coins, using 'default' as the default passphrase. This will also return the default userpass, which will need to be used to set the passphrase of the user, using the ``passphrase`` api call:

.. code-block:: json

   {
	"userpass":"1d8b27b21efabcd96571cd56f91a40fb9aa4cc623d273c63bf9223dc6f8cd81f",
	"method":"passphrase",
	"passphrase":"<passphrase>",
	"gui":"<name of gui>",
	"netid":0
   }

The ``netid`` needs to be defined when using a ``netid`` other than 0.

This method will return a response containing the ``userpass`` value for the user passphrase as defined in the ``passphrase`` method.

.. _new-or-private-network:

New or private network
^^^^^^^^^^^^^^^^^^^^^^

In order to start a network other than the default network, you need to add at least 2 arguments to the ``marketmaker`` startup arguments. When initiating a new network, a full relay node must be used, and it has to define ``"netid":<int netid>`` and ``"seednode":"<ipaddress>"`` to the marketmaker startup arguments, where the netid is any integer higher than 0 but lower than 14420. The seednode is the ip address of the server being a full relay node.

Non-relay nodes (``client``) need to use the same 2 arguments in its startup arguments, to be able to join that network.

At default, the RPC port for a marketmaker instance is 7783. To override this setting, add ``"rpcport":<int port>`` to the startup arguments. This port can be any port in the range 1025 - 65535. Defining the RPC port is for local networking; other nodes in the network do not have to comply by having the same RPC port settings.

Multiple marketmaker instances
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Multiple instances of marketmaker on the same machine are possible, by defining a different ``netid``, ``seednode`` (optional) and ``rpcport``. For example: One node is joining an existing network using ``netid:0`` and ``rpcport:8800``. A second instance of marketmaker can now be started with ``netid:1`` and ``rpcport:8810``. Each node has now access to a different network, and thus a different orderbook.

When initiating a new network, apart from defining the ``netid``, the ``seednode`` has to be defined too. As long as the combination of ``netid`` and ``seednode`` does not exist yet, a new network will be created. Therefore, multiple networks can exist with ``netid:0``, each with a different orderbook. The ``seednode`` is essential for defining multiple networks using the same ``netid``. When no ``seednode`` is defined, the `default seednodes`_ are used, which essentially are the seednodes of the main BarterDEX network. No new network will then be created; the ``marketmaker`` instance will be joining the existing, main BarterDEX network.

This basically means that an almost infinite number of BarterDEX networks can be created, using the ``netid`` and ``seednode`` startup arguments for ``marketmaker``.

.. _default seednodes: https://github.com/jl777/SuperNET/blob/dev/iguana/exchanges/LP_nativeDEX.c#L141 

General commands
----------------

- fetching orderbook
- get coin info, smart addy etc
- balance(s)
- listunspent
- swapstatus


Trade commands
--------------

Most, if not all, of the trade commands use the base/rel notation of pricing orders.

autoprice
^^^^^^^^^

The autoprice command is a rich command that allows anyone to create an order using data from CoinMarketCap or any other exchange. It refreshes the price every 1-2 minutes, such that once the autoprice command is executed, the order will be in the orderbooks permanently. 

There are several possibilities for autoprice:

fixed price
"""""""""""

The following command puts an ask in the BTC/KMD orderbook and basically says: 'I want to get KMD by selling BTC at a fixed price of 1800'. So, anyone who wants to buy BTC with KMD will see this order and can buy 1 BTC for 1800 KMD.

.. code-block:: json
   
   {
   	"userpass":"$userpass",
   	"method":"autoprice",
   	"base":"KMD",
   	"rel":"BTC",
   	"fixed":1800
   }

price with margin
"""""""""""""""""

<NEED TO ASK WHAT THIS DOES EXACTLY>

.. code-block:: json
   
   {
   	"userpass":"$userpass",
   	"method":"autoprice",
   	"base":"KMD",
   	"rel":"BTC",
   	"margin":0.01
   }

price based on external data
""""""""""""""""""""""""""""

The following command would refresh the price of the order in the orderbook based on price changes as defined in the ``refrel`` argument:

<NEED MORE INFO>

.. code-block:: json
   
   {
   	"userpass":"$userpass",
   	"method":"autoprice",
   	"base":"KMD",
   	"rel":"BTC",
   	"margin":0.05,
	"refbase":"kmd",
	"refrel":"coinmarketcap"
   }

.. note::

  the base and rel need to be uppercase and the refbase needs to be lowercase

UTXO tools
----------

withdraw
^^^^^^^^

sendrawtransaction
^^^^^^^^^^^^^^^^^^

Docker
------

lukechilds has provided a `docker image`_ for BarterDEX.

.. _docker image: https://github.com/lukechilds/docker-barterdex-api

