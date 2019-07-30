-------------------------
Creating Raw Transactions
-------------------------

In this example we will create a raw transaction sending some of an issued asset.

Issue an asset and get the asset hex so we can send some:

.. code-block:: bash

	Alice:~$ ISSUE=$(elements-cli issueasset 100 1)
	Alice:~$ ASSET=$(echo $ISSUE | jq '.asset' | tr -d '"')

Check the asset shows up in our wallet:

.. code-block:: bash

	Alice:~$ elements-cli getwalletinfo

Get a list of unspent we can use as inputs, referenced via transaction id, vout and amount:

.. code-block:: bash

	Alice:~$ UTXO=$(elements-cli listunspent 0 0 [] true '''{"''asset''":"'''$ASSET'''"}''')
	Alice:~$ TXID=$(echo $UTXO | jq '.[0].txid' | tr -d '"' )
	Alice:~$ VOUT=$(echo $UTXO | jq '.[0].vout' | tr -d '"' )
	Alice:~$ AMOUNT=$(echo $UTXO | jq '.[0].amount' | tr -d '"' )

Get an address to send the asset to, we'll use an unconfidential address:

.. code-block:: bash

	Alice:~$ ADDR=$(elements-cli getnewaddress)
	Alice:~$ VALIDATEADDR=$(elements-cli validateaddress $ADDR)
	Alice:~$ UNCONADDR=$(echo $VALIDATEADDR | jq '.unconfidential' | tr -d '"')

Build the raw transaction, sending an amount of 3 of the asset:

.. code-block:: bash

	Alice:~$ SENDAMOUNT="3.00"
	Alice:~$ RAWTX=$(elements-cli createrawtransaction '''[{"''txid''":"'''$TXID'''", "''vout''":'$VOUT', "''asset''":"'''$ASSET'''"}]''' '''{"'''$UNCONADDR'''":'$SENDAMOUNT'}''' 0 false '''{"'''$UNCONADDR'''":"'''$ASSET'''"}''')

Fund the transaction:

.. code-block:: bash

	Alice:~$ FRT=$(elements-cli fundrawtransaction $RAWTX)

Blind and sign the transaction:

.. code-block:: bash

	Alice:~$ HEXFRT=$(echo $FRT | jq '.hex' | tr -d '"')
	Alice:~$ BRT=$(elements-cli blindrawtransaction $HEXFRT)
	Alice:~$ SRT=$(elements-cli signrawtransactionwithwallet $BRT)
	Alice:~$ HEXSRT=$(echo $SRT | jq '.hex' | tr -d '"')

Send the raw transaction:

.. code-block:: bash

	Alice:~$ TX=$(elements-cli sendrawtransaction $HEXSRT)

After the transaction has confirmed, decode the raw transaction so we can see the amount of the asset sent and the address it went to:

.. code-block:: bash

	Alice:~$ GRT=$(elements-cli getrawtransaction $TX)
	Alice:~$ DRT=$(elements-cli decoderawtransaction $GRT)

