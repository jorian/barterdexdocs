Guides
======

.. _cli-manual-installation-guide:

How to use BarterDEX using CLI
------------------------------

Using BarterDEX with the Command Line Interface, you get access to all the low level Remote Procedure Calls (RPC) that exist in BarterDEX. It gives you power and control over how you want to be a trader / marketmaker. Throughout this guide, the command line (terminal) is used for installing BarterDEX.

Requirements
^^^^^^^^^^^^

At this time, only Linux and MacOS are supported.

Linux (16.04)
"""""""""""""
Download and install Nanomsg:

.. code-block:: bash

    git clone https://github.com/nanomsg/nanomsg
    cd nanomsg
    cmake .
    make
    sudo make install
    sudo ldconfig

Install required packages:

.. code-block:: bash

    sudo apt-get update && sudo apt-get install git libcurl4-openssl-dev build-essential

MacOS
"""""

To install Nanomsg on MacOS, it's easiest to install it using Homebrew. Homebrew is a handy package manager for MacOS. If you don't have it already, download it by entering the following into a Terminal window:

.. code-block:: bash

    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Afterwards, install Homebrew by typing:

.. code-block:: bash

    brew install nanomsg

That's it!

Installation & Setup
^^^^^^^^^^^^^^^^^^^^

Installation is the same for Linux and MacOS. Start by cloning the SuperNET repository:

.. code-block:: bash

    git clone https://github.com/jl777/SuperNET -b dev

This will download all the files necessary to start using BarterDEX from the command line. It uses the Development branch, assuring you with the latest updates. If you don't want this, type ``git checkout master`` to work with more stable releases.

Now navigate to the SuperNET folder and execute the install script:

.. code-block:: bash

    cd ~/SuperNET/iguana/exchanges
    ./install

This copies a bunch of scripts in the dexscripts folder, which is where we need to go next. This dexscripts folder now contains executable scripts that issue the most common API calls. 

.. code-block:: bash

    cd ../dexscripts

Before we can use any of these scripts, some security measures are needed to prevent bad actors from issuing API calls without your consent. The ``passphrase`` and ``userpass`` values take care of this security.

Create the passphrase file:

.. code-block:: bash

    nano passphrase
    
This file should contain the following line, with a strong passphrase between the <>:

.. code-block:: bash

    export passphrase="<strong userpass value here>"

``Ctrl-x`` to exit, and press ``y`` to save the changes.

The ``userpass`` value is derived from the ``passphrase`` value, and in order to obtain the ``userpass`` value, we need to start BarterDEX. Starting BarterDEX (or actually the ``marketmaker`` process) is done by executing the ``client`` script, which basically is an automated combination of retrieving the latest updates and building the marketmaker executable file: (it can take a while before anything shows up)

.. code-block:: bash

    ./client

Let it load until you see a line that starts with ``>>>>>>>>>> DEX stats 127.0.0.1:7783``. This means the BarterDEX node is now up and running and that it's able to listen for commands.

To obtain the ``userpass`` value, we need to execute the ``setpassphrase`` script. Open a new terminal window, navigate to the dexscripts folder and execute the ``setpassphrase`` script:

.. code-block:: bash

    cd ~/SuperNET/iguana/dexscripts
    ./setpassphrase

The response contains the ``userpass`` value. Copy this value and paste it in a newly created userpass file:

.. code-block:: bash

    nano userpass
    export userpass="<paste userpass value here>"

``Ctrl-x`` to exit, ``y`` to save changes.

Everything is now good to go. From here on, you can issue any script that is in the dexscripts folder, such as the ``orderbook`` script, that fetches all the orders from the specified pair, or the ``getcoin`` script that gets all the coin-specific information from the coin as defined inside that script. 

The API docs explain all the BarterDEX API calls.

How to use Insomnia together with the CLI
-----------------------------------------

Insomnia is a great tool to replace the terminal window, but still be able to issue all the API calls in a visually more attractive way.

Download Insomnia here: https://insomnia.rest


How to create a new BarterDEX trading network
---------------------------------------------

Since BarterDEX is a peer-to-peer network, seeded by some ip-addresses to create the network, others can create a BarterDEX network of their own. This enables people to trade within a private group of traders, or to trade directly from person to person.

See :ref:`new-or-private-network` in the API docs on how to do this.
