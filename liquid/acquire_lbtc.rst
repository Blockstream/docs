.. _acquire_lbtc:

How to Acquire Liquid Bitcoin (L-BTC)
*************************************

Liquid Bitcoin (L-BTC) are required to perform most functions on the Liquid Network.

Although all L-BTC must be created on the Liquid Network via a peg-in,
there are a few different ways to obtain L-BTC so that you can start
transacting on the Liquid Network:

+---------+--------------+---------+---------------------------+
| Method  | Required     | Level   | Notes                     |
+=========+==============+=========+===========================+
| Bitcoin | Exchange     | Beginner| Exchanges with            |
| exchange| account      |         | L-BTC                     |
|         |              |         | available:                |
|         |              |         | `Bitfinex`_,              |
|         |              |         | `The Rock                 |
|         |              |         | Trading`_,                |
|         |              |         | `Sideshift AI`_,          |
|         |              |         | `BTSE`_.                  |
+---------+--------------+---------+---------------------------+
| Informal| `Blockstream | Beginner| Only suitable             |
| swaps   | Green`_      |         | for small                 |
|         |              |         | amounts of                |
|         |              |         | L-BTC due to              |
|         |              |         | risk; example             |
|         |              |         | venues: `Liquid           |
|         |              |         | Community                 |
|         |              |         | Telegram                  |
|         |              |         | group`_.                  |
+---------+--------------+---------+---------------------------+
| Peg-in  | :ref:`Liquid | Advanced| Requires                  |
|         | Core         |         | moderate                  |
|         | <quickstart>`|         | technical                 |
|         |              |         | proficiency;              |
|         |              |         | must wait 102             |
|         |              |         | Bitcoin blocks            |
|         |              |         | to complete the           |
|         |              |         | peg-in; need a            |
|         |              |         | Liquid member             |
|         |              |         | to peg-out.               |
+---------+--------------+---------+---------------------------+

.. Warning:: L-BTC peg-outs must be performed via a Liquid Member, e.g. Bitfinex. Don’t acquire L-BTC unless you are sure you have a method of converting them back to BTC.

Some guides are shown below, explaining:

-  :ref:`How to exchange BTC for L-BTC on Bitfinex <bitfinex_lbtc>`
-  :ref:`How to peg-in Liquid Bitcoin (L-BTC) with Liquid Core <pegin_lbtc>`

.. _Bitfinex: https://www.bitfinex.com/
.. _The Rock Trading: https://www.therocktrading.com/
.. _Sideshift AI: https://sideshift.ai/
.. _BTSE: https://www.btse.com/
.. _Blockstream Green: https://blockstream.com/green
.. _Liquid Community Telegram group: https://t.me/liquid_community

.. _bitfinex_lbtc:

How to Exchange BTC for L-BTC on Bitfinex
-----------------------------------------

Bitfinex provide a 1:1 BTC:L-BTC conversion service on their **Wallet**
page.

.. Note:: Bitfinex have a minimum withdrawal size of 20 USD equivalent and this method is `not available for US residents`_.

1.  Register an account at `Bitfinex`_.

2.  `Set up your account`_.

3.  Send bitcoin to your `Bitfinex “Exchange Wallet”`_ from a wallet of
    your choice.

4.  Wait for at least 3 confirmations.

5.  Go to your `Wallets`_ page. Your bitcoin should show in the
    **Balances** section under the **Exchange Wallet** column.

6.  Still on the **Wallets** page, enter the following details into the
    **Currency Conversions** section:
    ``Amount: <amount of bitcoin you would like to convert>  From: BTC  To: LBTC  From Wallet: Exchange  To Wallet: Exchange``
    |image0|

    Double-check the details are correct, then click **Convert**.

7.  At the bottom of the `Wallets`_ page, you should now see a new row
    for “LiquidBTC” and a balance under **Exchange Wallet**. You’ve
    acquired your first L-BTC!

8.  To withdraw L-BTC to a non-custodial wallet (e.g. Blockstream
    Green), go to your `Withdraw`_ page, scroll down and select on
    LiquidBTC.

