API2 docs
========

BarterDEX API Summary by Category

Summary info for each API by Category
-------------------------------------

barterDEX Operation
^^^^^^^^^^^^^^^^^^^

:ref:`client`, :ref:`clientosx`, :ref:`convaddress`, :ref:`debug`, :ref:`disable`, 
:ref:`electrum`, :ref:`enable`, :ref:`getcoin`, :ref:`getcoins`, :ref:`getpeers`, :ref:`getpeersIP`, 
:ref:`help`, :ref:`jpg`, :ref:`millis`, :ref:`notarizations`, :ref:`parselog`, :ref:`run`, 
:ref:`setpassphrase`, :ref:`sleep`, :ref:`stop`, :ref:`trust`, :ref:`trusted`

BarterDEX Operation
-------------------

.. _client:

client
^^^^^^

The first API to run which will start barterDEX in client mode. Next script to run is ``setpassphrase``. If you want to close barterDEX, issue ``pkill -15 marketmaker`` every time. This ensures all barterDEX process is killed safely.

Sample File Contents:

.. code-block:: shell

	#!/bin/bash
	source passphrase
	source coins
	./stop
	git pull;
	cp ../exchanges/updateprices .;./updateprices
	cd ..; 
	./m_mm;
	pkill -15 marketmaker; 
	stdbuf -oL $1 ./marketmaker "{\"gui\":\"nogui\",\"client\":1,\"userhome\":\"/${HOME#"/"}\",\"passphrase\":\"$passphrase\",\"coins\":$coins}" &

Fields that can be used: ``gui:nogui`` or ``gui:SimpleUI`` - to identify the CLI/GUI being used to run marketmaker. ``client:1`` - to start marketmaker as client mode. ``"userhome\":\"/${HOME#"/"`` - defines the data dir for barterDEX. DB dir contains PRICES, SWAPS, UNSPENTS and instantdex_deposit (0conf) files. ``passphrase:$passphrase`` - is to set the passphrase to run the marketmaker with, to run and login into marketmaker using WIF (Wallet Import Format) key instead of passphrase. You need to use the WIF key as passphrase inside ``passphrase`` file. barterDEX supports login using WIF Key for all supported coins instead of passphrase. ``wif:1`` - will display the passphrase's WIF during the first api return. ``rpcport:port`` - to change the 7782 port to other open port of choice.
	
