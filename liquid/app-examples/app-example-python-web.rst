------------------
Python Web (Flask)
------------------

In this example we will be using Python (running within the `Flask <http://flask.pocoo.org/>`_ web framework) to make a Remote Procedure Call (RPC) to the Liquid daemon (liquidd), which you will need to start using the config file settings :ref:`outlined above <config>`. 

Our aim is simply to make a call to Liquid using RPC by executing some basic Python code. We will be using the popular `AuthServiceProxy <https://github.com/jgarzik/python-bitcoinrpc>`_ Python JSON-RPC interface to handle the connection, authentication and data typing for us as we communicate with our node. We'll also use `virtualenv <https://virtualenv.pypa.io/>`_ to create an isolated environment to run our code in.

You can check if Python is installed on your system by following the steps `here <https://wiki.python.org/moin/BeginnersGuide/Download>`_. We'll assume you already have Python3 installed.

We will use the Python code from the previous :ref:`Python example <python-app>` and have it run from Flask so that we can write the output to a web page.

Assuming you have already installed the :ref:`prerequisites <python-reqs>` from the previous example (i.e. python3-pip, virtualenv), we just need to set up the environment and install Flask:

.. code-block:: bash

	cd
	mkdir liquidpythonflask
	cd liquidpythonflask
	virtualenv venv
	source venv/bin/activate
	pip3 install python-bitcoinrpc
	pip3 install Flask

Before we try running any code make sure the Liquid daemon is running.

Create a file named liquidpythonflask.py, into which you should paste the following code. We are using AuthServiceProxy to handle the connection, authentication and data typing for us as we communicate with our Liquid node.

.. tip:: Python requires that lines are indented correctly. Make sure the code below is copied correctly and is using 4 spaces for indenting lines. Also note that some of the lines below wrap when viewed in a browser.

.. code-block:: python

	from flask import Flask
	from bitcoinrpc.authproxy import AuthServiceProxy, JSONRPCException

	app = Flask(__name__)
	 
	@app.route("/")
	def liquid():
	    rpc_port = 18884
	    rpc_user = 'user1'
	    rpc_password = 'password1'

	    try:
		rpc_connection = AuthServiceProxy("http://%s:%s@127.0.0.1:%s"%(rpc_user, rpc_password, rpc_port))
	    
		result = rpc_connection.getwalletinfo()
	    
	    except JSONRPCException as json_exception:
		return "A JSON RPC Exception occured: " + str(json_exception)
	    except Exception as general_exception:
		return "An Exception occured: " + str(general_exception)

	    return str(result['balance']['bitcoin'])
	 
	if __name__ == "__main__":
	    app.run()

Start the web server and execute our code:

.. code-block:: bash

	FLASK_APP=liquidtutorial.py flask run

You should see the web server startup. Now all you have to do is open a browser and go to:

http://127.0.0.1:5000

Which simply writes the result of the "balance" call to the page.

The example is intended to get you up and running. You now have a functioning setup which you can use as a building block for further development using Flask.
