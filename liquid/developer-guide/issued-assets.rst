-------------
Issued Assets
-------------

In this example we'll issue our own asset, label it, look at the re-issuance token, and learn how to send the asset to other other Liquid nodes and wallet addresses. We'll also take a look at how to keep track of what assets have been issued and re-issued.

First, let's take a look at Alice's wallet to see what it currently holds.

.. code-block:: bash

	Alice:~$ elements-cli getwalletinfo

We see that Alice holds a lot of the "bitcoin" asset and nothing else:

.. code-block:: text

	"balance": {
	  "bitcoin": 101.00000000
	}

Every asset you issue within Liquid (including the "bitcoin" default) will be assigned its own hex value. This is used to uniquely identify it on the network. Notice how "bitcoin" is displayed with a readable asset name. This is because Liquid automatically associates the label "bitcoin" with the asset hex for that default asset. To find out its hex value we can run:

.. code-block:: bash

	Alice:~$ elements-cli dumpassetlabels

Which returns:

.. code-block:: text

	"bitcoin": "cc5fb67403bee3b9a74e7518b7684d6cb64041e4156970a17aa653ee336b1097"

One of the main features of Liquid is the ability to issue your own assets. 

.. tip:: There is nothing inherently different between assets in the way they are handled within the Liquid protocol.

Run the following to perform a blinded issuance of 100 of a new asset, along with 1 reissuance token for the asset.

.. code-block:: bash

	Alice:~$ ISSUE=$(elements-cli issueasset 100 1)

.. tip:: To manually issue an asset using the rawissueasset command, please see the Creating Raw Issuances example. Additionally, the Proof of Issuance example shows how to prove that you were the one who issued the asset using the "contract hash" parameter.

That will create a new asset type, an initial supply of 100 and also 1 reissuance token. The reissuance token is used to prove authority to reissue more of the asset at a later date. We have issued one such token in the command above. The token is transferable and you can initially create as many as you think you will need based upon how many of the network participants will need to perform this duty. Each asset has its own reissuance token. We'll look at this in more detail later. It is also possibly to create an asset with no reissuance tokens by using zero for that argument.

.. tip:: The minimum amount of an asset you can issue is 0.00000001. This is also the smallest amount of any issued asset that you can send. The minimum amount of the default asset that you can send (L-BTC) is higher, at 0.00001. These differ as the default asset considers the minimum send amount of the Bitcoin network's dust limit. The maximum amount of an asset you can issue is 21,000,000 although more can be created by reissuing. When you create the reissuance token as above, where the amount was 1, you are actually creating 100,000,000 of them when measured in their smallest denomination. This is because they are divisible like every other asset on Liquid. The 1 is little more than a user interface concession. For ease of readability we will refer to this issuance as "one token".

