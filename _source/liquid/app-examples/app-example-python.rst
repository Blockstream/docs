------
Python
------
.. _python-app:

In this example we will be using Python to make a Remote Procedure Call (RPC) to the Liquid daemon (liquidd), which you will need to start using the config file settings :ref:`outlined above <config>`. 

Our aim is simply to make a call to Liquid using RPC by executing some basic Python code. We will be using the popular `AuthServiceProxy <https://github.com/jgarzik/python-bitcoinrpc>`_ Python JSON-RPC interface to handle the connection, authentication and data typing for us as we communicate with our node. We'll also use `virtualenv <https://virtualenv.pypa.io/>`_ to create an isolated environment to run our code in.

You can check if Python is installed on your system by following the steps `here <https://wiki.python.org/moin/BeginnersGuide/Download>`_. We'll assume you already have Python3 installed.

.. _python-reqs:

First we will need to install a few prerequisites. From the terminal run the following commands one after another:

.. code-block:: bash

	sudo apt-get install python3-pip
	sudo pip3 install virtualenv

Create a directory named "liquidpython" and move into it:

.. code-block:: bash

	cd
	mkdir liquidpython
	cd liquidpython

Set up and activate virtualenv in that directory:

.. code-block:: bash

	virtualenv venv
	source venv/bin/activate

Use pip to install python-bitcoinrpc within the environment:

.. code-block:: bash

	pip3 install python-bitcoinrpc

That will have set up all we need for our Python example code.

Create a new file named **liquidpython.py** in the "liquidpython" directory and paste the code below into it.

.. code-block:: python

	from __future__ import print_function
	from bitcoinrpc.authproxy import AuthServiceProxy, JSONRPCException

	rpc_port = 18884
	rpc_user = 'user1'
	rpc_password = 'password1'

	try:
	    rpc_connection = AuthServiceProxy("http://%s:%s@127.0.0.1:%s"%(rpc_user, rpc_password, rpc_port))
	    
	    result = rpc_connection.getwalletinfo("bitcoin")
	    
	    print(result["balance"])
	except JSONRPCException as json_exception:
	    print("A JSON RPC Exception occured: " + str(json_exception))
	except Exception as general_exception:
	    print("An Exception occured: " + str(general_exception))

The code defines the details needed to connect to Liquid using RPC commands, sets up the method we want to execute and the parameter we want to pass in, executes the call and prints out the "balance" value from the results.

Before we try running the code make sure the Liquid daemon or Liquid QT are running.

To run our Python code execute the following command:

.. code-block:: bash

	python3 liquidpython.py

The result of which should be the balance is written to the terminal.

When you have finished, deactivate virtualenv:

.. code-block:: bash

	deactivate

When you want to run your code again, activate the environment from within the "liquidpython" directory, run the code and then deactivate when finished:

.. code-block:: bash

	source venv/bin/activate
	python3 liquidrpcpython.py 
	deactivate

Obviously that's a very basic example but you now have a functioning setup which you can use as a building block for further development. The next section takes the code above and implements it within a Python web application using Flask.
