.. _swap_tool:

Liquid Swap Tool
****************

Liquid Node Preparation
-----------------------

If you have not already installed and run Liquid on your machine, follow the :ref:`Liquid Quickstart Guide <quickstart>` before continuing.

Once you have Liquid installed and running on your machine, you need to tell Liquid to allow connections from other applications, such as the Liquid Swap Tool.

To do this, follow the process in :ref:`Configuring Liquid <quickstart_configuring>` to locate and edit the Liquid config file and add the following line to the liquid.conf file:

``server=1``

After you have saved the change to the liquid.conf file you can start your Liquid node and it will be ready to accept connections from the Liquid Swap Tool.


Downloading and running Liquid Swap Tool
----------------------------------------

After you have prepared your Liquid node to accept connections from other apps, you can download the Liquid Swap Tool binaries from `https://github.com/Blockstream/liquid-swap/releases <https://github.com/Blockstream/liquid-swap/releases>`_.

To run Liquid Swap Tool using the binaries, please note the instructions for your operating system below.

Linux
=====

.. |br| raw:: html

    <br>

1. Download and extract the files.
|br| |br| 
2. Run the tool from the extracted file location using the command line:

   ``liquidswap-gui``
   
   or
   
   ``liquidswap-cli``


MacOS
=====

1. Download the .zip file and double click to extract the contents.
|br| |br| 
2. Open the newly extracted folder and double click the ".dmg" file and, when prompted, drag the Liquid Swap Tool to the Applications folder.
|br| |br|
3. Go to the Applications folder and double click the Liquid Swap Tool to run it. If you get an error saying the file is not from the App Store:

   3.1 Open System Preferences then Security & Privacy and then click on the General tab.

   3.2 Click the "Open Anyway" button next to the mention of Liquid Swap Tool.

   3.3 A new window will open allowing you to select "Open the Liquid Swap Tool".


Windows
=======

1. Download the installation file and double click to run the Liquid Swap Tool setup.
|br| |br|
2. Run the installed Liquid Swap Tool from the Programs menu.


Using Liquid Swap Tool
----------------------
Once started, follow the instructions on the `Liquid Swap Tool repository <https://github.com/Blockstream/liquid-swap/>`_ to perform a Liquid asset swap.


Wallet Compatibility Notice
---------------------------

Please note that the Liquid Swap Tool is not compatible with some older Liquid wallet versions. If you receive an error informing you that you have an "unsupported wallet version" you will need to back up your Liquid wallet.dat file, generate a new wallet and send funds from the old wallet to the new wallet before opening the Liquid SwapTool again. Please ensure you follow accepted processes for doing this in order to prevent loss of funds.

