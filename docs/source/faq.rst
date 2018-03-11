Frequently Asked Questions
==========================


What is an atomic swap?
-----------------------

An Atomic Swap is an on-chain, direct exchange of 2 different coins. For example KMD <-> LTC or BTC <-> SYS. It is atomic, because in a decentralized exchange, both parties must be assured that the other party is not able to skip out on his part, resulting in a loss of funds for 1 party. 

Atomic means that a swap either totally succeeds or not at all. 

Certain safeguards are needed to make sure the stealing of funds is not possible by either party.

.. _how-to-get-listed:

How to get listed on BarterDEX?
-------------------------------

The requirements for a coin to be able to do an atomic swap are:

- have `BIP65 (Check LockTime Verify)`_ implemented;
- support the following standard Bitcoin API methods ( http://chainquery.com/bitcoin-api )

.. code-block:: bash

    estimatefee			importaddress
    getblock 			importprivkey
    getblockhash		listunspent
    getinfo			listreceivedbyaddress
    getrawmempool 		listtransactions
    getrawtransaction		validateaddress
    gettxout 			sendrawtransaction
    				signrawtransaction

Note that apart from supporting the above requirements, no additional custom code is needed to be able to trade on BarterDEX. (This in contrast to the misinformation being spread on several forums, where is stated that BarterDEX requires customizations to coins)


.. _BIP65 (Check LockTime Verify): https://github.com/bitcoin/bips/blob/master/bip-0065.mediawiki


Electrum or Native?
-------------------

Electrum lets you use the blockchain of the coin you want to trade, without having to download, install and sync that wallet. Just select Electrum and you are good to go.

Electrum is a Simple Payment Verification (SPV) service, somewhere on a server, that can be used to verify transactions on the network of the selected coin, without having to expose your private key. This means you can use this SPV service instead of going through the hassle of downloading the wallet of your coin, synchronizing the blockchain etc. 

Native is when you download, install and sync the wallet of the coin you want to trade. Trading with native wallets is faster and more stable than using Electrum. It is highly recommended for Liquidity Providers to use Native and not to use Electrum.

What are Zcredits?
------------------

Zcredits are credits for being able to do zero confirmation swaps. Zero confirmation swaps let you swap 2 coins in approximately 30 seconds, instead of waiting for the coin's block confirmation times. 

You get Zcredits by depositing KMD and locking it for an X amount of weeks. So if you deposit 10 KMD you get 10 Zcredits, and trades worth up to 10 KMD are now eligible for Zero Confirmation swaps. This is valid for all coins, even if KMD is not involved in a trade. BarterDEX will determine the value of the trade equivalent in KMD to check if a Zero Confirmation swap is possible. For BarterDEX to be able to determine this, a price must be set for the coin/KMD pair.

What are UTXOs?
---------------

UTXO stands for `Unspent Transaction Output`_. BarterDEX trades UTXOs, not balances. This makes trading different from trading on a centralized exchange. Because of the atomic swap protocol, UTXOs must have certain sizes to be eligible for a trade. 

Basically this means that you need to make at least 3 transactions to your smartaddress to be able to trade. These transactions should have sizes X, X * 1.2 and X * 0.1. 

Why are multiple UTXOs needed?
------------------------------

BarterDEX is an UTXO based exchange. This means that 1 UTXO is exchanged for 1 other UTXO. For example: 1 KMD UTXO with a value of 32 KMD is traded for 1 BCH UTXO with a value of 0.5 BCH.

Why can't I claim my expired 0-conf deposit?
--------------------------------------------

This is due to avoid bad actors stealing the deposit, and depends on the time you made a deposit. Funds are available 3 - 10 days after the expiration of the number of weeks you defined when making the deposit.

The calculation in the code:

.. code-block:: c
   
   timestamp = (uint32_t) time(NULL);
   timestamp /= LP_WEEKMULT;
   timestamp += weeks + 2;
   timestamp *= LP_WEEKMULT;

Doing a claim on a still locked deposit will return the time you need to wait before your deposit can successfully be claimed.

.. _Unspent Transaction Output: http://learnmeabitcoin.com/glossary/utxo 

Who are alice and bob?
----------------------

In BarterDEX terms, alice is the one who initiates a trade. She buys an order from bob, who already put up an order in the orderbook. It is like saying bob has a marketstand with something to sell and alice walks up to the stand to buy something.

Is it free to cancel an order?
------------------------------

Yes. Placing orders and sending a request doesn't cost you anything. Only when your request has found a willing trade partner and a connection has been established, the dexfee and transaction fees will be paid.


How do I get the private key of my smartaddress?
------------------------------------------------

BarterDEX uses watch-only addresses, which basically means that BarterDEX is a trade wallet. The passphrase you enter when starting BarterDEX is the access to your trade wallet and thus, coins. 

It requires starting ``marketmaker`` from the command line to retrieve all private keys of all the smartaddresses, based on the passphrase. You do this by adding ``"wif":1`` to the marketmaker startup arguments json. In the initial ``getcoin`` that marketmaker does, it will return all wifs for each smartaddress.

For Komodo formatted addresses, it is possible to do the :ref:`api_calcaddress` api call.

How much are the fees?
----------------------

Fees for using the exchange exist in paying a dexfee, to be paid by alice (the one initiating the trade), also called the taker fee. This is about 0.15% of the alicepayment - the amount you're sending to the other party.

There are no maker fees.

You also pay the standard transaction fees, for sending the payment to the other party.

The dexfees are collected in a specific address, generated from the rmd160 hash ca1e04745e8ca0c60d8c5881531d51bec470743f. For Komodo, this is `RThtXup6Zo7LZAi8kRWgjAyi1s4u6U9Cpf`_ Once a significant amount of fees are collected, the fees are paid as dividend to DEX assetholders. DEX is a Komodo asset and can be traded on BarterDEX.


Currently supported coins
-------------------------

===== ============ ======== ================
Coin  Name         Asset    Name/description
===== ============ ======== ================
BTC   Bitcoin      REVS     Revenue Shares
LTC   Litecoin     SUPERNET Supernet / Unity
KMD   Komodo       DEX      InstantDEX
BTG   Bitcoin Gold PANGEA   Pangea Poker
BCH   Bitcoin Cash JUMBLR   `JUMBLR`_           
ZEC   Zcash        BET      BET Platform
VTC   VertCoin     CRYPTO   `CRYPTO777`_        
DOGE  DogeCoin     HODL     HODL
HUSH  Hush         MSHARK   MSHARK
GRS   GroestlCoin  BOTS     Tradebots
DGB   DigiByte     COQUI    Coqui
XMCC  Monoeci      WLC      WirelessCoin
BTCH  Bitcoin Hush KV       Key-Value
CRC   CrowdCoin    CEAL     CEAL
VOT   VoteCoin     MESH     MESH
INN   Innova       ETOMIC   ERC20 
MOON  MoonCoin
CRW   Crown
EFL   eGulden
GBX   GoByte
BCO   BridgeCoin
BLK   BlackCoin
ABY   Applebyte
STAK  Straks
XZC   Zcoin
QTUM  QTUM
PURA  PURA
DSR   Desire
MNZ   Monaize
BTCZ  Bitcoin Z
MAGA  MagaCoin
BSD   Bitsend
IOP   IoP
BLOCK BlockNET DX
CHIPS CHIPS
888   OctoCoin
ARG   Argentum
GLT   Global Token
ZER   Zero
HODLC HOdlcoin
UIS   Unitus
===== ============ ======== ================

All the `Komodo Platform assetchains`_

What are the differences between BarterDEX and BlockNET DX?
-----------------------------------------------------------

BlockNET DX, or BlockDX, is a coin which focus solely lies on creating a Decentralized Exchange. The differences between BarterDEX and BlockDX are subtle, but important.

1. First of all, the fees:

=========   ======= =========
fees:       BlockDX BarterDEX
=========   ======= =========
Maker-fee   0.05%   none
Taker-fee   0.20%   0.15%
=========   ======= =========

These fees do not take standard transaction fees into account, which for BarterDEX is based on the amount of transactions necessary to do an atomic swap (4 for maker, 3 for taker). BlockDX is (at the time of this writing) not yet live, so nothing can be said of how many standard transaction fees they require. 

2. BlockDX uses so-called Service Nodes to be able to do atomic swaps. They are essential in the process of doing an atomic swap; without the Service Nodes, you can't trade. This in stark contrast with BarterDEX, where you don't need Service Nodes to be able to trade. 2 nodes, both running BarterDEX, are able to do an atomic swap between each other, without needing anything else.

3. To use the BlockDX, you need to download and install the BlockDX wallet. BarterDEX is not tied to any cryptocurrency; all you need is the marketmaker executable that gives you access to BarterDEX networks.

At the time of this writing, BlockDX has yet to release (a beta of) their DEX and its source code. Until then, no proper comparison can be made between BlockDX and BarterDEX. It is clear though, that BlockDX is going to be the closest 'competitor' to BarterDEX. 


Do I need to leave BarterDEX running all the time?
--------------------------------------------------

Yes. Atomic swaps needs transactions signed with your private key, so you need to leave BarterDEX running to be able to execute orders.

Yes, that possibility exists, but for now it's only done using the Command Line. See the guide in our Guides section explaining what needs to be done.

.. _RThtXup6Zo7LZAi8kRWgjAyi1s4u6U9Cpf: https://kmd.explorer.supernet.org/address/RThtXup6Zo7LZAi8kRWgjAyi1s4u6U9Cpf
.. _JUMBLR: https://nxtforum.org/nxtservices-releases/jumblr-decentralized-bitcoin-mixer-seeking-marketing-lead-and-also-gui-dev/
.. _CRYPTO777: https://nxtforum.org/consensus-research/crypto777/
.. _Komodo Platform assetchains: https://www.komodoplatform.com/en/blog/komodo-smart-contracts-assetchains-and-geckochains

