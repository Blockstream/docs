---
layout: default
title: 2FA
nav_order: 2
parent: TROUBLESHOOTING AND ADVANCED FEATURES
grand_parent: Green
--- 

## Table of Contents

- [Best 2FA Practices](#best-2fa-practices)
- [Changing Your 2FA Settings](#changing-your-2fa-settings)
   - [Email 2FA](#email-2fa)
   - [SMS 2FA](#sms-2fa)
   - [Phone 2FA](#phone-2fa)
   - [Google Authenticator 2FA](#google-authenticator-2fa)
- [Resetting Your Lost 2FA](#resetting-your-lost-2fa)

___

# Best 2FA Practices

Properly managing your Two Factor Authentication (from here on referred to as 2FA) is one of the most important parts of using your Blockstream Green wallet, so we strongly recommend that you follow all the guidelines outlined here.

2FA is the security method you use to authorize our server to provide the second signature for transactions. This means that in order for someone to steal coins from your wallet, they would need to steal your private key (by learning your mnemonic, or logging into your device with your PIN), and gain access to at least one of your 2FA methods.

This is how Blockstream Green offers a higher level of security over other single-signature wallets, which are compromised the moment someone else learns their mnemonic.

This means, however, that if you lose access to your 2FA methods, your coins can get stuck for long periods of time.

For best practices, we strongly urge our users to:

Set up your 2FA methods as soon as you set up your wallet.
Only use a permanent email or phone number that you will retain access to indefinitely.
Set up at least two different 2FA methods. This allows you to retain access even if one of your methods fails.

Many of the most common problems that users of Blockstream Green face can be avoided by follow those basic practices.


# Changing Your 2FA Settings

Your 2FA settings are all managed through the “Settings” page, under the security section.

Here, you will find 4 different 2FA options:

* Email
* SMS
* Phone
* Google Authenticator

To set up a new 2FA method, you choose your preferred method, and toggle the switch to turn it on.

If you already had another 2FA method already set up, you will need to use it to confirm that you wish to make a change.

Depending on the method you choose, you will then need to enter the relevant information about your chosen 2FA, and then input your first 2FA code.

As long as at least one 2FA method is active, you will always need to provide 2FA confirmation for further changing your 2FA, or for sending any transactions.


## Email 2FA

Email is a very simple 2FA option to set up. You first need to have entered your email in the relevant field elsewhere in the setting section.

Then you just need to toggle this selection to on.

To activate this method, toggle the email option under settings->Two-Factor Authentication to activate it. If you have any other 2FA methods already activated, you will first need to confirm with one of them that you wish to add a new 2FA method. Once it is enabled, you can confirm any 2FA-secured action by entering the 6-digit code sent to your inbox.

Emailed confirmation codes expire after 5 minutes, so be sure to check your email and enter them soon after they arrive.

If you aren’t receiving your emails, be sure to check your spam folder, or otherwise contact your email provider in case they are being blocked somehow.

As a final point, you may only use an email address with a single wallet with Blockstream Green. This means that if you already have an existing wallet, you cannot create a new using the same email. If you have an old and unused wallet connected to an email that you wish to use, then you can either change its email address, or delete the wallet entirely, which will free up the email to be used.

Those are the only methods for freeing up an email address, so if you can no longer access the wallet (due to losing the mnemonic), then you will be unable to use that email address for any future wallets.


## SMS 2FA

You can also have 2FA codes sent to your phone by SMS message.

To activate this method, toggle the SMS option under settings->Two-Factor Authentication to activate it. If you have any other 2FA methods already activated, you will first need to confirm with one of them that you wish to add a new 2FA method.

Next, you will need to enter your phone number. Be sure to include the country code, and do not include any spaces or special characters.

After submitting your number, we will send a 6-digit code by SMS to confirm that it is working properly. After you confirm this number, your SMS 2FA method will be activated.

Once SMS 2FA is enabled, you can confirm any 2FA-secured action by entering the 6-digit code sent to your phone by SMS. Each SMS confirmation code expires after 5 minutes, so be sure to check your phone and enter it soon after it arrives.

In general, it is a good idea to keep more than one 2FA method activated, but this is especially important if SMS is one of your methods. This is because it is prone to more problems than other methods due to potential issues on the end of the phone carrier, connectivity issues in certain regions, and other issues, making it the least reliable 2FA method.

From time to time, users suddenly lose access to their SMS 2FA method for a variety of reasons beyond our control (SMS routing issues, service provider changes, etc), but there are several things you can do to reduce the chances of this happening:

For obvious reasons, never use a temporary phone number. Also, only set up your SMS 2FA method from the location that you plan on usually using it from. It has happened that users have activated their SMS 2FA while traveling, only to find that routing issues prevent them from using it when they return home.


## Phone 2FA

The final 2FA option is to have our service call your phone to read out the confirmation numbers. It functions similarly to SMS 2FA.

To activate this method, toggle the Phone option under settings->security->Two-Factor Authentication to activate it. If you have any other 2FA methods already activated, you will first need to confirm with one of them that you wish to add a new 2FA method.

Next, you will need to enter your phone number. Be sure to include the country code, and do not include any spaces or special characters.

After submitting your number, you will receive a phone call at the number to inform you of the 6-digit code. Once you confirm this number, your Phone 2FA method will be activated.

For similar reasons to SMS 2FA, please make sure that you keep at least one other 2FA method activated at all times if you are using Phone as your primary method.

## Google Authenticator 2FA

The second available 2FA method is using Google Authenticator.

To set up this method, you will first need to download the authenticator app on Android or iOS.

Next, toggle the Google Authenticator option under settings->Two-Factor authentication to activate it. If you have any other 2FA methods already activated, you will first need to confirm with one of them that you wish to add a new 2FA method.

Then, you will be prompted with a QR code, which you can scan using your Google Authenticator app. Alternatively, you can also manually enter the code.

Once this happens, your authenticator app will automatically and continuously generate 6-digit codes that can be entered.

One of the main advantages of using this as your 2FA method is that you do not need to wait for a message or a call in order to enter the code. Your app will constantly generate them.

Be aware that if you lose your only device with your Google Authenticator app, you can get permanently locked out of this method. This is one of the many reasons we suggest that users keep at least two different 2FA methods activated at all times.

Due to this risk, we also encourage users to backup the QR code and/or the alphanumeric code that is shown in the wallet when you set it up. This allows you to restore Google Authenticator 2FA on another device, using the Google Authenticator App, should you lose access to your first 

If you find yourself having trouble with your 2FA (for example, the codes are not working), then please check this support page, as the problem can usually be solved using those suggestions.

# Resetting Your Lost 2FA

For a variety of reasons, users can accidentally lose access to a 2FA method of theirs. As long as you retain access to at least one other 2FA method, though, you will be able to disable the lost 2FA method, and recreate it under your control.

This why we strongly recommend having at least two different methods enabled and accessible at all times.

Unfortunately, some users do not follow best practices and will lose their only 2FA (or in extreme circumstances, may be exceptionally unlucky and lose access to multiple 2FA methods at the same time).

When this happens, there are two available methods. The first is to wait for the redeposit period to expire and then use our garecovery tool to send your coins to another wallet.

The other option is to use our 2FA reset feature, which can be done from the “Settings” page, in security section.

You start the reset process by entering an email address, which you must confirm.

Once you have done this the waiting period will begin. The waiting period is at least 1 year long (see below), and a countdown in days will be displayed in your wallet’s settings.

If the waiting period goes by without interruption by the wallet’s owner, then the 2FA settings will be set to “disabled” except for email 2FA, which will be set to the reset email address. Then you can again use your wallet as normal, including setting up new 2FA methods.

The long waiting period is a crucial security measure, in case someone temporarily accessed their wallet and tried to use the reset feature to steal their coins. The waiting period gives the rightful owner of the wallet time to log in and access their wallet to cancel the 2FA reset.

The exact formula for the waiting period is whichever of these two is longer:


* 365 days after the reset is triggered, OR
* 365 days plus your nLockTime period after the your wallet’s last transaction

For example, if you have your nLockTime period set to 30 days, and your last transaction was 40 days ago, then the waiting period will be 365 days. But if you have your nLockTime period set to 90 days, and you sent a transaction 50 days, ago, then the waiting period will be 405 days (365 days + 40 days left on your nLocktime).
