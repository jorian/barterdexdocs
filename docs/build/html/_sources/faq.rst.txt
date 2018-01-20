Frequently Asked Questions
==========================


What is an atomic swap?
-----------------------

An Atomic Swap is an on-chain, direct exchange of 2 different coins. For example KMD <-> LTC or BTC <-> SYS. It is atomic, because in a decentralized exchange, both parties must be assured that the other party is not able to skip out on his part, resulting in a loss of funds for 1 party. 

Atomic means that a swap either totally succeeds or not at all. 

Certain safeguards are needed to make sure the stealing of funds is not possible by either party.

What are Zcredits?
------------------

zcredits are credits for being able to do zero confirmation swaps
you get zcredits by depositing KMD and locking it for an x amount of weeks
so if you deposit 10 KMD you get 10 zcredits, if i'm not mistaken

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

.. code-block:: c
   
   timestamp = (uint32_t) time(NULL);
       timestamp /= LP_WEEKMULT;
       timestamp += weeks + 2;
       timestamp *= LP_WEEKMULT;



.. _Unspent Transaction Output: http://learnmeabitcoin.com/glossary/utxo 

Currently supported coins
-------------------------

===== ============
Coin  Name
===== ============
BTC   Bitcoin
LTC   Litecoin
KMD   Komodo
BTG   Bitcoin Gold
BCH   Bitcoin Cash
ZEC   Zcash
VTC   VertCoin
DOGE  DogeCoin
HUSH  Hush
DGB   DigiByte
XMCC  Monoeci
BTCH  Bitcoin Hush
CRC   CrowdCoin
VOT   VoteCoin
INN   Innova
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
===== ============

All the `Komodo Platform assetchains`_

What are the differences between BarterDEX and BlockNET DX?
-----------------------------------------------------------

What are the differences between BarterDEX and Altcoin.io?
----------------------------------------------------------

.. _Komodo Platform assetchains: https://www.komodoplatform.com/en/blog/komodo-smart-contracts-assetchains-and-geckochains

