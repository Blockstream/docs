.. _node-setup:

Liquid Node Setup
*****************

Overview
--------

In this section we will look at how to install and run a full Liquid client node. We will also see how to use the configuration file to switch the node between:

* Connecting to the live Liquid network.

* Running in a local test/development mode (similar to Bitcoin's "regtest").

When run against the live Liquid network, our node will be able to connect to other Liquid network peers, allowing it to receive, validate and relay transactions and blocks.

In local test/development mode, our node will start its own copy of a new Liquid blockchain. The live and test chains will be stored in different folders on your machine, so they will never conflict with each other and the wallets will remain separate. This allows us to perform actions like sending funds, issuing assets, and generating blocks without fear of losing real funds.

When connected to the live network, our node will act as a "client" node. Other nodes on the network, called :ref:`Functionary <to-functionary>` nodes, are responsible for securing the transfer of Bitcoin in to and out of the Liquid network and also for the creation of blocks through the block signing process. The blocks these Functionary nodes create are then relayed to other network participants. When our node runs in local test mode, we will be able to generate blocks ourselves, making the development and testing process a lot simpler. You can read more about the different types of Liquid nodes and the roles they play on the network in the :ref:`Technical Overview <technical-overview>`.

To set a Liquid node up we need to:

* Download/install the Liquid binaries.

* Configure our node using a configuration file.

Before we begin, it is worth giving a brief overview of the applications we will be downloading and using.

**Liquid Daemon (liquidd)**

A Liquid node that runs as a background service. It can also processes requests made from other applications using Remote Procedure Call (RPC). It cannot be run at the same time as liquid-qt if they share the same data directory, which they will by default.

**Liquid QT (liquid-qt)**

A desktop application (GUI/front-end) that serves as a Liquid node. It can also process requests using Remote Procedure Call (RPC). It cannot be run at the same time as liquidd if they share the same data directory, which they will by default.

**Liquid Client (liquid-cli)**

A client application that allows you to make calls to liquidd or liquid-qt by issuing RPC commands. Use of liquid-cli is detailed in the :ref:`Developer Guide <developer-guide>` section.

Liquid will, by default, try and connect to a local Bitcoin node in order to validate transactions where bitcoin are moved into the Liquid network (called a peg-in). We will assume that you have already installed and set up a Bitcoin node before moving on with installing Liquid. This requirement can be switched off using the ``validatepegin`` parameter within the Liquid configuration file but we advise against disabling peg-in validation unless you are aware of the implications, running in a testing environment, or are not dealing with large amounts of funds.


Installation
------------

The applications listed above can be downloaded from the `Elements Github repository <https://github.com/ElementsProject/elements/releases>`_ as either an installation/setup package or as the contents of a compressed file.

You will find variants for Linux, Windows, and MacOS.

Using the 0.17 release (the latest at time of writing) as an example:


**Linux**

There are a number of Linux distrutions supported. For example; Ubuntu users can choose the `x86_64 linux <https://github.com/ElementsProject/elements/releases/download/elements-0.17.0/liquid-0.17.0-x86_64-linux-gnu.tar.gz>`_ file, whereas Raspberry Pi users should choose the `arm linux <https://github.com/ElementsProject/elements/releases/download/elements-0.17.0/liquid-0.17.0-arm-linux-gnueabihf.tar.gz>`_ file.


**Windows**

You can choose the `win64 setup unsigned <https://github.com/ElementsProject/elements/releases/download/elements-0.17.0/elements-0.17.0-win64-setup-unsigned.exe>`_ file if you are happy to run an unsigned setup on your Windows x64 machine. If you need a 32bit variant or a version without an installer, you should chose the appropriate file from the list.


**MacOS Installation**

You can choose the `OSX unsigned <https://github.com/ElementsProject/elements/releases/download/elements-0.17.0/liquid-0.17.0-osx-unsigned.dmg>`_ dmg image file, or download the binaries themselves.


After installation, you are now ready to move on to configuring your node.


Configuration
-------------

This guide assumes you are already running a Bitcoin full node. To allow Liquid Core to communicate with your Bitcoin node, certain parameters must be set in your Bitcoin configuration file, and then possibly included in the Liquid configuration file.

bitcoin.conf
============

We will first make sure that our Bitcoin node is set up to allow our Liquid node to communicate with it.

Include the following in your ``bitcoin.conf`` file:

.. code-block:: text

	server=1

Using a cookie file is the prefered way of authenticating against a Bitcoin node. When a Bitcoin node starts up with the server parameter set (as above), it will automatically generate a cookie file for this purpose. We will later tell our Liquid node the location of this file so that it can authenticate against the Bitcoin node.

Alternatively, you can also use the RPC parameters (``rpcuser``, ``rpcport``, and ``rpcpassword``) as the authentication method.

If you want to use the RPC parameter method of allowing access, then also set the following within bitcoin.conf. 

*Note that these values will start your Bitcoin node in "regtest" mode*:

.. code-block:: text

	regtest=1
	regtest.rpcport=18888
	regtest.port=18889
	rpcuser=<your user>
	rpcpassword=<your password>

You may also want to include the ``prune`` parameter in your Bitcoin node settings. Pruned mode reduces disk space requirements but will will not change the initial amount of time required for download and validation of the chain.


liquid.conf
===========

The liquidd, liquid-qt and liquid-cli applications will all use a configuration file named liquid.conf. The liquid.conf file tells liquidd and liquid-qt which network to connect to and can set a number of different behaviours within the applications. It also tells them what credentials must be provided in order to accept an RPC request. The liquid-cli application uses the configuration file to obtain the correct credentials in order to communicate with liquidd or liquid-qt using RPC. 

When you start either of the three applications you can provide a ``datadir`` path. The path you provide tells the applications which directory to use to:

* Obtain RPC authentication data (user, password, port).

* Store blockchain and wallet data.

* Store log files etc.

If you want to use a different data directory that the defaults referenced below, for example an external hard drive, you can follow `this guide <https://bitzuma.com/posts/moving-the-bitcoin-core-data-directory/>`_.

The liquid.conf configuration file is located in the following places by default:

**Linux**

~/.liquid/

**Windows**

%homepath%\AppData\Roaming\Liquid

**MacOS**

Select Macintosh HD and then Library/Application Support/Liquid.


If you do not see the directory or the liquid.config file you should create them now. Otherwise, open the file for editing.

.. note::

	After making any changes to liquid.conf in the future, you will need to restart your Liquid node so that they take effect.


If your Bitcoin node is installed in the default location, Liquid should automatically find it. If you use a non-default location for your Bitcoin node, you will have to add the following parameter to your liquid.conf file, pointing to the cookie file created by your Bitcoin node:

.. code-block:: text

	mainchainrpccookiefile=<location_of_your_bitcoin_datadir>

If you want to use the RPC parameter method of allowing access to your Bitcoin node then also set the following within liquid.conf, using the same user, password, and port that you set in bitcoin.conf:

.. code-block:: text

	mainchainrpcport=<18888_for_example>
	mainchainrpcuser=<your_bitcoin_rpc_user_here>
	mainchainrpcpassword=<your_bitcoin_rpc_password_here>

If you want to allow your Liquid node to accept RPC requests (such as those used in the :ref:`Developer Guide <developer-guide>`) then also set the following. 

*Note that these values will start your Liquid node in test/development mode*:

.. code-block:: text

	chain=elementsregtest
	rpcuser=<your_liquid_rpc_user_here>
	rpcpassword=<your_liquid_rpc_password_here>
	elementsregtest.rpcport=<18884_for_example>
	elementsregtest.port=<18886_for_example>

.. tip::
	To switch between live and test/development modes you will need to change the ``chain`` value between ``liquidv1`` (live) and ``elementsregtest`` (test/development). You must restart your node for these to take effect.

If you do not wish to validate peg-ins against your Bitcoin node, you can set the ``validatepegin`` parameter to a value of ``0``. This can be done either in the liquid.conf file, or passed in as a command line parameter.

.. code-block:: text

	validatepegin=0

With this setting, you do not need to run a Bitcoin node as Liquid will not attempt to connect to one on startup. 

.. warning::
	We advise against disabling peg-in validation unless you are aware of the implications, running in a testing environment, or are not dealing with large amounts of funds.  

A complete `Liquid configuration file template <https://github.com/ElementsProject/elements/blob/master/share/examples/liquid.conf>`_ can be found here.


Running your Liquid Node
------------------------


**Linux**

You will be able to run each of the applications from the command line within the folder you extracted them to. For example:

.. code-block:: bash

	./liquidd

or

.. code-block:: bash

	./liquid-qt

and 

.. code-block:: bash

	./liquid-cli

Depending on your system set up, you may have to change the permissions on the files before they will run.


**Windows**

On Windows you can run Liquid QT as a normal desktop application and make RPC calls to it from other applications and from the liquid-cli application.


You cannot run the liquidd application just by starting it like any other application, as it needs to be run as a background service. You will have to configure Windows to start liquidd as a background service if you want to run it this way.

The liquid-cli application can be called and used from the command prompt within Windows.


**MacOS**

If you installed Liquid QT from the dmg image, you can run it as a normal desktop application, otherwise the applications can be started from the terminal within the folder you extracted them to. For example:

.. code-block:: bash

	./liquid-qt

You should now be set up to start using your node. 

You can connect it to the live Liquid network by setting ``chain=liquidv1`` and letting it sync its local copy of the Liquid blockchain. 

You might also want to switch your Liquid node to test/development mode using ``chain=elementsregtest`` and start the :ref:`Developer Guide <developer-guide>` and :ref:`App Examples <liquid-app-examples>` sections if you want to.