9.  You’ll be taken to a **Withdraw > LiquidBTC** page.

10. Enter a receive address from your Liquid wallet, withdrawal amount,
    select **Exchange Wallet**, check that you have read the conditions,
    and then click **Request Withdrawal**.

    |image1|

11. Complete the withdrawal confirmation steps.

12. After Bitfinex have processed the withdrawal, the L-BTC will be
    settled in a prompt two minutes and you should see your balance
    updated in your Liquid wallet. Your funds will be immediately
    available for spending, no need to wait for multiple confirmations.

The process for converting L-BTC to BTC on Bitfinex is the same as above
but in reverse: deposit L-BTC, convert L-BTC to BTC, then withdraw your
BTC to a Bitcoin wallet.

.. _not available for US residents: https://support.bitfinex.com/hc/en-us/articles/115003461254-US-Residents-Frequently-Asked-Questions
.. _Bitfinex: https://www.bitfinex.com/
.. _Set up your account: https://support.bitfinex.com/hc/en-us/articles/115004405873-A-Beginner-s-Guide-to-Bitfinex
.. _Bitfinex “Exchange Wallet”: https://www.bitfinex.com/deposits/new/bitcoin
.. _Wallets: https://www.bitfinex.com/wallets
.. _Withdraw: https://www.bitfinex.com/withdraw

.. |image0| image:: https://i.imgur.com/gCOGdSe.png
.. |image1| image:: https://i.imgur.com/0eDygN6.png

.. _pegin_lbtc:

How to Peg-In Liquid Bitcoin (L-BTC) with Liquid Core
-----------------------------------------------------

This guide requires a moderate level of technical proficiency. Some
users may prefer to go use one of the exchange methods for a more
convenient solution.

.. Warning:: L-BTC peg-outs must be performed via a Liquid Member, e.g. Bitfinex. Don’t peg-in L-BTC unless you are sure you have a method of converting them back to BTC.

1.  `Download and install Bitcoin Core`_ (use `Node Launcher`_ for fast
    setup).

2.  Run and sync your Bitcoin node (could take a day or more).

3.  Follow the :ref:`Liquid Quickstart Guide <quickstart>`.

4.  :ref:`Enable peg-in validation <quickstart_pegin>`.

5.  Run and sync your Liquid node.

6.  In the Liquid Core client, open the console window by clicking
    Help/Debug Window -> Console tab.

7.  In the console, get a peg-in address using the following command.
    ``getpeginaddress``

8.  Save the **mainchain_address** and **claim_script** values for use
    later.

9.  Send Bitcoin to the **mainchain_address** and keep a copy of the
    transaction id returned.

10. Wait for 102 confirmations on the Bitcoin chain, which will take on
    average around 17 hours with a sufficient Bitcoin miner fee. You can
    track your transaction’s progress on `Blockstream Explorer`_.

11. Once the transaction has received 102 confirmations, go to your
    Bitcoin Core client, and open the console by clicking Help/Debug
    Window -> Console tab.

12. Enter the following two commands and record the results, you will
    need them to claim the peg-in on Liquid.

    ::

       getrawtransaction <yourTXID>

    ::

       gettxoutproof '["'<yourTXID>'"]'

13. Go back to the Liquid Core client and open the console window as before. Enter the
    following command, using the result from
    ``getrawtransaction <yourTXID>`` as ``<raw>`` and the result from
    ``gettxoutproof '["'<yourTXID>'"]'`` as ``<proof>``.

    ::

       claimpegin <raw> <proof> <claim_script>

14. The claim transaction should confirm in around two minutes. Once
    confirmed, you should see your L-BTC balance updated in your Liquid
    Core client.

Congratulations! You’re now the proud owner of some Liquid Bitcoin. These can be transferred to other Liquid users, exchanges and other businesses that support Liquid, or used to cover the transaction fee when creating and transferring Issued Assets.

.. _Download and install Bitcoin Core: https://bitcoincore.org/en/download/
.. _Node Launcher: https://github.com/lightning-power-users/node-launcher/releases
.. _Blockstream Explorer: https://blockstream.info
