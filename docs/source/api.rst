API docs
========

WORK IN PROGRESS! This will list all API commands available in BarterDEX. WORK IN PROGRESS!

Introduction
------------

BarterDEX commands are called using RPC. If you installed BarterDEX by following the <INSERT LINK TO THAT GUIDE HERE> CLI manual installation guide, you'll have a lot of these commands ready to use in the ``~/SuperNET/iguana/dexscripts`` folder. These API docs will explain what each command does and what the possible arguments for each method are.

You need to set a strong passphrase (prior to starting marketmaker) and userpass to be able to talk to BarterDEX using RPC. The passphrase is what determines the addresses and the userpass value. 

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


Starting marketmaker
--------------------

There are two different nodes in BarterDEX: a full relay node and a node that doesn't relay. In the dexscripts folder, these are started by ./run or ./client, respectively. Normally, only Liquidity Providers will prefer a full relay node, as they can respond to incoming requests one hop sooner. Others will use ./client, since it doesn't require as much bandwidth as a full relay node.

A full relay node is started with the following command:

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
	"refbase":"kmd"
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

