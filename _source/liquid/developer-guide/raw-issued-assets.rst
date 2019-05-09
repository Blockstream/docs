----------------------
Creating Raw Issuances
----------------------

Start the funding of the issuance by sending funds to a new address:

.. code-block:: bash

	Alice:~$ ADDR=$(liquid-cli getnewaddress)
	Alice:~$ liquid-cli sendtoaddress $ADDR 2

After the transaction has confirmed, create a raw transaction:

.. code-block:: bash

	Alice:~$ RAWTX=$(liquid-cli createrawtransaction [] '''{"'''$ADDR'''":1.00}''')

Fund the raw transaction:

.. code-block:: bash

	Alice:~$ FRT=$(liquid-cli fundrawtransaction $RAWTX)
	Alice:~$ HEXFRT=$(echo $FRT | jq '.hex' | tr -d '"')

Get the unconfidential address we will create the asset at:

.. code-block:: bash

	Alice:~$ VALIDATEADDR=$(liquid-cli validateaddress $ADDR)
	Alice:~$ UNCONADDR=$(echo $VALIDATEADDR | jq '.unconfidential' | tr -d '"')

Create the raw issuance using the funding transaction hex, our unconfidential address etc:

.. code-block:: bash

	Alice:~$ RIA=$(liquid-cli rawissueasset $HEXFRT '''[{"''asset_amount''":33, "''asset_address''":"'''$UNCONADDR'''", "''blind''":"''false''"}]''')

The results of which can be stored and viewed:

.. code-block:: bash

	Alice:~$ HEXRIA=$(echo $RIA | jq '.[0].hex' | tr -d '"')
	Alice:~$ ASSET==$(echo $RIA | jq '.[0].asset' | tr -d '"')
	Alice:~$ ENTROPY==$(echo $RIA | jq '.[0].entropy' | tr -d '"')
	Alice:~$ TOKEN==$(echo $RIA | jq '.[0].token' | tr -d '"')

Blind, sign and send the transaction that creates the asset issuance:

.. code-block:: bash

	Alice:~$ BRT=$(liquid-cli blindrawtransaction $HEXRIA)
	Alice:~$ SRT=$(liquid-cli signrawtransactionwithwallet $BRT)
	Alice:~$ HEXSRT=$(echo $SRT | jq '.hex' | tr -d '"')
	Alice:~$ ISSUETX=$(liquid-cli sendrawtransaction $HEXSRT)

After the transaction has confirmed, check the issuance worked:

.. code-block:: bash

	Alice:~$ liquid-cli getwalletinfo
	Alice:~$ liquid-cli listissuances      

