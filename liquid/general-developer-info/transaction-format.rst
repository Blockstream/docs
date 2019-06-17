-----------------------------------
Bitcoin v Liquid Transaction Format
-----------------------------------

As Liquid is built on top of the Bitcoin codebase, the format of the transaction is similiar in many ways, but it does contain some extra sections and slight differences in the way some data is returned.

The examples below show a transaction within the calling node's wallet (using gettransaction) and a transaction that is not in the node's wallet (using decoderawtransaction). Peg-out transaction properties are show later for reference.

Returned data from gettransaction
---------------------------------

The results of calling gettransaction returns something similar to that below. Differences between a Liquid and Bitcoin call to gettransaction have been highlighted and explained.

The amount is a data object in Liquid which shows the amount of each asset held. In Bitcoin this is just a simple property and value pair:

Liquid

.. code-block:: bash

	{
	  "amount": {
	    "bitcoin": 0.00000000
	  },

Bitcoin

.. code-block:: bash

	{
 	  "amount": 0.00000000,

The fee amount is a data object in Liquid which shows the amount of the asset used to pay fees. In Bitcoin this is just a simple property and value pair:

Liquid

.. code-block:: bash

	"fee": {
	  "bitcoin": -0.00043100
	},

Bitcoin

.. code-block:: bash

	"fee": -0.00003740,

Bitcoin & Liquid

.. code-block:: bash

	"confirmations": 101,
	"blockhash": "7ebd6c337a0f414f22d0246aa7ec7feb3742030b2ab9fdb660120b717dedaf94",
	"blockindex": 1,
	"blocktime": 1554811774,
	"txid": "5fa4ef7898eb704dbaea16634ab68302826c42198c9a5f9ff3e60622f83aac90",
	"walletconflicts": [],
	"time": 1554811735,
	"timereceived": 1554811735,
	"bip125-replaceable": "no",
	"details": [
	  {
  	    "address": "2NGSJetH6NMAMimt3MBUWe4Eb9zetevw2Tt",
  	    "category": "send",
  	    "amount": -1.00000000,

There are fields in Liquid showing the asset being transacted along with the amount and asset blinders (when applicable):

Liquid

.. code-block:: bash

	    "amountblinder": "07284ca3100c24a2e3bc0483ade3a462468074b7b207375ccfb0f27846c99ea2",
	    "asset": "b2e15d0d7a0c94e4e2ce0fe6e8691b9e451377f6e46e8045a86f7c4b5d4f0f23",
	    "assetblinder": "846663fc5318aa9ea602449c8d598c9cd43459c0056971eff44fba464d89b328",

Bitcoin & Liquid

.. code-block:: bash

	    "label": "",
	    "vout": 0,
	    "fee": 0.00043100,
	    "abandoned": false
	  },
	  {
	    "address": "2NGSJetH6NMAMimt3MBUWe4Eb9zetevw2Tt",
	    "category": "receive",
	    "amount": 1.00000000,

Liquid shows the asset being transacted along with the amount and asset blinders (when applicable):

Liquid

.. code-block:: bash

	    "amountblinder": "07284ca3100c24a2e3bc0483ade3a462468074b7b207375ccfb0f27846c99ea2",
	    "asset": "b2e15d0d7a0c94e4e2ce0fe6e8691b9e451377f6e46e8045a86f7c4b5d4f0f23",
	    "assetblinder": "846663fc5318aa9ea602449c8d598c9cd43459c0056971eff44fba464d89b328",

Bitcoin & Liquid

.. code-block:: bash

	    "label": "",
	    "vout": 0
	  }
	],
	"hex": "0200000001…
	<continues>

Returned data from decoderawtransaction
---------------------------------------

The results of calling decoderawtransaction returns something similar to that below. Differences between a Liquid and Bitcoin call to gettransaction have been highlighted and explained.

Bitcoin & Liquid

.. code-block:: bash

	{
	  "txid": "5fa4ef7898eb704dbaea16634ab68302826c42198c9a5f9ff3e60622f83aac90",
	  "hash": "6d5f1b8965db82ef37a6ec7381de4a09f0369a3ec3ac236d17384738974232b2",
	  "wtxid": "6d5f1b8965db82ef37a6ec7381de4a09f0369a3ec3ac236d17384738974232b2",
	  "withash": "95151cf350baf2c99c0ff36e1f4110989b1f33e0d32d8c302c95ad5ecd79e8a2",
	  "version": 2,
	  "size": 7525,
	  "vsize": 2155,
	  "weight": 8620,
	  "locktime": 303,
	  "vin": [
	    {
	      "txid": "956e34fb9ace5709183e8bed4555c5d701781735633cfa17e9c1c68fb2462ee1",
	      "vout": 0,
	      "scriptSig": {
	      "asm": "00144e4f612e206ec2a27cd0818630bb3304526cc56a",
	      "hex": "1600144e4f612e206ec2a27cd0818630bb3304526cc56a"
	    },

The 'is_pegin' field in Liquid denotes if the transaction was a peg-in transaction:

Liquid

.. code-block:: bash

	  "is_pegin": false,

Bitcoin & Liquid

.. code-block:: bash

	  "sequence": 4294967293,
	  "txinwitness": [
	    "304402201da54cfd...<continues>",
	    "039c1a8daee141ef32399e3dab9e04bd9c4239342577e94e9e58b644c8dc58ba98"]
	  }],
	"vout": [
	  {

Some additional fields in Liquid are linked to the use of Confidential Transactions. For each vout:

Liquid

.. code-block:: bash

	  "value-minimum": 0.00000001,
	  "value-maximum": 687.19476736,
	  "ct-exponent": 0,
	  "ct-bits": 36,
	  "valuecommitment": "0844778d24db8b3454924e3b77d2aa00b4bd57bc20cb852a65238a336b93db7ac6",
	  "assetcommitment": "0b96a62e05fcf65a50ad58643a603f21bb033172336c653840accbae54e9fe7dd7",
	  "commitmentnonce": "02acc6606bdd8c65bdaeadf1eefec726d0d3b777586922b6255c557bb8e43ac946",
	  "commitmentnonce_fully_valid": true,

Whereas Bitcoin uses the 'value' field:

Bitcoin

.. code-block:: bash

	  "value": 48.99996260,

Bitcoin & Liquid

.. code-block:: bash

	  "n": 0,
	  "scriptPubKey": {
  	  "asm": "OP_HASH160 fe635cfa8bb86040c3871d7e973e7e155583f7f9 OP_EQUAL",
  	  "hex": "a914fe635cfa8bb86040c3871d7e973e7e155583f7f987",
  	  "reqSigs": 1,
  	  "type": "scripthash",
  	  "addresses": [
    	    "2NGSJetH6NMAMimt3MBUWe4Eb9zetevw2Tt"
  	  ]
	  }
	},

The fee in Liquid is explicitly stated as a vout and is not derived from deducting the vout total from the vin total as in Bitcoin:

Liquid

.. code-block:: bash

	{
	  "value": 0.00043100,
	  "asset": "b2e15d0d7a0c94e4e2ce0fe6e8691b9e451377f6e46e8045a86f7c4b5d4f0f23",
	  "commitmentnonce": "",
	  "commitmentnonce_fully_valid": false,
	  "n": 2,
	  "scriptPubKey": {
	  "asm": "",
	  "hex": "",
  	  "type": "fee"
	  }
	}


Peg-out data
------------

In a Peg-out transaction, an addition set of fields will be included in the vout section:

Liquid

.. code-block:: bash

	"pegout_chain": "000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f",
        "pegout_asm": "OP_DUP OP_HASH160 bd3432857da35ae7c93a05c911208aad0d6fa695 OP_EQUALVERIFY OP_CHECKSIG",
        "pegout_hex": "76a914bd3432857da35ae7c93a05c911208aad0d6fa69588ac",
        "pegout_reqSigs": 1,
        "pegout_type": "pubkeyhash",
        "pegout_addresses": [
          "1JFREzVxcivkeRQpngZ9ERYaoFuSP9o3vG"
        ]

