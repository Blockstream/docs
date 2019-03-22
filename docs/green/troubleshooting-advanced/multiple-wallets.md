---
layout: default
title: Multiple Wallets and Devices
nav_order: 4
parent: TROUBLESHOOTING AND ADVANCED FEATURES
grand_parent: Green
--- 

## Managing a Single Wallet Across Multiple Devices

One of the many advantages of using Blockstream Green is that you get multi-platform support - you can access your wallet from different devices on different operating systems.

To do this, you need to first have a wallet created (on any one of our apps), and have the mnemonic carefully recorded.

Now, install any version of our app on the secondary device that you wish to use. Once it is installed, do not create a new wallet. Instead, you can restore your existing wallet by entering the mnemonic from your original wallet.

After this, the app will ask you to create a PIN. It is important to understand that this is not the same PIN that you use to enter your wallet in the original device. Each device can be set up with its own PIN. The mnemonic is common to all devices accessing the same wallet and coins, but each PIN is specific to the device it was created on.

Once you have done this, you can freely access your coins from either device, and make changes as you please. Also, any coins sent or received on one device will be reflected in all other devices connecting to that same wallet.

## Managing Multiple Wallets on a Single Device

It is also possible to use a single device and app to access different wallets created by the app’s user. Keep in mind, however, that this will mean carefully managing a separate 24-word mnemonic for each wallet. It is already imperative that you keep any mnemonic safely and secretly recorded, but managing multiple wallets will make very clear as to why!

Let’s imagine that you have created your first wallet.

Now, you want to create a separate wallet. To do this, you will need to erase your existing wallet data on that app with the new wallet you are going to create. (Your mnemonic is what allows you to recover your wallet after you have done this.)

You do this by clicking on “create new wallet” on the bottom of the “Onboarding” page, which will guide you through the new wallet setup process.

You will also need to use a separate email address, since Blockstream Green will only allow one wallet per email address.

Once you have generated the new wallet, you will have a separate mnemonic for that wallet, and your app will default to asking you for the PIN of that newly created wallet.

If you want to switch back to your old wallet, you will need to go back to the “”Onboarding” page and select the “Restore Green Wallet” option, and enter the mnemonic of your old wallet. Then you can recreate your PIN and access your original wallet again.

From now on, you will be prompted to enter the PIN of whatever wallet was last accessed on your app, and if you want to change to your other wallet you will have to enter those mnemonic again.

Every time that you re-enter your mnemonic to access a wallet, this will wipe the previously loaded wallet from the app, including its PIN. This is why you will need to generate a new PIN each time you enter your mnemonic.

Now, some of you may note that this isn’t all that convenient, having to re-enter your mnemonic each time you switch wallets.

That is correct; it’s not a function that we generally recommend our users to use. Instead, if you wish to keep several different accounts for different groups of coins, we recommend that you use our simple account feature.