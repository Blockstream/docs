----------------------
Creating Raw Issuances
----------------------

Get an address to issue the asset to:

.. code-block:: bash

	Alice:~$ ASSET_ADDR=$(liquid-cli getnewaddress "" legacy)

Get an address to issue the reissuance token to:

.. code-block:: bash

	Alice:~$ TOKEN_ADDR=$(liquid-cli getnewaddress "" legacy)

Create the raw transaction and fund it:

.. code-block:: bash

	Alice:~$ RAWTX=$(liquid-cli createrawtransaction '''[]''' '''{"''data''":"''00''"}''')
	Alice:~$ FRT=$(liquid-cli fundrawtransaction $RAWTX)
	Alice:~$ HEXFRT=$(echo $FRT | jq '.hex' | tr -d '"')

Create the raw issuance:

.. code-block:: bash

	Alice:~$ ASSET_AMOUNT=100
	Alice:~$ TOKEN_AMOUNT=1
	Alice:~$ RIA=$(liquid-cli rawissueasset $HEXFRT '''[{"''asset_amount''":'$ASSET_AMOUNT', "''asset_address''":"'''$ASSET_ADDR'''", "''token_amount''":'$TOKEN_AMOUNT', "''token_address''":"'''$TOKEN_ADDR'''", "''blind''":false}]''')

The results of which include the following for reference: 

.. code-block:: bash

	Alice:~$ HEXRIA=$(echo $RIA | jq '.[0].hex' | tr -d '"')
	Alice:~$ ASSET=$(echo $RIA | jq '.[0].asset' | tr -d '"')
	Alice:~$ ENTROPY=$(echo $RIA | jq '.[0].entropy' | tr -d '"')
	Alice:~$ TOKEN=$(echo $RIA | jq '.[0].token' | tr -d '"')

Blind, sign and send the transaction that creates the asset issuance:

.. code-block:: bash

	Alice:~$ BRT=$(liquid-cli blindrawtransaction $HEXRIA true '''[]''' false)
	Alice:~$ SRT=$(liquid-cli signrawtransactionwithwallet $BRT)
	Alice:~$ HEXSRT=$(echo $SRT | jq '.hex' | tr -d '"')
	Alice:~$ ISSUETX=$(liquid-cli sendrawtransaction $HEXSRT)

Confirm and check it worked:

.. code-block:: bash

	Alice:~$ liquid-cli generatetoaddress 101 $(liquid-cli getnewaddress)
	Alice:~$ liquid-cli listissuances
	Alice:~$ liquid-cli getwalletinfo

