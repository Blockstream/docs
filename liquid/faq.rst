FAQ
***

What is Liquid?
===============

Liquid is an implementation of a federated sidechain designed to complement the delayed transaction finality of transactions on the Bitcoin network with rapid, confidential, and secure transfers enabled by the Liquid Network. Liquid is built on Elements, Blockstream’s general-purpose blockchain protocol.
 

Who are the members of Liquid?
=============================

Liquid currently supports 35 network members.


What are the benefits of Liquid?
================================

- Speed - Liquid enables near-instant transfers between exchanges. Users can send amounts from their bitcoin balances between Liquid member exchanges who will execute transfers of L-BTC (BTC pegged on Liquid) on their behalf.
- Privacy - Liquid supports Confidential Transactions to prevent exposing transaction amounts to other members.
- Efficiency - Liquid helps market makers improve their capital efficiency by reducing balances held on multiple exchanges. Users can also directly withdraw L-BTC from exchanges to wallets that support Liquid.
- Reliability - Liquid builds on the time-tested Bitcoin codebase, but Liquid block times are one minute apart on average, compared to Bitcoin’s ten-minute average block time, because Liquid blocks are signed instead of mined.
- Security - Liquid allows traders to secure their assets with a multi-signature trust model while retaining the ability to quickly transfer to and withdraw from exchanges.
 

How can exchanges use Liquid?
============================

Liquid member exchanges can hold L-BTC to quickly credit user balances who transfer BTC to allow for instant trading without waiting for Bitcoin network transaction confirmations.
 

How can traders use Liquid?
===========================

Instead of risking funds by leaving large balances on exchanges to avoid missing favorable trade, Liquid allows traders to move L-BTC rapidly between wallets and exchanges.
 

What are Issued Assets?
=======================

Issued Assets are utility tokens, digital collectibles, attested assets, and cryptocurrencies issued by Liquid members through the Element protocol’s Confidential Assets feature. Example issued assets:
 

What are Confidential Transactions?
===================================

Confidential Transactions are a privacy feature that hide transaction amounts from everyone except transaction participants. Liquid transactions are publicly recorded on the network’s sidechain similar to Bitcoin transactions recorded on its blockchain, only transaction amounts are hidden. This minimizes front-running risks via blockchain analytics. Confidential Transactions also enable private solvency proofs without disclosing total reserve balances by only requiring proof of meeting a certain balance threshold.
 

Why is Liquid a federated sidechain?
====================================

Liquid’s tradeoffs allow for private and rapid asset transfer settlement at the expense of utilizing a permissioned network that relies on the trust of other network functionaries. This structure is better suited for trading environments compared to Bitcoin’s tradeoffs that offer permissionlessness, trustlessness, and censorship-resistance at the expense of a deterministic transaction flow and mining. As a sidechain, Liquid also leverages Bitcoin’s security.
 

How do I exchange BTC for L-BTC?
================================

Liquid users send BTC to addresses controlled by the Liquid Federation who credits the senders with L-BTC upon receiving 102 Bitcoin network confirmations. This is a peg-in. Users can also instruct Liquid network functionaries to pay BTC to their Bitcoin network addresses in exchange for L-BTC. This is a peg-out.
 

Can I run a Liquid full node?
=============================

Yes. Anyone can download the node software for a Liquid full node `here <https://github.com/elementsproject/elements/releases>`_.
 

What happens to the bitcoin held by the Federation if Liquid stops functioning?
===============================================================================

Liquid is equipped with emergency keys to return bitcoin held in the network to the last recorded owner. The keys are only active if the network is down for an extended period of time. The Liquid Federation will exhaust all other efforts to revive the network before using the emergency keys. Issued assets can be migrated to another platform or to a new Liquid-compatible blockchain, depending on the asset-issuer’s preference.
 

What is the difference between Lightning and Liquid?
====================================================

Liquid and Lightning are separate networks with distinct but complementary functions.

- Payment size: Lightning optimizes for micropayments by requiring payment routing between parties to execute a transaction, which makes larger transactions more difficult to complete. Liquid enables payments of any size at any time.
- Asset swaps: Liquid is a multi-asset network that supports natively issued tokens, which can make Liquid more useful for single-transaction atomic asset swaps than attempting atomic swaps via Lightning.

Blockstream eventually expects Lighting integration in Liquid to enable faster transfers and smaller payments.
 

Who can join Liquid?
====================

Liquid is designed for cryptocurrency exchanges, institutional investors, OTC desks, and large traders. Other cryptocurrency users can still use and benefit from Liquid via Liquid wallets and Liquid Network member exchanges.
 

Who controls Liquid?
====================

Liquid members operate the network. Blockstream only functions as a technology provider to Liquid members. The Liquid Network could continue to function indefinitely even if members decided to no longer receive support from Blockstream or if Blockstream dissolved.
 
How can I get more information?
===============================

To learn more about Liquid and inquire about joining the network, email `liquid@blockstream.com.`
