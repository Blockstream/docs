--------------
Basic Commands
--------------

The Liquid Client (liquid-cli) application allows you to issue Remote Procedure Call (RPC) commands to the Liquid Daemon (liquidd) or Liquid Core GUI (liquid-qt) from the terminal.

A full list of all the commands you can issue over RPC can be be seen by running:

.. code-block:: bash

	liquid-cli help

To see further information on how to use a certain command you can append the name of the command like this:

.. code-block:: bash

	liquid-cli help sendtoaddress

This returns examples of how to use the command, a list of arguments that can be passed to it and and the format of the results returned.

If you run the above code you will see that the sendtoaddress accepts 2 mandatory arguments and has 8 additional optional arguments.

For example, to use the sendtoaddress command to send an amount of 1 L-BTC and subtract the fee from the amount being sent we would run: 

.. code-block:: bash

	liquid-cli sendtoaddress AzppUfWkYpjiThupf2t6Kn1yPTgseg3buBexuMSbZxpPWAsQcucHXFpr4HHGFQAiyBiddvcjAYyoVeMD 1 "" "" true

The result returned from sendtoaddress is the transaction id. For example:

.. code-block:: text

	466c7da2555f0e1e8eff0188cbf7ba0e44fae03a61b81a1226c34ed785e4bb58

The result was returned as a string value. Many results are returned as JSON formatted data however. Using getwalletinfo as an example:

.. code-block:: bash

	liquid-cli getwalletinfo

This returns JSON formatted data similar to that below:

.. code-block:: json

	{
	  "walletname": "",
	  "walletversion": 169900,
	  "balance": {
	    "bitcoin": 100.00000000,
	    },
	  "unconfirmed_balance": {
	    "bitcoin": 0.00000000
	  },
	  "immature_balance": {
	    "bitcoin": 0.00000000
	  },
	  "txcount": 10,
	  "keypoololdest": 1557907592,
	  "keypoolsize": 1000,
	  "keypoolsize_hd_internal": 999,
	  "paytxfee": 0.00000000,
	  "hdseedid": "17cea58f08ed56e975216699061df4a75002fcf8",
	  "hdmasterkeyid": "17cea58f08ed56e975216699061df4a75002fcf8",
	  "private_keys_enabled": true
	}

In the remaining code examples we will continue using the terminal and the liquid-cli application to send commands to liquidd. Other languages can be used to send RPC commands as mentioned previously.
