.. _node-setup:

Node Setup
**********

A Liquid node refers to a "full" client that is validating and relaying transactions with other members in the Federation. Block proposal and signing is performed by :ref:`Functionary <to-functionary>` members and relayed to all other clients in the network.

If a Liquid end node connects to the network via a fully validating node such as functionary or :ref:`pseudo-functionary <to-participant>` (participant) node, then it is possible to run the end node  in prune mode and without having to run an instance of a bitcoin client on the same machine, all this, without having to compromise security. To run a Liquid node without validating the Bitcoin chain (only recommended for testing or when already connected to a fully validating node) use ``validatepegin=0`` in your ``liquid.conf`` file.

All binaries variants are available at `Elements Github repository <https://github.com/ElementsProject/elements/releases>`_.

**End Node Hardware Requirements**

* Minimum requirements are application dependant. For a pruned node, at least 20 GB of free space is recommended.


Windows Installation
====================

For Windows you can download the installer available here; `Win64 setup unsigned <https://github.com/ElementsProject/elements/releases/download/elements-0.17.0/elements-0.17.0-win64-setup-unsigned.exe>`_.

If you prefer a 32bit variant or a variant without installer please see avaliable `releases <https://github.com/ElementsProject/elements/releases>`_.

.. note::
	If you want to use a different data directory, for example an external hard drive, you can follow `this guide <https://bitzuma.com/posts/moving-the-bitcoin-core-data-directory/>`_.

If you want to make changes to your Liquid settings, locate the config file in the default data directory ``%homepath%\AppData\Roaming\Liquid``. After making the required changes, you will need to restart your Liquid node so that they take effect.


MacOs X Installation 
====================

For MacOs X you can download the dmg file available here; `OSX unsigned <https://github.com/ElementsProject/elements/releases/download/elements-0.17.0/liquid-0.17.0-osx-unsigned.dmg>`_.

.. note::
	If you want to use a different data directory, for example an external hard drive, you can follow `this guide <https://bitzuma.com/posts/moving-the-bitcoin-core-data-directory/>`_.

If you want to make changes to your Liquid settings, use Finder to locate the config file by selecting Macintosh HD then ``Library/Application Support/Liquid`` and opening the configuration file with a text editor. After making the required changes, you will need to restart your Liquid node so that they take effect.


Linux Installation 
==================

Download and untar the Liquid binaries available here; `x86_64 Linux <https://github.com/ElementsProject/elements/releases/download/elements-0.17.0/liquid-0.17.0-x86_64-linux-gnu.tar.gz>`_.

.. note::
	If you want to use a different data directory, for example an external hard drive, you can follow `this guide <https://bitzuma.com/posts/moving-the-bitcoin-core-data-directory/>`_.

If you want to make changes to your Liquid settings, locate and edit the config file in ``~/.liquid/``. After making the required changes, you will need to restart your Liquid node so that they take effect.


Setting up Configuration Files
==============================

By default, Liquid client requires that you run a Bitcoin node so that Liquid can validate peg-ins (transactions that move bitcoin into the Liquid Network).

This guide assumes you are already running a Bitcoin full node. To allow Liquid Core to communicate with your Bitcoin node, certain parameters must be included in your Bitcoin configuration file and potentially to the Liquid configuration file.


bitcoin.conf
-------------

.. note::
	Using a cookie file is now the prefered way of authenticating against bitcoind. Alternatively, you can also use the RPC parameters (``rpcuser``, ``rpcport``, and ``rpcpassword``) as the authentication method.

Include the following parameters in your ``bitcoin.conf`` file:

.. code-block:: text

	server=1

You may also want to include the ``prune`` parameter in your Bitcoin node settings. Pruned mode reduces disk space requirements but will will not change the initial amount of time required for download and validation of the chain.

liquid.conf
-------------

If your Bitcoin node is installed in the normal default location Liquid should automatically find it.  But if you use a non default datadir for bitcoin you may want to add to your ``liquid.conf`` the following parameter to point to the cookie file created by bitcoin (default bitcoin datadir):

.. code-block:: text

	mainchainrpccookiefile=<location_of_your_bitcoin_datadir>

If bitcoind rpc authentication is done through user and password, include the following parameters instead. Notice that these values should be taken from your ``bitcoin.conf`` file.

.. code-block:: text

	mainchainrpcuser=<your_bitcoin_rpc_user_here>
	mainchainrpcpassword=<your_bitcoin_rpc_password_here>

If you do not wish to validate peg-ins against your Bitcoin node, you can set the ``validatepegin`` parameter to a value of ``0``. This can be done either in the ``liquid.conf`` file, or passed in as a command line parameter:

.. code-block:: text

	validatepegin=0

With this setting, you do not need to run a Bitcoin node and Liquid will not attempt to connect to one on startup. 

.. warning::
	We advise against disabling peg-in validation unless you are aware of the implications, running in a testing environment, or are not dealing with large amounts of funds.  

A complete `Liquid configuration file template <https://github.com/ElementsProject/elements/blob/master/share/examples/liquid.conf>`_ can be found here.
