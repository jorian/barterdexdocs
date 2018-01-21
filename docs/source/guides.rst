Guides
======

How to use your native wallet with BarterDEX
--------------------------------------------

- make sure to have valid .conf file set, with at least a rpcuser and (strong!) rpcpassword
- when a transaction was sent to the smartaddress after using it for the first time, the wallet needs to be rescanned. 

<coin>d -rescan in CLI
start <coin>-qt from CLI and add -rescan
 

How to build BarterDEX from source
----------------------------------

How to use Barterdex using CLI
------------------------------

- escape characters in passphrase

How to become a Liquidity Provider
----------------------------------

How to create a new BarterDEX trading network
---------------------------------------------

Since BarterDEX is a peer-to-peer network, seeded by some ip-addresses to create the network, others can create a BarterDEX network of their own. This enables people to trade within a private group of traders, or to trade directly from person to person.

Creating your own BarterDEX trade network is as easy as adding 1 command:
the only difference is how you launch the marketmaker -  by adding 2 arguments: 

\"netid\":2,\"seednode\":\"5.9.253.204\"

to the startup command, where netid can be anything up to 14420, and the seednode is the ip address of the node initializing the network. Participants of this network would have to start marketmaker with the same arguments.

How to claim a 0-conf deposit manually
--------------------------------------
