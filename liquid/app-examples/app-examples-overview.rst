---------------------------
Liquid Application Overview
---------------------------

Applications can make use of Liquid's Remote Procedure Call (RPC) interface to develop applications on Linux, Windows and Mac OS in any language capable of handling RPC request-response messaging over HTTP.

Examples are provided in Python, C#, Ruby, Go, Perl, and Java. The Python and C# examples are expanded to cover web applications using Flask and the .NET Core MVC frameworks. 

In the :ref:`Developer Guide <developer-guide>` section, the terminal is used to send commands to the Liquid client (elements-cli), which in turn issues RPC commands to the Liquid daemon (elementsd) or the Liquid Core GUI (elements-qt). This configuration will be familiar to those who develop for Bitcoin.

The communication between daemon and client is possible because, when started in server or daemon mode, elementsd initiates an HTTP server and listens for requests being made to it on a port specified in the elements.conf file.

Requests to the daemon are made by posting JSON formatted data to the HTTP server port that elementsd or elements-qt are listening on. The request is processed and the results are returned as JSON formatted data.

Verification details of the credentials needed to make these calls are stored in the elements.config file so the client making the RPC requests can satisfy the authentication checks of the daemon.

Take a look in the elements.conf file and notice the rpc related settings. If there are none then you will have to add some. The "chain" setting tells Liquid to run on the live Liquid network (liquidv1) or another network, such as regtest (elementsregtest). The following settings tell Liquid to start in regtest mode and that it can be accessed over RPC using the username "user1" and the password "password1". You should change the values used to a secure username and password.

.. code-block:: text

	# Start as a background process and accept RPC
	daemon=1

	# Set RPC details for client authentication
	rpcuser=user1
	rpcpassword=password1

	# Regest settings 
	chain=elementsregtest
	elementsregtest.rpcport=18884
	elementsregtest.port=18886
	# To connect to other local liduidd instances:
	elementsregtest.connect=localhost:18887


	# Live network settings:
	# chain=liquidv1
	# rpcport=18884

	# To validate peg-ins 
	validatepegin=1

	# RPC details (as set in bitcoin.conf)
	mainchainrpcport=18888
	mainchainrpcuser=userOfBitcoinRPCHere
	mainchainrpcpassword=passwordOfBitcoinRPCHere

	# Other config settings you want to use can go here:

In our examples, we will be connecting to elementsd in regtest mode, using the regtest authentication details and port number above to send requests to the Liquid daemon. You should change the example code to match what you have set in your own elements.conf.

Please note that the examples listed below are intended to get you to the point where you have a functioning setup which you can use as a building block for further development.
