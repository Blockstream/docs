.. _to-confidential-transactions:

-------------------------
Confidential Transactions
-------------------------

All addresses in Liquid are, by default, blinded using Confidential Transactions. Blinding is the process by which the amount and type of asset being transferred is cryptographically hidden from everyone except the sending and receiving parties. This is done using a blinding key, which we will look at later.

Imagine that our cryptographic friends Alice and Bob are running Liquid nodes. We'll have Bob send some assets to himself using a blinded Liquid address as the destination.

First Bob will generate a new address and store it in a variable named "ADDR" so he can recall it for use later:

.. code-block:: bash

	Bob:~$ ADDR=$(liquid-cli getnewaddress)

To see the new address Bob created he can print out the value that was stored in the "ADDR" variable.

.. code-block:: bash

	Bob:~$ echo $ADDR

.. tip:: We will use the "store the value returned from an RPC command in a variable for future use" technique throughout the rest of the code examples. We'll print out the contents of a variable every now and again when highlighting something that has been returned. You can always just echo out the contents of any others as we go along should you want to. 

After running the echo command above you should see something similar to this:

.. code-block:: bash

	Azpr7BwzjwdiB1pNZKcLkk6Esn5NWAE7wtrC4UzEsshpKe3eUZzPQBvfJ7q9wzJLbt9yn8hYZmZDayGG

.. tip:: As of Liquid v0.17, getnewaddress defaults to creating P2SH-P2WPKH addresses. You can create "CTE" prefixed addresses (the default for versions of Liquid previous to 0.17) by calling getnewaddress like this: ``liquid-cli getnewaddress "" legacy``. You can also set the ``addresstype=legacy`` argument on node startup, or set it in your config file to always get legacy addresses from "getnewaddress". Some commands require a "legacy" style address in order to work, such as message signing.

Let's look at the address in more detail to check that it is indeed a confidential one. To do this we can use the "getaddressinfo" command, passing in the address that we stored in the ADDR variable as a parameter:

.. code-block:: bash

	Bob:~$ getaddressinfo $ADDR

You should see a long value for the "confidential_key" property within the JSON formatted results. It will look something like this:

.. code-block:: text

	"confidential_key": "030788da8d9ca229cbe57e346daaf8d94cba3ed548b41922a8abefaec91ff1abb1"

The confidential_key is the public blinding key, which has been added to the address and is the reason why a confdential address is so long. You will also see that the "getaddressinfo" command shows an associated "unconfidential" address, which can be used to receive assets if you don't want to make use of the Confidential Transaction feature for some reason.

We'll now send an amount of 1 "bitcoin" (L-BTC) from Bob's wallet to the new address we generated for him:

.. code-block:: bash

	Bob:~$ TXID=$(liquid-cli sendtoaddress $ADDR 1)

In order to have the transaction confirm we need a block to be generated. As an aside, at this stage we can query the mempool of each of our Liquid nodes to see the transaction waiting to be added to a block, as well as the current block count of each node's blockchain:

.. code-block:: bash

	Alice:~$ liquid-cli getrawmempool
	Bob:~$ liquid-cli getrawmempool
	Alice:~$ liquid-cli getblockcount
	Bob:~$ liquid-cli getblockcount

Both should display results including the transaction with the same ID as that stored in the "TXID" variable and the same block count value. Once a new block has been created, we can check the mempool again for each client to see that the transaction has been confirmed:

.. code-block:: bash

        Bob:~$ liquid-cli generatetoaddress 1 $(liquid-cli getnewaddress)
	Alice:~$ liquid-cli getrawmempool
	Bob:~$ liquid-cli getrawmempool
	Alice:~$ liquid-cli getblockcount
	Bob:~$ liquid-cli getblockcount

Note that although Bob sent an amount of 1 L-BTC to himself the net effect is that he now has slightly less than he did before, this is because some of the transaction amount was spent on fees that have yet to mature and be seen as spendable. 

The above shows that the client's blockchains and mempools are in sync. If they are not, wait a few seconds and try the calls above again as it may take a moment for the nodes to synchronize. They display the same results because they are connected nodes on the same Liquid network and broadcast transactions and blocks between each other in very much the same was as Bitcoin nodes do.

Now let's examine the transaction as it is seen by Bob's wallet and also how it is seen from the point of view of Alice's wallet. First the view from Bob's wallet:

.. code-block:: bash

	Bob:~$ liquid-cli gettransaction $TXID

The output from that initially looks like just a huge random assortment of letters and numbers (the hex value of the transaction), but if you scroll up you will see some more readable content above that.

Looking in the "details" section near the top, you will see that there are two amount values:

.. code-block:: text

	"details": [
	  {
	    ...
	    "category": "send",
	    "amount": -1.00000000,
	    ...
	  },
	  {
	    ...
	    "category": "receive",
	    "amount": 1.00000000,
	    ...
	  }
	]

And so we can confirm that Bob's wallet can view the actual amounts being sent and received in this transaction. This is because the blinded transaction was sent from Bob's own wallet and so it has access to the required data to unblind the amount values. You will also see two other properties and their values within the two details sections: "amountblinder" and "assetblinder". These indicate that both the asset amount and the type of asset were blinded. This ensures that wallets without knowledge of the blinding key are prevented from viewing them.