We have stored all of the returned data from the issuance command in a variable named "ISSUE", which we'll pull the hex of the new asset from, storing that value in another variable named "ASSET". We'll also store the "token" value (which we'll explain and use later) and the "txid" and "vin" of the issuance, which will be used when we try and unblind the issuing transaction shortly.

In order to do this we can use a tool called jq_ (which we installed as part of the tutorial dependencies) to strip out and store only the parts returned and saved within “ISSUE” that we are interested in:

.. _jq: https://stedolan.github.io/jq/

.. code-block:: bash

	Alice:~$ ASSET=$(echo $ISSUE | jq '.asset' | tr -d '"')
	Alice:~$ TOKEN=$(echo $ISSUE | jq '.token' | tr -d '"')
	Alice:~$ ITXID=$(echo $ISSUE | jq '.txid' | tr -d '"')
	Alice:~$ IVIN=$(echo $ISSUE | jq '.vin' | tr -d '"')

To see the hex identifier for the asset we issued run:

.. code-block:: bash

	Alice:~$ echo $ASSET

Which will return something like this:

.. code-block:: text

	f0379482f9b77917670be0f060cc58debc6d93db0bf857458d5fb19080c8ab67

In order to view all asset issuances that have been made we run the "listissuances" command:

.. code-block:: bash

	Alice:~$ elements-cli listissuances

That will show the asset we just issued. You'll notice tha it has the following property:

.. code-block:: text

	"isreissuance": false,

This indicates that it was an original issuance and not a reissuances. You'll also see that the newly issued asset does not have an "assetlabel".

.. tip:: Asset labels are not part of network protocol consensus and are local only to each node. You should not rely on them for transaction processing but instead use the asset's hex value, which is shared across the network.

You can set the label by assigning it against the hex identifier of the asset. This can be done in the relevant Liquid.conf file by adding a line:

.. code-block:: text

	assetdir=asset_hex_here:your_label_here

Or you can do this by passing in "assetdir" as a parameter when you start the node. We'll do this now and call our new asset "demoasset". We need to stop the node first.

.. code-block:: bash

	Alice:~$ elements-cli stop
	Alice:~$ elementsd -assetdir=$ASSET:demoasset
	Alice:~$ elements-cli listissuances

This shows that the asset we issued has the label we assigned to its hex value:

.. code-block:: text

	"assetlabel": "demoasset",

Having labelled our asset for ease of reference, we will now look at the issuance data for "demoasset" in more detail. You will notice a "token" property similar to that below:

.. code-block:: text

	"token": "33244cc19dd9df0fd901e27246e3413c8f6a560451e2f3721fb6f636791087c7",

This is the hex of the token and it can be used to reissue the asset. Yours will likely differ from the actual value above. There is also a "tokenamount" property which corresponds to the amount we created:

.. code-block:: text

	"tokenamount": 1.00000000,

When the transaction has confirmed, we can take a look using Bob's wallet to see if it is aware of Alice's asset issuance:

.. code-block:: bash

	Bob:~$ elements-cli listissuances

Bob's wallet isn't aware of the issuance, so we'll import the address into his wallet.

In order to check Bob's view of the issuance, we need Bob's node to use the address used when Alice issued the asset. As that's currently known to Alice's node, we need to copy the information over to Bob's node. This can be done by using echo to print the value within one terminal session, copying the value, and then setting it within the other node's terminal session, like so:

.. code-block:: bash

	Alice:~$ IADDR=$(elements-cli gettransaction $ITXID | jq '.details[0].address' | tr -d '"')
	Alice:~$ echo $IADDR

Copy the result, which will be similar to:

.. code-block:: bash

	AzpkDFjr6nNm4nncoBHuZr5S2zH4wPEpTvU63M8jNuU7JMXAVb5rVNCYmLB3fTmP9kLGgTAdCTwrrndu

And then we can set a corresponding variable in Bob's terminal session, similar to doing the following:

.. code-block:: bash

	Bob:~$ IADDR=AzpkDFjr6nNm4nncoBHuZr5S2zH4wPEpTvU63M8jNuU7JMXAVb5rVNCYmLB3fTmP9kLGgTAdCTwrrndu

.. tip:: You can use this technique whenever we need to use a variable set in one terminal session within another.

Bob can now import the address relating to Alice's issuance:

.. code-block:: bash

	Bob:~$ elements-cli importaddress $IADDR

If we try and view the list of issuances from Bob's node now we'll see the issuance, but notice that the amount of the asset and the amount of its associated token are hidden:

.. code-block:: bash

	Bob:~$ elements-cli listissuances

The asset amount and the token amount are both blinded and show as -1:

.. code-block:: text

	"tokenamount": -1,
	"assetamount": -1,

In the Confidential Transactions example, we were able to expose the amount and type of asset being sent in a regular Confidential Transaction by exporting the blinding key used to create the blinded address and importing it into another wallet. We can do the same type of thing with the issuance transaction using the issuance blinding key.

First, we need to export the issuance blinding key. We refer to issuances by their txid/vin pair. As there is only one per input it will be zero, but we'll use the value we saved earlier as it is good practice to not rely on such things staying fixed. You will need to echo, copy and create the appropriate variables (ISSUEKEY, ITXID, IVIN) in Bob's node as we did above for this to work (steps not shown).

.. code-block:: bash

	Alice:~$ ISSUEKEY=$(elements-cli dumpissuanceblindingkey $ITXID $IVIN)

.. code-block:: bash

	Bob:~$ elements-cli importissuanceblindingkey $ITXID $IVIN $ISSUEKEY

Now when we run the command to list known issuances from Bob's wallet we should see the actual values:

.. code-block:: bash

	Bob:~$ elements-cli listissuances

Which returns:

.. code-block:: text

	"tokenamount": 1.00000000,
	"assetamount": 100.00000000,

Indeed, Bob's wallet can now see both the amount of the asset and its reissuance token that were issued.

Just like any other asset in Liquid, we can send our "demoasset" from Alice's address to Bob's using the "sendtoaddress" command. This differs from its implementation in Bitcoin's source code in that it accepts an additional parameter, which allows you to specify the type of asset to be sent. Be aware that the step above where we imported the issuance blinding key is not required in order to transact an issued asset between addresses and wallets. Importing the issuance blinding key just enables another wallet to view the issuance details in full.

You will need to echo, copy and create the appropriate variable (BOBDEMOADD) in Alice's node as we did above for this to work (steps not shown).

.. code-block:: bash

	Bob:~$ BOBDEMOADD=$(elements-cli getnewaddress)

.. code-block:: bash

	Alice:~$ elements-cli sendtoaddress $BOBDEMOADD 10 "" "" false false 1 UNSET demoasset

After confirmation, Bob's wallet will now show an amount of 10 "demoasset" and Alice's will show 90:

.. code-block:: bash

	Bob:~$ elements-cli getwalletinfo
	Alice:~$ elements-cli getwalletinfo

As we didn't assign a label in Bob's node for the asset we created, it will be identified by its hex value instead. We will therefore have to use the hex identifier instead of the asset label when we send it from his node. Remember that asset labels are local only to each node and are not part of the network's protocol rules. We'll demonstrate how Bob can send the asset using the hex value by transferring the 10 "demoasset" back to Alice:

You will need to echo, copy and create the appropriate variable (ALICEDEMOADD) in Bob's node as we did above for this to work (steps not shown).

.. code-block:: bash

	Alice:~$ ALICEDEMOADD=$(elements-cli getnewaddress)

.. code-block:: bash

	Bob:~$ elements-cli sendtoaddress $ALICEDEMOADD 10 "" "" false false 1 UNSET $ASSET
	Bob:~$ ADDRGEN=$(elements-cli getnewaddress)
	Bob:~$ elements-cli generatetoaddress 1 $ADDRGEN

We should see that Bob's wallet has no "demoasset" in it anymore and Alice's is back to 100:

.. code-block:: bash

	Bob:~$ elements-cli getwalletinfo
	Alice:~$ elements-cli getwalletinfo

We can see that is indeed the case.

