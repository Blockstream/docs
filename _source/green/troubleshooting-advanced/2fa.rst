-------------------------
Two-Factor Authentication
-------------------------

Best Two-Factor Authentication Practices
----------------------------------------

Properly managing your Two-Factor Authentication is one of the most important parts of
using your Blockstream Green wallet, so we strongly recommend that you follow all the
guidelines outlined here.

.. attention:: Two-Factor authentication is the security method you use to authorize our
   server to provide the second signature for transactions. This means that in order for
   someone to steal coins from your wallet, they would need to steal your private key (by
   learning your mnemonic, or logging into your device with your PIN), and gain access to
   at least one of your Two-Factor authentication methods.

This is how Blockstream Green offers a higher level of security over other single-
signature wallets, which are compromised the moment someone else learns their mnemonic.

This means, however, that if you lose access to your Two-Factor authentication methods,
your coins can get stuck for long periods of time (~90 days by default).

For best practices, we strongly urge our users to:

.. tip::
   1. Set up your Two-Factor authentication methods as soon as you set up your wallet.
   2. Only use a permanent email or phone number that you will retain access to
      indefinitely.
   3. Set up at least two different Two-Factor authentication methods. This allows you to
      retain access even if one of your methods fails.

Many of the most common problems that users of Blockstream Green face can be avoided by
following those basic practices.


Changing Your Two-Factor Authentication Settings
------------------------------------------------

Your Two-Factor authentication settings are all managed through the “Settings” page, under
the Two-Factor section.

.. tip:: **Set up new Two-Factor authentication method**:
   
            1. Choose your preferred method (Email_, SMS_, Phone_ `Google Authenticator`_) and 
               toggle the switch on to enable it;
            2. Input your first 6-digits *Two-Factor authentication* code.

.. _`disable and change`:

.. tip:: **Change an existing Two-Factor authentication method**:

            1. Switch off the *Two-Factor* method you wish to change;
            2. Confirm the action by providing a *Two-Factor* 6-digits code of any active
               method;
            3. Switch back on the *Two-Factor* method you wish to change.

Here, you will find 4 different Two-Factor authentication methods:

- Email_
- SMS_
- Phone_
- `Google Authenticator`_

.. note:: **If you already had another Two-Factor authentication method set up**, you will
   need to use it to confirm that you wish to make a change.
   
   As long as at least one Two-Factor authentication method is active, you will always
   need to provide Two-Factor authentication confirmation for further changing your Two-
   Factor authentication methods settings, or for sending any transactions.


Email
'''''

Email is a very simple Two-Factor authentication option to set up.

Toggle the email option under settings->Two-Factor Authentication to activate it.

.. note:: If you have any other Two-Factor authentication methods already activated, you
   will first need to confirm with one of them that you wish to add a new Two-Factor
   authentication method.

Once it is enabled, you can confirm any Two-Factor-authentication-secured action by
entering the 6-digit code sent to your inbox.

Emailed confirmation codes expire after 5 minutes, so be sure to check your email and
enter them soon after they arrive.

If you aren’t receiving your emails, be sure to check your spam folder, or otherwise
contact your email provider in case they are being blocked somehow.


SMS
'''

You can also have Two-Factor authentication codes sent to your phone by SMS message.

Toggle the SMS option under settings->Two-Factor Authentication to activate it.

.. note:: If you have any other Two-Factor authentication methods already activated, you
   will first need to confirm with one of them that you wish to add a new Two-Factor
   authentication method.

Next, you will need to enter your phone number.

.. attention:: Be sure to include the country code, and do not include any spaces or
   special characters.

After submitting your number, we will send a 6-digit code by SMS to confirm that it is
working properly. After you confirm this number, your SMS Two-Factor authentication method
will be activated.

Once SMS Two-Factor authentication is enabled, you can confirm any Two-Factor-
authentication-secured action by entering the 6-digit code sent to your phone by SMS.
Each SMS confirmation code expires after 5 minutes, so be sure to check your phone and
enter it soon after it arrives.

.. caution:: In general, it is a good idea to keep more than one Two-Factor authentication
   method activated, but this is especially important if SMS is one of your methods.

This is because it is prone to more problems than other methods due to potential issues on
the end of the phone carrier, connectivity issues in certain regions, and other issues,
making it the least reliable Two-Factor authentication method.

From time to time, users suddenly lose access to their SMS Two-Factor authentication
method for a variety of reasons beyond our control (SMS routing issues, service provider
changes, etc), but there are several things you can do to reduce the chances of this
happening:

- For obvious reasons, never use a temporary phone number.
- Also, only set up your SMS Two-Factor authentication method from the location that you
  plan on usually using it from. It has happened that users have activated their SMS Two-
  Factor authentication while traveling, only to find that routing issues prevent them
  from using it when they return home.


