--------------
Other Features
--------------

Fiat Pricing Source
-------------------

Your wallet will always display your balance in terms of BTC, but since the price of
Bitcoin can be volatile, we also display your wallet’s balance in terms of a fiat currency
of your choose.

This is the first setting that can be altered under “Settings”. You can choose which
currency you want (USD, EUR, CAD, JPY, etc) and which source you want use for the
conversion rate (LocalBitcoins, BitStamp, The Rock Trading, etc).

Keep in mind that this is not a live feed with up-to-the-minute pricing information. The
wallet will occasionally ping its source for the latest prices, and then use that
conversion rate for the next while. If you need to have access to live data to make fast
decisions, you will need to use a more direct source.


Bitcoin Denomination
--------------------

There are several different denominations of Bitcoin’s currency units, and you can choose
whichever one you prefer.

The default setting is simply ``BTC``, which is equal to one full unit of Bitcoin.

Next is ``mBTC`` (millibitcoin), which is 1/1,000th of a ``BTC``.

There is also ``µBTC`` (microbitcoin) which is worth 1/1,000,000th of a ``BTC``.

The last option, ``bits``, are identical to ``µBTC``, in all but name.

For example, let’s assume that someone has 0.045 BTC in their wallet. This is what would
be displayed depending on their preferred denomination:

.. table::
   :widths: auto
   :align: center
   
   ============  ===============
   Denomination  Displayed As
   ============  ===============
   BTC           0.045 ``BTC``
   mBTC          45 ``mBTC``
   µBTC          45,000 ``µBTC``
   bits          45,000 ``bits``
   ============  ===============

Keep in mind that this also affects the amounts you enter when you are sending coins, so
be careful to always check what symbol is displayed before sending coins. You don’t want
to accidentally send 1000x the amount you intended!


nLockTime
---------

Funds in your main Green account (or a multisignature 2of2 subaccount) require 2
signatures to be spent: one from you and one from Green. In order to protect you from
loss of access to your funds should Green become unavailable, Green automatically creates
pre-signed transactions which you can subsequently countersign to recover the funds to an
address controlled solely by you. These transactions are called nLockTime transactions
because they can only be spent and confirmed by the network after a specific period of
time.

If the service becomes unavailable, you simply wait until the specified expiry period (90
days by default), then sign and send the transaction using our open source recovery tool 
garecovery_.

.. _garecovery: https://github.com/greenaddress/garecovery

.. note:: From mobile, you need to set an Email Two-Factor authentication method in order
   to be able to receive the nlocktime.zip files.
   
   From the `desktop app`_ under Security -> Email, you can set an email where you receive
   the nlocktime.zip files without having to set any email Two-Factor authentication.

.. tip:: From the `desktop app`_ you can also customize the length of your expiry period.

.. _`desktop app`: https://github.com/greenaddress/WalletElectron/releases

There are pros and cons to making the expiry period longer or shorter. The longer it is,
the less frequently you need to do redeposits, which saves you fees. But this also means
that if you lose your 2FA, or if our servers become inaccessible, then you need to wait
longer to to unilaterally move your coins.

nLocktime is measured in blocks, which occur every 10 minutes on average, so our software
can estimate how long this will take in terms of days. This is, however, only an average,
so the expiry period have vary somewhat from the estimate.


Two-Factor threshold
--------------------

By default, sending transactions from a Blockstream Green wallet will require a user to
confirm using *Two-Factor authentication* (as long as there is at least one method
activated).

Two-Factor threshold
  By enabling a Two-Factor threshold, the owner of a wallet can unlock a certain amount of
  coins that can be spent without 2FA confirmation.

For example, let’s say that you have 200 mBTC in your wallet, and you want to make sure
that you don’t spend more than 35 mBTC before again .

You could go into your Two-Factor settings to “Two-Factor threshold”, press the it and set
it to 35 mBTC. This would allow you to spend up to a total of 35 mBTC without passing Two-
Factor authentication. Let’s say that you make 3 payments of 10 mBTC over the next week.
The fourth time you attempt to do this, the app will require you to pass 2FA (since you
will only have 5 mBTC left on your spending limit), which can act as a reminder that you
are getting close to your threshold.

You can always override or update the Two-Factor threshold from settings, but this will
require confirming using your 2FA if the threshold is higher than the existing one.


Segregated Witness
------------------

Segregated Witness (aka SegWit) was a crucial upgrade to the Bitcoin protocol from 2017,
and enables cheaper transaction fees, among other benefits.

SegWit is turned on by default in Blockstream Green wallets, and we strongly recommend you
to leave it as such. If you are using an older version of our app that did not have it on
by default, you may wish to turn it on.

It will require no change in behavior from you as a user - you can send and receive
transactions as normal. The only difference you may notice would be slightly lower fees.

As a brief description of how it works - SegWit generates a special type of receiving
address that makes it cheaper to spend from.

Because this requires spending from SegWit addresses, if you have been using your wallet
without SegWit and then you turn it on, your existing bitcoins in your wallet will still
pay non-SegWit fees.

But whenever you spend any bitcoins that have been received in your from from after SegWit
has been enabled, they will benefit from lower fees.


Changing Your Email Address
---------------------------

In your settings you can change the email address associated with your wallet for Two-
Factor authentication and for the nLockTime.zip files.

.. warning:: Never use a temporary email address and make sure it is one that you will
   retain access to indefinitely.

To `change your email address`_, you will need to switch off and on your Email Two-Factor
authentication.

.. _`change your email address`: ./troubleshooting-advanced-index.html#email

When you switch off your Email Two-Factor, you will need to confirm using one of your
active 2FA methods.

Next, as you switch the Email Two-Factor back on you will receive an email with a
confirmation code to your newly submitted email address. Once you successfully enter the
6-digit code, your new email address will have been set.


Deleting Your Wallet
--------------------

If you wish to remove your wallet and all associated data from our system, you will need
to do this from our `desktop app`_.

Once you have logged in, go to Settings, and then find the section marked “Delete wallet”.

Be sure that you have moved all coins out of your wallet before you delete it, since there
is no way to re-access it again afterward.

After you click the delete button, you simply need to confirm your 2FA, and then your
wallet will be permanently deleted, and all associated data pertaining to it will be
deleted from our servers.