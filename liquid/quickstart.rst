.. _quickstart:

Liquid Quickstart
*****************


Overview
--------

This guide will show you how to set up and run a Liquid node from scratch.


.. _quickstart_installing:

Installing and running Liquid
-----------------------------

**1. Downloading Liquid**

Download the Liquid application setup files from the `Liquid release page <https://github.com/ElementsProject/elements/releases/tag/elements-0.17.0>`_.

Ubuntu: ``liquid-0.17.0-x86_64-linux-gnu.tar.gz``

MacOS: ``liquid-0.17.0-osx-unsigned.dmg``

Windows: ``elements-0.17.0-win64-setup-unsigned.exe``

Raspberry Pi: ``liquid-0.17.0-arm-linux-gnueabihf.tar.gz``

**2. Installing Liquid**

Run the file you downloaded to install Liquid on your machine. 

MacOS users should note: When you double click the downloaded ".dmg" file it will appear on the left hand panel in Finder as "Elements-Core". If you select that then you will be able to drag the application into the Applications folder.


**3. Running Liquid**

MacOS users should note: The Liquid desktop application installed is named "Elements Core". 

.. Note:: When it first starts up Liquid will currently ask you for the data directory you want to use to store the Bitcoin blockchain data in. This is a GUI labeling error and it is actually referring to the Liquid blockchain data directory. 

Click OK to accept the default data directory.

The first time Liquid runs, it will create a Liquid data directory for you and, if you do not have a Bitcoin node running on your machine, notifies you that it "cannot get a valid response from the mainchain daemon". This just means that it cannot connect to a local Bitcoin node and is currently set to do so.

* **If you see the error message**: you can close it and move on to the :ref:`Configuring Liquid <quickstart_configuring>` section. 

* **If you do not see the error message**: you are already running a Bitcoin node and Liquid has located it. Your Liquid node will start its initial Liquid blockchain sync and you can skip the :ref:`Configuring Liquid <quickstart_configuring>` section, your Liquid node is now set up!

.. _quickstart_configuring:

Configuring Liquid
------------------

If you do not run a Bitcoin node on the same machine as your Liquid node, you can tell Liquid to not connect to it on startup. To do this we need to add a setting to Liquid's configuration file. Depending on the operating system you are using.


Linux
=====

As Liquid's data is stored in a hidden folder, the simplest way to create and edit the config file is to:

1. Open the Terminal app.

2. Copy and paste the following into Terminal and then press enter (the "touch" command creates the file if it doesn't already exist): ``touch ~/.liquid/liquid.conf``

3. Open the file for editing by copying and pasting the following line into the terminal and pressing enter: ``nano ~/.liquid/liquid.conf``

4. That will open the file in your default text editor. Paste the following into the empty file and then save changes before closing the file: ``validatepegin=0``

5. Continue to the :ref:`Running Liquid <quickstart_running>` section.


MacOS
=====

As Liquid's data is stored in a hidden folder, the simplest way to create and edit the config file is to:

1. Open the Terminal app from your Applications/Utilities menu.

2. Copy and paste the following in and then press enter (the "touch" command creates the file if it doesn't already exist): ``touch "Library/Application Support/Liquid/liquid.conf"``

3. Open the file for editing by copying and pasting the following line into the terminal and pressing enter: ``open -t "Library/Application Support/Liquid/liquid.conf"``

4. That will open the file in your default text editor. Paste the following into the empty file and then save changes before closing the file: ``validatepegin=0``

5. Continue to the :ref:`Running Liquid <quickstart_running>` section.


Windows
=======

To create and edit the Liquid config file:

1. Open File Explorer and in the location text box paste the following and press enter: ``%homepath%AppDataRoaming/Liquid``

2. Select the View tab in File Explorer and make sure the "File name extensions" check box is checked.

3. If there is no file already named liquid.conf, right click within the folder and select New and then Text Document and name the file ``liquid.conf``.

4. Open liquid.conf by double clicking on it. If asked what application should be used to edit ".conf" files, choose Notepad.

5. Paste the following into the empty file and then save changes before closing the file: ``validatepegin=0``

6. Continue to the :ref:`Running Liquid <quickstart_running>` section.


.. _quickstart_running:

Running Liquid
--------------

Now that you have added a line to the config file telling Liquid to not try and validate peg-ins against a Bitcoin node, you can start the Liquid application again as you did before. 

Your Liquid node should start downloading the Liquid blockchain data from other nodes on the network.

.. Note:: The :ref:`Configuring Liquid <quickstart_configuring>` section is for those who do not have a Bitcoin node running on their machine. It should be noted that connecting to a Bitcoin node allows Liquid to validating peg-in transactions and is an important part of Liquid network security. For this reason, it is recommended that once you have followed this guide and have your Liquid node up and running, you install and sync a Bitcoin node, then follow the steps in the :ref:`Enabling Peg-in Validation <quickstart_pegin>` section.

At the time of writing the 0.17 tagged release is the only release with binaries available, although this will change over time.



