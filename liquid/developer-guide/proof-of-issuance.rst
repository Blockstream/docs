-------------------------------------------------------
Proof of Issuance - Blockstream's Liquid Asset Registry
-------------------------------------------------------

This example shows how to use Blockstream's `Liquid Asset Registry`_. The asset registry allows you to register an asset and prove ownership against a domain name. 

The code below creates an asset, an associated token, and outputs two files:

* A 'liquid-asset-proof-' file that must be placed on the server of the registered domain.

* A 'register_asset.sh' file that, when run, will post the asset registration data to Blockstream's Liquid Asset Registry.

The use of the output files will be explained later.

There are six fields that need setting within the script:

* ``NAME`` The name of the asset as it will apear in the registry and applications that use asset registry data, such as the `Blockstream Explorer`_. Length must be 5 to 255 ascii characters.

* ``TICKER`` The ticker you would like to assign to the asset. Length must be 3 to 5 characters (valid characters are a-z and A-Z).

* ``DOMAIN`` The domain that will be used to verify the asset. Must be a valid domain name format, for example: example.com or sub.example.com. Do not include the http/s or www prefixes.

* ``ASSET_AMOUNT`` The amount to be issued. It is preferable to consider one satoshi unit as representing one asset. So to issue an amount of 100 of an asset, a value of 0.00000100 should be used. Please note that how this value is displayed within applications that use asset registry data is affected by the ``PRECISION`` field. 

* ``TOKEN_AMOUNT`` The amount of reissuance tokens to be created. It is preferable to consider one satoshi unit as representing one reissuance token, so a value of 0.00000001 will create 0.00000001 reissuance tokens and when viewed from Liquid Core it will also show as 0.00000001. When viewed from an app that uses satoshi sized units, such as the `Blockstream Explorer`_ it will show as 1. This field is not affected by the ``PRECISION`` field.

* ``PRECISION`` The precision used to display the asset amount within applications that use `Liquid Asset Registry`_ data, such as the `Blockstream Explorer`_. Examples: a ``PRECISION`` value of 0 for an ``ASSET_AMOUNT`` of 0.00000100 will create a value of 0.00000100 when viewed from Liquid Core and 100 when viewed in an app that uses the asset registry data. A ``PRECISION`` value of 2 for an ``ASSET_AMOUNT`` of 0.00000100 will create a value of 0.00000100 when viewed from Liquid Core and 1.00 when viewed in the same app. The default is 0 and the maximum value is 8. 

.. _Blockstream Explorer: https://blockstream.info/liquid/
.. _Liquid Asset Registry: https://assets.blockstream.info

The code below uses Liquid in live mode. The code can easily be adapted for test use.

Save the code below in a file named issue_and_prepare_register.sh