Phone
'''''

The third Two-Factor authentication option is to have our service call your phone to read
out the confirmation numbers. It functions similarly to SMS Two-Factor authentication.

Toggle the Phone option under settings->Two-Factor Authentication to activate it.

.. note:: If you have any other Two-Factor authentication methods already activated, you
   will first need to confirm with one of them that you wish to add a new Two-Factor
   authentication method.

Next, you will need to enter your phone number.

.. attention:: Be sure to include the country code, and do not include any spaces or
   special characters.

After submitting your number, you will receive a phone call at the number to inform you of
the 6-digit code. Once you confirm this number, your Phone Two-Factor authentication
method will be activated.

.. caution:: For similar reasons to SMS Two-Factor authentication, please make sure that
   you keep at least one other Two-Factor authentication method activated at all times if
   you are using Phone as your primary method.

Google Authenticator
''''''''''''''''''''

The final available Two-Factor authentication method is using Google Authenticator.

To set up this method, you will first need to download the authenticator app on Android or
iOS.

Toggle the Google Authenticator option under settings->Two-Factor authentication to
activate it.

.. note:: If you have any other Two-Factor authentication methods already activated, you
   will first need to confirm with one of them that you wish to add a new Two-Factor
   authentication method.

Then, you will be prompted with a QR code, which you can scan using your Google
Authenticator app. Alternatively, you can also manually enter the code.

Once this happens, every 30 seconds your authenticator app will automatically generate
6-digit codes that can be entered.

One of the main advantages of using this as your Two-Factor authentication method is that
you do not need to wait for a message or a call in order to enter the code. Your app will
constantly generate them.

.. warning:: If you lose your only device with your Google Authenticator app, you can get
   permanently locked out of this method.

This is one of the many reasons we suggest that users keep at least 2 different Two-Factor
authentication methods activated at all times.

.. tip:: Due to this risk, we also encourage users to backup the QR code and/or the
   alphanumeric code that is shown in the wallet when you set it up.
 
This allows you to restore Google Authenticator Two-Factor authentication on another
device, using the Google Authenticator App, should you lose access to your first.

If you find yourself having trouble with your Two-Factor authentication (for example, the
codes are not working), then please check this support page, as the problem can usually be
solved using those suggestions.

Resetting Your Lost Two-Factor Authentication
---------------------------------------------

.. danger:: Two-Factor reset is an option of last resort for users that accidentally lose
   access to **all** of their Two-Factor authentication methods.
   
   It will require **at least 1 year** before being able to spend the funds with the new
   Two-Factor authentication email.

.. tip:: As long as you retain access to at least one other Two-Factor authentication
   method, you will be able to `disable and change`_ the lost Two-Factor authentication
   method, and recreate one under your control.
   
   An alternative to re-gain control of your funds is to wait for the redeposit period to
   expire and then use our garecovery_ tool to send your coins to another wallet.

.. _garecovery: https://github.com/greenaddress/garecovery

This why we strongly recommend having at least two different methods enabled and
accessible at all times.

Unfortunately, some users do not follow best practices and will lose their only Two-Factor
authentication (or in extreme circumstances, may be exceptionally unlucky and lose access
to multiple Two-Factor authentication methods at the same time).

**Resetting is advisable if, and only if, all of the following are applicable to you**:

- If you lost access to all of your Two-Factor authentication methods;
- If you do not feel confident in using the garecovery_ tool;
- If you do not possess the `nLockTime.zip files`_.

.. _`nLockTime.zip files`: ./troubleshooting-advanced-index.html#nlocktime

The Two-Factor reset feature, can be found in the “Settings” page, under the Two-Factor
section.

You start the reset process by entering an email address, which you must confirm.

.. Warning:: Once you have done this the waiting period will begin. The **waiting period
   is at least 1 year long** (see below), and a countdown in days will be displayed in
   your wallet’s settings.

If the waiting period goes by without interruption by the wallet’s owner, then email
Two-Factor authentication will be enabled and set to the reset email address. Then you can
again use your wallet as normal, including setting up new Two-Factor authentication
methods.

The long waiting period is a crucial security measure, in case someone temporarily
accessed their wallet and tried to use the reset feature to steal their coins. The waiting
period gives the rightful owner of the wallet time to log in and access their wallet to
dispute the Two-Factor authentication reset.

The exact formula for the waiting period is whichever of these two is longer:

- 365 days after the reset is triggered, OR
- 365 days plus your nLockTime period after the your wallet’s last transaction

For example:
  i. you have your nLockTime period set to 30 days, and your last transaction was 40 days
  ago, then the waiting period will be 365 days.
 
  ii. you have your nLockTime period set to 90 days, and you sent a transaction 50 days,
  ago, then the waiting period will be 405 days (365 days + 40 days left on your
  nLocktime).