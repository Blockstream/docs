Liquid Network FAQs
*******************

What is Liquid?
==============

Liquid is an implementation of a federated sidechain - a blockchain with different features, capabilities, and benefits than the Bitcoin blockchain.

The Liquid Network was built to address the particular needs of traders who use exchanges. It enables the rapid, confidential, and secure transfer of funds between participants, providing a solution to the inherent problem of delayed transaction finality on the Bitcoin network.

Liquid’s sidechain is built on the Elements codebase and uses Blockstream’s Strong Federation technology to support the 1-to-1 exchange of bitcoin between chains.


What are the benefits of Liquid? 
================================

- Faster Trading - Near instant bitcoin transfers between exchanges allow users to take advantage of arbitrage opportunities like never before.
- Enhanced Efficiency - Market makers can improve their capital efficiency by reducing balances held across multiple exchanges.
- Better Privacy - Liquid supports Confidential Transactions for bitcoin amounts as they are transferred between users in the system, which protects users from exposure.
- Superb Reliability - Built using the battle-tested Bitcoin codebase, Liquid software is highly reliable. Also, since Liquid uses signed blocks instead of mining, blocks are targeted to be one minute apart instead of the more variable ten minute blocks in Bitcoin.


Who are the members of Liquid?
==============================

The Liquid Network is comprised of a group of exchanges, traders, and financial institutions from nine countries across four continents. Currently, Liquid members include: Altonomy, Bitbank, Bitfinex, Bitmax, BitMEX, Bitso, BTCBOX, BTCTrader/BtcTurk, BTSE, Cobo, Coinone, Coinut, Crypto Garage, DGroup, DMM Japan, FRNT Financial, Gate.io, GOPAX (operated by Streami), Huobi, L2B Global, OKCoin, OpenNode, Poolin, Prycto, Sideshift AI, The Rock Trading, SIX Digital Exchange, TaoTao, Tilde, Unocoin, Xapo, XBTO, and Zaif.


How do end users/traders benefit from Liquid?
=============================================

Users and members can benefit from Liquid through the ability to send bitcoins between member exchanges, who can send Liquid Bitcoin (L-BTC: BTC pegged into the Liquid sidechain) on their behalf. End-users can also withdraw L-BTC from exchanges to wallets that support Liquid, such as Blockstream Green or withdraw from one exchange directly onto their Liquid deposit address at another Liquid member exchange. Other wallets plan to support Liquid in the future.


How do exchanges use Liquid to help traders move funds more quickly?
====================================================================

Exchanges may maintain a balance of L-BTC to do fast conversion from user deposits in BTC so users can immediately trade across exchanges using Liquid. For L-BTC deposits exchanges can receive deposits and credit their customers accounts within minutes, rather than waiting for unpredictable Bitcoin blocks.


How can traders use Liquid to reduce custodial risk?
====================================================

Traders often need to leave funds on an exchange for an extended period of time to avoid missing favorable trading opportunities. Liquid allows traders to move L-BTC into their own wallet while still being able to quickly move them back to an exchange in two minutes. This allows traders to use a multi-signature trust model to hold their assets rather than relying on a single exchange.


What are Issued Assets?
=======================

Liquid was built using the Elements blockchain platform and therefore has multi-asset issuance capabilities built in through the Confidential Assets feature. This allows anyone to create new assets for many purposes such as stablecoins, security tokens, utility tokens, and digital collectibles.


What are Confidential Transactions?
===================================

Liquid is designed to be auditable and open to any network participant, as well as to facilitate confidentiality between transacting parties. Like Bitcoin, Liquid transactions are viewable by everyone in the network, though Confidential Transactions hide transaction amounts from everyone except for the parties directly involved in the transaction itself. In doing so, it minimizes front-running risks that occur through blockchain analysis.

Confidential Transactions fundamentally provide privacy for any end user that requires it, regardless of reason. Use cases for this level of commercial transaction privacy will become more apparent as the cryptocurrency trading ecosystem matures, and larger market makers and financial institutions begin trading bitcoin across exchanges. They will want to move bitcoin between their accounts at different exchanges, without broadcasting the amounts to the general public, providing assurances that their trading strategies and holdings are not exploited by potential competitors.

