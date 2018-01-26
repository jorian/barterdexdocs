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

``127.0.0.1:7783`` is the ip address and port where BarterDEX is listening for commands.

The json code in all the methods below is the data that is needed for each method described (mind the escape characters when using shell). For example:

.. code-block:: json

   {
   	"userpass":"$userpass",
   	"method":"autoprice",
   	"base":"KMD",
   	"rel":"BTC",
   	"margin":0.0001
   }

After ``marketmaker`` started successfully, the first RPC to be issued will always return a ``getcoin``  <REF TO GETCOIN> call for all coins.


Starting marketmaker
--------------------

There are two different nodes in BarterDEX: a full relay node and a node that doesn't relay. In the ``dexscripts`` folder, these are started by ``./run`` or ``./client``, respectively. Normally, only Liquidity Providers will prefer a full relay node, as they can respond to incoming requests one hop sooner. Others will use ``./client``, since it doesn't require as much bandwidth as a full relay node and can therefore be perfect for regular users, ie. traders on BarterDEX.

Startup arguments
^^^^^^^^^^^^^^^^^

A full relay node is started by running marketmaker with the following arguments string:

.. code-block:: json

   {
	"gui":"<name of gui>", 
	"profitmargin":0.01,
	"userhome":"<userhome + />",  
	"passphrase":"<passphrase>", 
	"coins":["<coins>"] 
   }

``gui`` is the codename for the GUI used to start marketmaker with. If you are the developer of a GUI, you need to define a codename for your GUI. Share this in the Komodo Platform slack and you will get paid for every trade a user makes using your GUI.

``profitmargin`` is the default profitmargin that this full relay node (or Liquidity Provider node) will use when placing orders in orderbooks using the ``autoprice`` method.

``userhome`` is the location of the userhome.

``passphrase`` is the passphrase that is needed by ``marketmaker`` to determine the userpass and all smartaddresses that BarterDEX is going to use. 

``coins`` needs a JSON of all BarterDEX-enabled coins. Not all cryptocurrencies are able to do atomic swaps, because they lack CheckLockTimeVerify (BIP65) or one of the necessary Bitcoin API methods. (See `How to get listed on BarterDEX`_ for details)

.. _

New or private network
^^^^^^^^^^^^^^^^^^^^^^

In order to start a network other than the default network, you need to add 2 arguments to the marketmaker startup arguments. When initiating a new network, a full relay node must be used, and it has to define ``"netid":<int netid>`` and ``"seednode":"<ipaddress>"`` to the marketmaker startup arguments, where the netid is any integer higher than 0 but lower than 14420. The seednode is the ip address of the server being a full relay node.


Trade commands
--------------

Most of the commands use the base/rel notation of pricing orders.

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

