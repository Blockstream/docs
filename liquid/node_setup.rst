.. _node-setup:

Liquid Node Setup (Advanced)
****************************

Overview
--------

Liquid is built using open-source `Elements <https://github.com/ElementsProject/elements>`_
code. This is why we'll download the application from the Elements repository, execute
commands against elementsd (daemon) and elements-cli (client), and edit things like the
elements.conf config file.

As the Elements code defaults to using the live Liquid network, we'll be referring to
installing and running Liquid, and not Elements, within this guide and throughout the
`docs.blockstream.com <https://docs.blockstream.com>`_ site.

In this section we will look at how to install and run a full Liquid client node and use
the configuration file to switch the node between:

* Connecting to the live Liquid network.

* Running in a local test/development mode (similar to Bitcoin's "regtest").

When run against the live Liquid network, our node will be able to connect to other Liquid
network peers, allowing it to receive, validate and relay transactions and blocks.

In local test/development mode, our node will start its own copy of a new Liquid
blockchain. The live and test chains will be stored in different folders on your machine,
so they will never conflict with each other and the wallets will remain separate. This
allows us to perform actions like sending funds, issuing assets, and generating blocks
without fear of losing real funds.

When connected to the live network, our node will act as a "client" node. Other nodes on
the network, called :ref:`Functionary <to-functionary>` nodes, are responsible for
securing the transfer of Bitcoin in to and out of the Liquid network and also for the
creation of blocks through the block signing process. The blocks these Functionary nodes
create are then relayed to other network participants. When our node runs in local test
mode, we will be able to generate blocks ourselves, making the development and testing
process a lot simpler. You can read more about the different types of Liquid nodes and
the roles they play on the network in the :ref:`Technical Overview <technical-overview>`.

To set a Liquid node up we need to:

* Download/install the Liquid binaries.

* Configure our node using a configuration file.

Before we begin, it is worth giving a brief overview of the applications we will be
downloading and using.

**Liquid Daemon (elementsd)**

A Liquid node that runs as a background service. It can also processes requests made from
other applications using Remote Procedure Call (RPC). It cannot be run at the same time as
elements-qt if they share the same data directory, which they will by default.

**Liquid Core/QT (elements-qt)**

A desktop application (GUI/front-end) that serves as a Liquid node. It can also process
requests using Remote Procedure Call (RPC). It cannot be run at the same time as elements
if they share the same data directory, which they will by default.

**Liquid Client (elements-cli)**

A client application that allows you to make calls to elementsd or elements-qt by issuing
RPC commands. Use of elements-cli is detailed in the :ref:`Developer Guide <developer-guide>`
section.

Liquid will, by default, try and connect to a local Bitcoin node in order to validate
transactions where bitcoin are moved into the Liquid network (called a peg-in). We will
assume that you have already installed and set up a Bitcoin node before moving on with
installing Liquid. This requirement can be switched off by setting ``validatepegin=0``
within the :ref:`Liquid configuration <node-configuration>` (elements.conf) file, but we
advise against disabling peg-in validation unless you are aware of the implications,
running in a testing environment, or are not dealing with large amounts of funds.


Installation
------------

The applications listed above can be downloaded from the `Elements Github repository
<https://github.com/ElementsProject/elements/releases>`_ as either an installation/setup
package or as the contents of a compressed file.

.. Note:: After you have followed the install instructions below (you will find variants
   for Linux, Windows, and MacOS) do not run the software until you have followed the
   instructions in the Configuration section.

Some releases are 'code only' releases. If you only want to use the binaries, select the
most recent release that has associated binary installation files. Using the 0.17 release
(the latest at time of writing) as an example:


**Linux**

There are a number of Linux distrutions supported. For example; Ubuntu users can choose
the ``elements-<version>-x86_64-linux-gnu.tar.gz`` file, whereas Raspberry Pi users should
choose the ``elements-<version>-arm-linux-gnueabihf.tar.gz`` file.


**Windows**

You can choose the ``elements-<version>-win64-setup-unsigned.exe`` file if you are happy to
run an unsigned setup on your Windows x64 machine. If you need a 32bit variant or a
version without an installer, you should chose the appropriate file from the list.


**MacOS Installation**

You can choose the ``elements-<version>-osx-unsigned.dmg`` dmg image file, or download the
binaries themselves.


After installation, you are now ready to move on to configuring your node.

.. _node-configuration:

Configuration
-------------

This guide assumes you are already running a Bitcoin full node. To allow Liquid Core to
communicate with your Bitcoin node, certain parameters must be set in your Bitcoin
configuration file, and then possibly included in the Liquid configuration file.

bitcoin.conf
============

We will first make sure that our Bitcoin node is set up to allow our Liquid node to
communicate with it.

Include the following in your ``bitcoin.conf`` file if it is not already present:

.. code-block:: text

	server=1

This will ensure that your Bitcoin node creates a 'cookie' file within the data directory
it is using on start up. The cookie file allows other applications to locally authenticate
against your Bitcoin node, as long as they know where it is located. Using a cookie file
is the prefered way of authenticating against a Bitcoin node. We will later tell our
Liquid node the location of this file so that it can authenticate against the Bitcoin node.

Alternatively, you can also use RPC parameters (``rpcuser``, ``rpcport``, and
``rpcpassword``) specified in the bitcoin.conf file as the authentication method. If you
want to use the RPC parameter method of allowing access, then also set the following
within bitcoin.conf.

*Note that the first value will start your Bitcoin node in "regtest" mode so that you can
develop against it - you can omit it if you want to start the node on the live Bitcoin
network*:

.. code-block:: text

	regtest=1
	regtest.rpcport=18888
	regtest.port=18889
	rpcuser=<your user>
	rpcpassword=<your password>

You may also want to include the ``prune`` parameter in your Bitcoin node settings. Pruned
mode reduces disk space requirements but will will not change the initial amount of time
required for download and validation of the chain.


elements.conf
=============

The elementsd, elements-qt and elements-cli applications will all use a configuration file
named elements.conf. The elements.conf file tells elementsd and elements-qt which network
to connect to and can set a number of different behaviours within the applications. It
also tells them what credentials must be provided in order to accept an RPC request. The
elements-cli application uses the configuration file to obtain the correct credentials in
order to communicate with elementsd or elements-qt using RPC. 

When you later start either of the three applications you can provide a ``datadir`` path.
The path you provide tells the applications which directory to use to:

* Obtain RPC authentication data (user, password, port).

* Store blockchain and wallet data.

* Store log files etc.

If you want to use a different data directory that the defaults referenced below, for
example an external hard drive, you can follow `this guide
<https://bitzuma.com/posts/moving-the-bitcoin-core-data-directory/>`_.

The elements.conf configuration file is located in the following places by default. If you
do not see the Liquid directory and the elements.config file you should create them now.
Otherwise, open the elements.conf file for editing.

**Linux**

``~/.elements/``

**Windows**

``%homepath%\AppData\Roaming\Elements``

**MacOS**

As the Library file is hidden, you can access it by opening Finder, selecting 'Go' then
'Go to Folder' from the menu and then entering the path as ``~/Library``. From there you
can go into the ``Application Support`` folder and create the ``Elements`` folder. Once
you have created the Elements folder, you can run the following from the Terminal app to
create the Liquid config file:

``touch "Library/Application Support/Elements/elements.conf"``


.. note::

	After making any changes to elements.conf in the future, you will need to restart your
	Liquid node so that they take effect.


If your Bitcoin node is installed in the default location, Liquid should automatically
find it when you later start it. If you use a non-default location for your Bitcoin node,
you will also have to add the following parameter to your elements.conf file, pointing to
the cookie file created by your Bitcoin node:

.. code-block:: text

	mainchainrpccookiefile=<location_of_your_bitcoin_datadir>

If you want to use the RPC parameter method of allowing access to your Bitcoin node then
also set the following within elements.conf, using the same user, password, and port that
you set in bitcoin.conf:

.. code-block:: text

	mainchainrpcport=<18888_for_example>
	mainchainrpcuser=<your_bitcoin_rpc_user_here>
	mainchainrpcpassword=<your_bitcoin_rpc_password_here>

If you want to allow your Liquid node to accept RPC requests (such as those used in the
:ref:`Developer Guide <developer-guide>`) then also set the following. 

*Note that these values will start your Liquid node in test/development mode. To start in
live Liquid network mode, set the chain value to liquidv1*:

.. code-block:: text

	chain=elementsregtest
	rpcuser=<your_liquid_rpc_user_here>
	rpcpassword=<your_liquid_rpc_password_here>
	elementsregtest.rpcport=<18884_for_example>
	elementsregtest.port=<18886_for_example>

.. tip::
	To switch between live and test/development modes you will need to change the ``chain``
	value between ``liquidv1`` (live) and ``elementsregtest`` (test/development). You must
	restart your node for these to take effect if you change them in the future. Be sure
	to also change the mode your Bitcoin node runs in if you do this.

If you do not wish to validate peg-ins against your Bitcoin node, you can set the
``validatepegin`` parameter to a value of ``0``. This can be done either in the
elements.conf file, or passed in as a command line parameter.

.. code-block:: text

	validatepegin=0

With this setting, you do not need to run a Bitcoin node as Liquid will not attempt to
connect to one on startup. 

.. warning::
	We advise against disabling peg-in validation unless you are aware of the implications
	, running in a testing environment, or are not dealing with large amounts of funds.  

A complete `Liquid configuration file template 
<https://github.com/ElementsProject/elements/blob/master/share/examples/liquid.conf>`_ can
be found here.


Running your Liquid Node
------------------------

Once you have completed the steps in the Configuration section you will be able to start
Liquid GUI or Liquid Daemon.

**Linux**

You will be able to run each of the applications from the command line within the folder
you extracted them to. For example:

.. code-block:: bash

	./elementsd

or

.. code-block:: bash

	./elements-qt

and 

.. code-block:: bash

	./elements-cli

Depending on your system set up, you may have to change the permissions on the files

before they will run.


**Windows**

You can run Liquid as a normal desktop application. It is worth noting that the actual
application will appear in your installed apps list as 'Elements Core'.


**MacOS**

If you installed Liquid from the dmg image, you can run it as a normal desktop application.
It is worth noting that the actual application will appear in your installed apps list as
'Elements Core'.


What next?
==========

You should now be set up to start using your node. 

You can connect it to the live Liquid network by setting ``chain=liquidv1`` and letting it
sync its local copy of the Liquid blockchain. 

You might also want to switch your Liquid node to test/development mode using
``chain=elementsregtest`` and start the :ref:`Developer Guide <developer-guide>` and
:ref:`App Examples <liquid-app-examples>` sections if you want to.

.. _quickstart_pegin:


Enabling Peg-in Validation
--------------------------

This section will show you how to enable your Liquid node to validate peg-ins against a
Bitcoin node. This is not mandatory, but it an important part of Liquid network security.

If you have not already installed and synced a Bitcoin node on your machine, you should
follow the instructions on `https://bitcoincore.org/en/download 
<https://bitcoincore.org/en/download/>`_.

When you have a fully synced Bitcoin node running on your machine, you need to enable it
to process requests from other applications, such as Liquid. To do this we need to add a
line to the bitcoin.conf config file. 


Changes to bitcoin.conf
=======================

.. |br| raw:: html

    <br>

1. Shutdown your Bitcoin node first before proceeding.
   |br| |br|

2. In order to edit Bitcoin's configuration file (bitcoin.conf) you can use the process in
   the :ref:`Configuring Liquid <quickstart_configuring>` to locate and edit config files
   for your given operating system, **remembering to replace the references to elements
   with bitcoin**. Use the following paths to locate and open the bitcoin.conf file,
   depending on your operating system:

   Linux: ``~/.bitcoin/``

   Windows: ``%homepath%AppDataRoaming/Bitcoin``

   MacOS: ``”Library/Application Support/Bitcoin”``
   |br| |br|

3. When you have located and opened the bitcoin.conf file, add the following line to it:

   ``server=1``

   If you use a non-default location for your Bitcoin node, you will also have to add the
   following parameter to your elements.conf file, so that Liquid knows the location of
   the cookie file created by your Bitcoin node:

   ``mainchainrpccookiefile=<location_of_your_bitcoin_datadir>``
   |br| |br|

4. Save and close the file and restart your Bitcoin node.


Changes to elements.conf
========================

1. Open the Liquid config file by following the steps in the :ref:`Configuring Liquid
   <quickstart_configuring>` section.
   |br| |br|

2. If your elements.conf file contains an entry of ``validatepegin=0``, replace the ``0``
   with a ``1``, and save and close the file. If your elements.conf file contains no value
   for ``validatepegin`` at all, you can close the file, no changes are needed.

Your Liquid node is now set up to validate peg-ins!