Looking at the transaction from Alice's wallet, we would expect both amount and type to be unknown as they were sent using a Confidential Transaction. 

In order to check Alice's view of the transaction, we need Alice's node to use the value of the transaction id that we stored in the TXID variable in Bob's terminal session. When our code examples use a variable that was set by the other node like this, we will assume that you will set the variable across terminal sessions. This can be done by using echo to print the value within one terminal session, copying the value, and then setting it within the other node's terminal session, like so:

.. code-block:: bash

	Bob:~$ echo $TXID

Copy the result, which will be similar to:

.. code-block:: bash

	533533c5a382ccf14f4b432130f02871091b6a28594a9481da12f360f711685d

And then we can set a corresponding variable in Alice's terminal session, similar to doing the following:

.. code-block:: bash

	Alice:~$ TXID=533533c5a382ccf14f4b432130f02871091b6a28594a9481da12f360f711685d

.. tip:: You can use this technique whenever we need to use a variable set in one terminal session within another.

This then allows us to run the code below. 

.. code-block:: bash

	Alice:~$ liquid-cli gettransaction $TXID

This causes an error. The reason is that Alice's wallet will not contain wallet details of the transaction as it does not relate to an address contained in her wallet. We can get the raw transaction data from Alice's node's copy of the blockchain using the getrawtransaction command like this:

.. code-block:: bash

	Alice:~$ liquid-cli getrawtransaction $TXID 1

That returns raw transaction details. If you look within the "vout" section you can see that there are three instances. The first two instances are the receiving and change amounts and the third is the transaction fee. Of these three amounts, the fee is the only one in which you can see a value, as the fee itself is unblinded. For the first two instances you will see (amongst others) properties with values similar to this:

.. code-block:: text

	"value-minimum": 0.00000001,
	"value-maximum": 11258999.06842624,
	"amountcommitment": "0881c61d8a15ad26e6ef621ca99a188ccebbdb348d5285012393459b7e5b1e6113",
	"assetcommitment": "0b1b7a1a4a604f4a68b3277e3a8926d74e86adce7b92e8e6ba67f9c5a8ad2cbcf4",

What this shows are the "blinded ranges" of the value amounts and the commitment data that acts as proof of the actual amount and type of asset transacted. The raw view of the transaction will be the same accross all nodes, regardless of if they hold the blinding key or not, only the results of gettransaction from a wallet aware of the blinding key used will show the actual amounts.

Even if we were to import Bob's private key into Alice's wallet it would still not be able to see the amounts and type of asset using gettransaction because it still has no knowledge of the blinding key used. 

If we want to let Alice's wallet view the actual amount details we'll need to import the address as 'watch only' so gettransaction will work, and then import the blinding key so we can see the unblinded amounts. First, import and then view the transaction. Note the use of the second argument passed to gettransaction, set to true. This tells gettransaction to include watch only addresses, such as the one we imported. Remember to copy the value of $ADDR from Bob's session and set it in Alice's before running the code below.

.. code-block:: bash

	Alice:~$ liquid-cli importaddress $ADDR
	Alice:~$ liquid-cli gettransaction $TXID true

This time the call to gettransaction does not error but, because Alice still does not know the blinding key, the amount (towards the top of the output) will show as:

.. code-block:: text

	"amount": {
	  "bitcoin": 0.00000000

Without knowledge of the Blinding Key, the amount and type of asset being transacted is still hidden.

In order for anyone else apart from the sender and receiver of a Confidential Transaction (such as an auditor) to view the amount and type of assets being transacted, they need to know the blinding key that was used to generate the blinded address. To show this, we can export the blinding key Bob's wallet used for the related address, import it into Alice's wallet and try to view the transaction again. Let's export the key for that particular address from Bob's wallet and import it into Alice's.

Export Bob's blinding key for the address:

.. code-block:: bash

	Bob:~$ BOBBLINDINGKEY=$(liquid-cli dumpblindingkey $ADDR)

Echo, copy and set the variable accross terminal sessions again like we did above (steps not shown) and then import Bob's blinding key into Alice's wallet:

.. code-block:: bash

	Alice:~$ liquid-cli importblindingkey $ADDR $BOBBLINDINGKEY

Now that Alice's wallet has knowledge of the blinding key used on that address, we can run the checks we did above from Alice's wallet, this time expecting to see the actual amount value:

.. code-block:: bash

	Alice:~$ liquid-cli gettransaction $TXID true

Magic! Alice's wallet now shows the actual value sent in the transaction.

.. code-block:: text

	"amount": {
	  "bitcoin": 1.00000000

We've seen that the use of a blinding key hides the amount and type of assets in an address and that by importing the right blinding key, we can reveal those values. In practical use, a blinding key may be given to an auditor, should there be a need to verify the amounts and types of assets held by a party. The Confidential Transactions feature of Liquid also allows for "range proofs" to be performed without the need to expose actual amounts. This allows statements such as "address abc holds at least an amount x of asset y" to be cryptographically proven as true or false.

