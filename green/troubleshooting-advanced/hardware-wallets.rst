----------------
Hardware Wallets
----------------

Green Wallets with Mnemonic on Hardware Wallet (Android only)
-------------------------------------------------------------

Blockstream Green multi-sig wallet can be used with hardware wallets. This gets you the
best of both worlds by granting you access to Blockstream Green multisig security and
features, while using your hardware wallet to safely store your own private key.

Currently, we support Ledger Nano S and both the Trezor ONE and the Trezor T hardware
wallets. You can see detailed instructions how to use each type below.

* This setup enhances your funds security. In fact, a Blockstream Green wallet associated
  with a hardware wallet creates a 2-of-2 multisig wallet where your key is held in cold
  storage on your hardware device. The other key will be held by the Blockstream Green
  service and serves to authorize transactions through Two-Factor Authentication.

* Your 2-of-2 Blockstream Green **is different** from any other existing single signature
  wallet you may have already created with your hardware wallet device with other hardware
  wallet managers.

* Your 2-of-2 Blockstream Green wallet **is also different** from any other 2-of-2 Green
  wallet that has a different mnemonic from the one generated and protected on your
  hardware wallet device.

.. raw:: html

   <iframe width="560" height="315" src="https://www.youtube.com/embed/nkQ_LXEuSVg"
   frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope;
   picture-in-picture" allowfullscreen></iframe>


New Wallet Set Up and Log In
----------------------------

0. To use a hardware wallet with Green, you will first need to initialize your hardware
   device as normal, according to the manufacturer’s instructions. When this process is
   finished, you should have safely recorded the hardware device’s mnemonic, and have a
   PIN to access the device.

1. Plug the hardware wallet into your Android device that has a Blockstream Green wallet
   installed, and open Blockstream Green. It doesn't matter if you open Green before or
   after plugging your hardware wallet.

2. Your device is going to detect your hardware wallet, and prompt you to select which app
   you want to open it with. Select Blockstream Green.

3. Enter your hardware wallet PIN:

   - through your smartphone by looking at your Trezor ONE keyboard disposition;
   - through the Ledger Nano S, enter the PIN and open the bitcoin app to login.

4. You will be logged into your 2-of-2 Blockstream Green wallet:

   - if you didn't have an existing Green wallet associated with this hardware wallet
     device, a new 2-of-2 wallet will be created for you.
   - if you had already logged into a Green (or GreenAddress) wallet with this hardware
     wallet device, you will automatically log back into it.


Using Blockstream Green with your Hardware Wallet
-------------------------------------------------

Log into your Green wallet as explained above_.

.. _above: troubleshooting-advanced-index.html#new-wallet-set-up-and-log-in

When sending transactions you will be asked to confirm those through your hardware wallet
device.

After this physical confirmation, you will also be required to provide a Two-Factor
authentication code (if any, which we strongly advise you to set up).

Restore a Green Wallet Created with Hardware Wallet
---------------------------------------------------

If you ever lose or break the hardware wallet device you normally use to log into your
Green wallet, you have two options to restore your Green wallet:

1. Restore on a new hardware wallet device the 24-words backup mnemonic phrase (which you
   have generated earlier_ at point 0. when you initialized your hardware wallet device).
   Follow the hardware wallet manufacturer instructions to restore the backup mnemonic.
   Then follow the same instructions provided above_, starting from point 1.
  
2. Restore by typing 24-words mnemonic (which you have generated earlier_ at point 0. when
   you initialized your hardware wallet device) via the ``Restore Green Wallet`` on the
   Onboarding screen.

.. _earlier: troubleshooting-advanced-index.html#new-wallet-set-up-and-log-in