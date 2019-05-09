-----------------
Proof of Issuance
-----------------

In this example we aim to show how an asset issuer can prove that they were the ones who issued a particular asset. We do that by making use of the Contract Hash argument of rawissueasset. We will refer to the idea of a public 'Asset Registry' although no specific implementation is suggested at present.

We need to get a 'legacy' type address and can directly use that:

.. code-block:: bash

	Alice:~$ NEWADDR=$(liquid-cli getnewaddress "" legacy)
	Alice:~$ VALIDATEADDR=$(liquid-cli getaddressinfo $NEWADDR)
	Alice:~$ PUBKEY=$(echo $VALIDATEADDR | jq '.pubkey' | tr -d '"')
	Alice:~$ ADDR=$NEWADDR

Record a message (MSG) to some form of public asset registry to prove we (ABC Company) control the address in question:

.. code-block:: bash

	Alice:~$ MSG="THE ADDRESS THAT I, ABC, HAVE CONTROL OVER IS:[$ADDR] PUBKEY:[$PUBKEY]"
	Alice:~$ SIGNEDMSG=$(liquid-cli signmessage $ADDR "$MSG")
	Alice:~$ VERIFIED=$(liquid-cli verifymessage $ADDR $SIGNEDMSG "$MSG")

Write that proof to the registry:

.. code-block:: bash

	Alice:~$ echo "REGISTRY ENTRY = MESSAGE:[$MSG] SIGNED MESSAGE:[$SIGNEDMSG]"
	Alice:~$ FUNDINGADDR=$(liquid-cli getnewaddress)
	Alice:~$ liquid-cli sendtoaddress $FUNDINGADDR 2

Wait for the transaction to confirm.

.. code-block:: bash

	Alice:~$ RAWTX=$(liquid-cli createrawtransaction [] '''{"'''$FUNDINGADDR'''":1.00}''')
	Alice:~$ FRT=$(liquid-cli fundrawtransaction $RAWTX)
	Alice:~$ HEXFRT=$(echo $FRT | jq '.hex' | tr -d '"')

Write whatever you want as a contract text. Include reference to signed proof of ownership above:

.. code-block:: bash

	Alice:~$ CONTRACTTEXT="THIS IS THE CONTRACT TEXT FOR THE XYZ ASSET. CREATED BY ABC. ADDRESS:[$ADDR] PUBKEY:[$PUBKEY]"

Sign it using the same address it references:

.. code-block:: bash

	Alice:~$ SIGNEDCONTRACTTEXT=$(liquid-cli signmessage $ADDR "$CONTRACTTEXT")

Hash that signed message, which we will use as the contract hash. We need to use a hash of size 32 bytes. You can do this a number of ways, we will use openssl commandline in the example:

.. code-block:: bash

	Alice:~$ CONTRACTTEXTHASH=$(echo -n $SIGNEDCONTRACTTEXT | openssl dgst -sha256)
	Alice:~$ CONTRACTTEXTHASH=$(echo ${CONTRACTTEXTHASH#"(stdin)= "})
	Alice:~$ echo $CONTRACTTEXTHASH

Issue the asset to the address that we signed for earlier and which we included in the signed contract hash:

.. code-block:: bash

	Alice:~$ RIA=$(liquid-cli rawissueasset $HEXFRT '''[{"''asset_amount''":33, "''asset_address''":"'''$ADDR'''", "''blind''":"''false''", "''contract_hash''":"'''$CONTRACTTEXTHASH'''"}]''')

Details of the issuance:

.. code-block:: bash

	Alice:~$ HEXRIA=$(echo $RIA | jq '.[0].hex' | tr -d '"')
	Alice:~$ ASSET=$(echo $RIA | jq '.[0].asset' | tr -d '"')
	Alice:~$ ENTROPY=$(echo $RIA | jq '.[0].entropy' | tr -d '"')
	Alice:~$ TOKEN=$(echo $RIA | jq '.[0].token' | tr -d '"')

Blind, sign and send the issuance transaction:

.. code-block:: bash

	Alice:~$ BRT=$(liquid-cli blindrawtransaction $HEXRIA)
	Alice:~$ SRT=$(liquid-cli signrawtransactionwithwallet $BRT)
	Alice:~$ HEXSRT=$(echo $SRT | jq '.hex' | tr -d '"')
	Alice:~$ ISSUETX=$(liquid-cli sendrawtransaction $HEXSRT)

Wait for sufficient confirmations and then in the output from decoderawtransaction you will see in the vout section the asset being issued to the address we signed from earlier:

.. code-block:: bash

	Alice:~$ RT=$(liquid-cli getrawtransaction $ISSUETX)
	Alice:~$ DRT=$(liquid-cli decoderawtransaction $RT)

Build an asset registry entry saying that we issued the asset:

.. code-block:: bash

	Alice:~$ ASSETREGISTERMESSAGE="I, ABC, CREATED ASSET:[$ASSET] WITH ASSET ENTROPY:[$ENTROPY] AT ADDRESS:[$ADDR] IN TX:[$ISSUETX]"
	Alice:~$ SIGNEDMSG=$(liquid-cli signmessage $ADDR "$ASSETREGISTERMESSAGE")
	Alice:~$ liquid-cli verifymessage $ADDR $SIGNEDMSG "$ASSETREGISTERMESSAGE"

Then make the entry in the aset registry:

.. code-block:: bash

	Alice:~$ echo "REGISTRY ENTRY = ASSET CREATION MESSAGE:[$ASSETREGISTERMESSAGE] SIGNED VERSION:[$SIGNEDMSG]"
	Alice:~$ liquid-cli listissuances
	Alice:~$ liquid-cli getwalletinfo

To prove the issuance was indeed made against the contract hash, we need to provide the following to anyone wishing to validate that the contract has was used to produce the asset: Hex of funded raw transaction used to fund the issuance and the contract_hash. Not needed: asset_amount, asset_address, blind.

If someone else tries to claim they created the asset and we didn't, they will need to prove they can sign for the address it was sent to and explain how come we can sign messages (as found in the asset registry) for that address.

.. code-block:: bash

	Alice:~$ VERIFYISSUANCE=$(liquid-cli rawissueasset $HEXFRT '''[{"''asset_amount''":33, "''asset_address''":"'''$ADDR'''", "''blind''":"''false''", "''contract_hash''":"'''$CONTRACTTEXTHASH'''"}]''')

	Alice:~$ ASSETVERIFY=$(echo $VERIFYISSUANCE | jq '.[0].asset' | tr -d '"')
	Alice:~$ ENTROPYVERIFY=$(echo $VERIFYISSUANCE | jq '.[0].entropy' | tr -d '"')
	Alice:~$ TOKENVERIFY=$(echo $VERIFYISSUANCE | jq '.[0].token' | tr -d '"')
	Alice:~$ [[ $ASSET = $ASSETVERIFY ]] && echo ASSET HEX: VERIFIED || echo ASSET HEX: NOT VERIFIED
	Alice:~$ [[ $ENTROPY = $ENTROPYVERIFY ]] && echo ENTROPY: VERIFIED || echo ENTROPY: NOT VERIFIED
	Alice:~$ [[ $TOKEN = $TOKENVERIFY ]] && echo TOKEN: VERIFIED || echo TOKEN: NOT VERIFIED