Private solvency proofs are also possible using Confidential Transactions to remove the need to show the full amount held but instead show that a threshold is met in order to prove reserves are adequate. For example: user account balances can be shown to sum to the total assets held without revealing individual amounts or even what that sum is. The number of transaction participants is even hidden.

Liquid makes compliance easy, whilst providing privacy of transactions. This would be impossible without the use of Confidential Transactions.


Why is Liquid a federated sidechain?
====================================

The Bitcoin blockchain excels at offering the permissionless, trustless, and censorship-resistant transfer of value. However, achieving those ends comes at the expense of a deterministic transaction flow and with the expense of mining. The Liquid sidechain provides a different set of trade-offs that allow privacy and rapid finality of asset transfer at the expense of being a permissioned network relying on trust of the functionaries.

Extending the Bitcoin protocol using a sidechain allows the main chain to remain secure, specialized and robust whilst its underlying properties support the smooth operation of the sidechain.

Whereas Bitcoin is secured by proof of work, assets on the Liquid blockchain are secured by a Strong Federation of trusted functionaries. This allows transactions on Liquid to reach a state of finality faster and more reliably than those on the Bitcoin blockchain.


How does BTC become L-BTC and vice versa?
=========================================

Users of Liquid are able to send bitcoins to an address controlled by the Liquid Federation; these users will be credited with L-BTC once the mainchain transaction receives 102 confirmations. This process is known as a peg-in. A high threshold of confirmations is needed to ensure that the network stays solvent in case of mainchain reorganizations. Members of Liquid should maintain a portion of their funds as L-BTC based on user demand for L-BTC and can rebalance as needed. If a user wishes to move L-BTC back to BTC, they can make a peg-out transaction in Liquid that will instruct the functionaries to pay to their mainchain address. Only members of the Liquid network are able to peg-out L-BTC.


Can I run a Liquid full node?
=============================

Yes. Anyone can run a full node for Liquid. Download the node software here.


What happens to the bitcoin held by the federation if Liquid stops functioning?
===============================================================================

There are emergency keys that are able to access the bitcoin held by the network only if the network is down for an extended period of time. The keys are strictly powerless otherwise. All efforts will be made to revive the network before using these emergency keys. These keys will allow the bitcoin held by the Liquid Network to be returned to the last owner on the Liquid blockchain. Issued Assets can be migrated to another platform, or a new blockchain compatible with Liquid, depending on the preferences of the issuer of the asset.


What's the difference between Lightning and Liquid?
===================================================

Lightning requires routes between the parties of a transaction to exist before the transaction. This makes Lightning very well suited for micropayments. However, this approach present challenges for larger payments that may be made between arbitrary parties, as a route might not always be available. Liquid allows parties to send funds of any size at any time. Liquid also supports multiple assets, which can make it useful for single-transaction atomic swaps that require no setup. Blockstream is a strong believer in both Liquid and Lightning, but for different use cases.

Due to being based on Bitcoin, Liquid already supports Lightning transactions with L-BTC, allowing even faster transfers and smaller-value payments. Lightning support for Issued Assets will also be coming in the future.


Who can join Liquid?
====================

Liquid membership is intended for cryptocurrency exchanges, OTC desks, large traders, and financial institutions. Smaller traders and individual users will still be able to use Liquid through member exchanges, running a Liquid full node, or Liquid-enabled wallets such as Blockstream Green.


Who controls Liquid?
====================

Liquid is operated by its members. Blockstream has no control over the network and only serves as a technology provider to the network members. Liquid can continue indefinitely even if Blockstream ceased to exist or if the members no longer wanted to receive support from Blockstream.


How can I get more information to join the network?
===================================================

If you are interested in joining the Liquid Network as a federation member, please send an email to `liquid@blockstream.com <mailto:liquid@blockstream.com>`_. Businesses and traders just looking to get started using Liquid should `download Blockstream Green <https://blockstream.com/green>`_ to set up a Liquid wallet, or `download Elements Core <https://github.com/ElementsProject/elements/releases>`_ to set up a Liquid full node, then `pick up some L-BTC <https://docs.blockstream.com/liquid/acquire_lbtc.html>`_.
