-----------------
Proof of Issuance
-----------------

This example shows how to use Blockstream's `Liquid asset registry`_. The asset registry allows you to register an asset and prove ownership against a domain name. 

The code below creates an asset and an associated token. It outputs two files:

* A file that must be placed on the server of the registered domain.

* A .sh file that will post the asset registration data to the blockstream asset registry.

We'll be using Liquid in test mode, so the registration will not complete if you run the output registration script. The code can easily be adapted for live use. To start Liquid in test mode, set the following in your config file.

.. _Liquid asset registry: https://assets.blockstream.info

.. code-block:: text

	chain=elementsregtest
	daemon=1
	listen=1
	txindex=1
	validatepegin=0
	initialfreecoins=2100000000000000

First of all, set the values you want associated with the asset. These are required.

.. code-block:: bash

	Alice:~$ NAME="Liquid XYZ Asset"
	Alice:~$ TICKER="LXYZ"
	Alice:~$ DOMAIN="lxzy.com"
	Alice:~$ ASSET_AMOUNT=100
	Alice:~$ TOKEN_AMOUNT=1

Next, set the precision. For most uses, zero will be fine.

.. code-block:: bash

	Alice:~$ PRECISION=0

Do not change the following value.

.. code-block:: bash

	Alice:~$ VERSION=0 

Next we need to get a 'legacy' type address, extract the public key, and store the address so we can issue the asset to it later.

.. code-block:: bash

	Alice:~$ NEWADDR=$(liquid-cli getnewaddress "" legacy)
	Alice:~$ VALIDATEADDR=$(liquid-cli getaddressinfo $NEWADDR)
	Alice:~$ PUBKEY=$(echo $VALIDATEADDR | jq '.pubkey' | tr -d '"')
	Alice:~$ ASSET_ADDR=$NEWADDR

We also need an address for the token.

.. code-block:: bash

	Alice:~$ NEWADDR=$(liquid-cli getnewaddress "" legacy)
	Alice:~$ VALIDATEADDR=$(liquid-cli getaddressinfo $NEWADDR)
	Alice:~$ TOKEN_ADDR=$NEWADDR

Create the contract and calculate the contract hash. The contract is formatted for use in the Blockstream Asset Registry.

.. code-block:: bash

	Alice:~$ CONTRACT_REGISTRY_ENTRY="{\"name\": \"$NAME\", \"ticker\": \"$TICKER\", \"precision\": $PRECISION, \"entity\": {\"domain\": \"$DOMAIN\"}, \"issuer_pubkey\": \"$PUBKEY\", \"version\": $VERSION}"

We will hash using openssl to create the contratc hash. Other options are available.

.. code-block:: bash

	Alice:~$ CONTRACT_TEXT_HASH=$(echo -n $CONTRACT_REGISTRY_ENTRY | openssl dgst -sha256)
	Alice:~$ CONTRACT_TEXT_HASH=$(echo ${CONTRACT_TEXT_HASH#"(stdin)= "})

Create and fund the transaction.

.. code-block:: bash

	Alice:~$ RAWTX=$(liquid-cli createrawtransaction '''[]''' '''{"''data''":"''00''"}''')
	Alice:~$ FRT=$(liquid-cli fundrawtransaction $RAWTX)
	Alice:~$ HEXFRT=$(echo $FRT | jq '.hex' | tr -d '"')

Create the raw issuance.

.. code-block:: bash

	Alice:~$ RIA=$(liquid-cli rawissueasset $HEXFRT '''[{"''asset_amount''":'$ASSET_AMOUNT', "''asset_address''":"'''$ASSET_ADDR'''", "''token_amount''":'$TOKEN_AMOUNT', "''token_address''":"'''$TOKEN_ADDR'''", "''blind''":false, "''contract_hash''":"'''$CONTRACT_TEXT_HASH'''"}]''')

Show details of the issuance. These are not all essential to the registry, but may be useful for reference.

.. code-block:: bash

	Alice:~$ HEXRIA=$(echo $RIA | jq '.[0].hex' | tr -d '"')
	Alice:~$ ASSET=$(echo $RIA | jq '.[0].asset' | tr -d '"')
	Alice:~$ ENTROPY=$(echo $RIA | jq '.[0].entropy' | tr -d '"')
	Alice:~$ TOKEN=$(echo $RIA | jq '.[0].token' | tr -d '"')

Blind, sign and send the issuance transaction.

.. code-block:: bash

	Alice:~$ BRT=$(liquid-cli blindrawtransaction $HEXRIA true '''[]''' false)
	Alice:~$ SRT=$(liquid-cli signrawtransactionwithwallet $BRT)
	Alice:~$ HEXSRT=$(echo $SRT | jq '.hex' | tr -d '"')
	Alice:~$ ISSUETX=$(liquid-cli sendrawtransaction $HEXSRT)

Confirm the transaction and check the results.

.. code-block:: bash

	Alice:~$ liquid-cli generatetoaddress 101 $ADDRGEN1
	Alice:~$ liquid-cli listissuances
	Alice:~$ liquid-cli getwalletinfo

We have the required data and have formatted the contract data so can now create the required files that will allow the registration.

Write the domain and asset ownership proof to a file. The file should then be placed in a directory within the root of your domain. The directory should be named ".well_known". Note that the file should have no extension and just copied as it is created. Do not change the contents of the file in any way.

.. code-block:: bash

	Alice:~$ echo "Authorize linking the domain name $DOMAIN to the Liquid asset $ASSET" > liquid-asset-proof-$ASSET

After you have placed the file output above on your domain, you can run the register_asset.sh script created below to post the asset data to the registry.

.. code-block:: bash

	Alice:~$ echo "curl https://assets.blockstream.info/ --data-raw '{\"asset_id\":\"$ASSET\",\"contract\":$CONTRACT_REGISTRY_ENTRY,\"issuance_txin\":{\"txid\":\"$ISSUETX\",\"vin\":0}}'" > register_asset.sh

Once the required checks against the domain and issuance transaction have been made, the registration can be found on Blockstream's `Liquid asset registry`_.