.. code-block:: bash

	#!/bin/bash
	set -x
	shopt -s expand_aliases
	
	# ASSUMES liquidd IS ALREADY RUNNING
	
	######################################################
	#                                                    #
	#    SCRIPT CONFIG - PLEASE REVIEW BEFORE RUNNING    #
	#                                                    #
	######################################################
	
	# Amend the following:
	NAME="Wintercoin"
	TICKER="WTX"
	# Do not use a domain prefix in the following:
	DOMAIN="wintercooled.github.io"
	# Issue 100 assets using the satoshi unit, dependant on PRECISION when viewed from 
	# applications using Asset Registry data.
	ASSET_AMOUNT=0.00000100
	# Issue 1 reissuance token using the satoshi unit, unaffected by PRECISION.
	TOKEN_AMOUNT=0.00000001
	
	# Amend the following if needed:
	PRECISION=0
	
	# Don't change the following:
	VERSION=0
	
	# If your Liquid node has not seen enough transactions to calculate
	# its own feerate you will need to specify one to prevent a 'feeRate'
	# error. We'll assume this is the case:
	FEERATE=0.00003000
	
	# Change the following point to your liquid-cli binary and liquid data directory.
	# If you have not changed the data directory, you can delete that argument below.
	alias e1-cli="$HOME/liquid/liquid-0.17.0/bin/liquid-cli -datadir=$HOME/liquid"
	
	##############################
	#                            #
	#    END OF SCRIPT CONFIG    #
	#                            #
	##############################

	# Exit on error
	set -o errexit
	
	# We need to get a 'legacy' type (prefix 'CTE') address for this:
	NEWADDR=$(e1-cli getnewaddress "" legacy)
	
	VALIDATEADDR=$(e1-cli getaddressinfo $NEWADDR)
	
	PUBKEY=$(echo $VALIDATEADDR | jq '.pubkey' | tr -d '"')
	
	ASSET_ADDR=$NEWADDR
	
	NEWADDR=$(e1-cli getnewaddress "" legacy)
	
	TOKEN_ADDR=$NEWADDR
	
	# Create the contract and calculate the contract hash.
	# The contract is formatted for use in the Blockstream Asset Registry
	# Do not amend the following!
	
	CONTRACT='{"entity":{"domain":"'$DOMAIN'"},"issuer_pubkey":"'$PUBKEY'","name":"'$NAME'","precision":'$PRECISION',"ticker":"'$TICKER'","version":'$VERSION'}'
	
	# We will hash using openssl, other options are available
	CONTRACT_HASH=$(echo -n $CONTRACT | openssl dgst -sha256)
	CONTRACT_HASH=$(echo ${CONTRACT_HASH#"(stdin)= "})
	
	# Reverse the hash. This will be calculated from the contract by the asset registry service to
	# check validity of the issuance against the registry entry.
	TEMP=$CONTRACT_HASH
	
	LEN=${#TEMP}
	
	until [ $LEN -eq "0" ]; do
	    END=${TEMP:(-2)}
	    CONTRACT_HASH_REV="$CONTRACT_HASH_REV$END"
	    TEMP=${TEMP::-2}
	    LEN=$((LEN-2))
	done
	
	RAWTX=$(e1-cli createrawtransaction '''[]''' '''{"''data''":"''00''"}''')
	
	# If your Liquid node has seen enough transactions to calculate its
	# own feeRate then you can switch the two lines below. We'll default
	# to specifying a fee rate:
	#FRT=$(e1-cli fundrawtransaction $RAWTX)
	FRT=$(e1-cli fundrawtransaction $RAWTX '''{"''feeRate''":'$FEERATE'}''')
	
	HEXFRT=$(echo $FRT | jq '.hex' | tr -d '"')
	
	RIA=$(e1-cli rawissueasset $HEXFRT '''[{"''asset_amount''":'$ASSET_AMOUNT', "''asset_address''":"'''$ASSET_ADDR'''", "''token_amount''":'$TOKEN_AMOUNT', "''token_address''":"'''$TOKEN_ADDR'''", "''blind''":false, "''contract_hash''":"'''$CONTRACT_HASH_REV'''"}]''')
	
	# Details of the issuance...
	HEXRIA=$(echo $RIA | jq '.[0].hex' | tr -d '"')
	ASSET=$(echo $RIA | jq '.[0].asset' | tr -d '"')
	ENTROPY=$(echo $RIA | jq '.[0].entropy' | tr -d '"')
	TOKEN=$(echo $RIA | jq '.[0].token' | tr -d '"')
	
	# Blind, sign and send the issuance transaction...
	BRT=$(e1-cli blindrawtransaction $HEXRIA true '''[]''' false)
	
	SRT=$(e1-cli signrawtransactionwithwallet $BRT)
	
	HEXSRT=$(echo $SRT | jq '.hex' | tr -d '"')
	
	# Test the transaction's acceptance into the mempool
	TEST=$(e1-cli testmempoolaccept '''["'$HEXSRT'"]''')
	ALLOWED=$(echo $TEST | jq '.[0].allowed' | tr -d '"')
	
	# If the transaction is valid
	if [ "true" = $ALLOWED ] ; then
	    # Broadcast the transaction
	    ISSUETX=$(e1-cli sendrawtransaction $HEXSRT)
	else
	    echo "ERROR SENDING TRANSACTION!"
	fi
	
	#####################################
	#                                   #
	#    ASSET REGISTRY FILE OUTPUTS    #
	#                                   #
	#####################################
	
	# Blockstream's Liquid Asset Registry (https://assets.blockstream.info/) can be used to register an asset to an issuer.
	# We already have the required data and have formatted the contract plain text into a format that we can use for this.
	
	# Write the domain and asset ownership proof to a file. The file should then be placed in a directory 
	# within the root of your domain named ".well-known"
	# The file should have no extension and just copied as it is created.
	
	echo "Authorize linking the domain name $DOMAIN to the Liquid asset $ASSET" > liquid-asset-proof-$ASSET
	
	# After you have placed the above file without your domain you can run the register_asset.sh script created below to post the asset data to the registry.
	
	echo "curl https://assets.blockstream.info/ --data-raw '{\"asset_id\":\"$ASSET\",\"contract\":$CONTRACT}'" > register_asset.sh
	
	# For reference, write some asset details. These are not needed by the asset registry.
	
	echo "ISSUETX:$ISSUETX ASSET:$ASSET ENTROPY:$ENTROPY TOKEN:$TOKEN ASSET_AMOUNT:$ASSET_AMOUNT TOKEN_AMOUNT:$TOKEN_AMOUNT ASSET_ADDR:$ASSET_ADDR TOKEN_ADDR:$TOKEN_ADDR CONTRACT_HASH_REV:$CONTRACT_HASH_REV" > liquid-asset-ref-$ASSET
	
	##################################################################
	
	echo "Completed without error"


When you have saved the above to the file, edit the variables at the top and of the file and start Liquid QT or liquidd using an argument of ``-server=1`` to allow the Liquid client to communicate with it. Execute the script from the directory you created it in by opening a Terminal session and running:

.. code-block:: bash

	bash issue_and_prepare_register.sh

In order to register the asset just created:

* Wait a couple of minutes for the issuance transaction to confirm.

* Place the 'liquid-asset-proof-' file in a folder named '.well-known' in the root of your domain.

* Run the 'register_asset.sh' script. 

For example, if your domain was www.your-example-domain-here.com and the asset id generated was 123abc (it will of course be much longer) then the file generated would be named:

.. code-block:: text

	liquid-asset-proof-123abc

The domain variable in the code above would be set to:

.. code-block:: text

	your-example-domain-here.com

So the path used to check asset to domain registry would end up being: 

.. code-block:: text

	www.your-example-domain-here.com/.well-known/liquid-asset-proof-123abc

Once that file is accessible you can then run the 'register_asset.sh' script and, when the required checks against the domain and issuance transaction have been made, the registration will be found on Blockstream's `Liquid Asset Registry`_.
