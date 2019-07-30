----
Perl
----

In this example we will be using Perl to make a Remote Procedure Call (RPC) to the Liquid daemon, which you will need to start using the config file settings :ref:`outlined above <config>`. 

Our aim is simply to make a call to Liquid using RPC by executing some basic Perl code.

Perl can be installed from https://www.perl.org and comes pre-installed on Ubuntu.

We will be using the `JSON::RPC::Client <https://metacpan.org/pod/release/MAKAMAKA/JSON-RPC-0.95/lib/JSON/RPC/Client.pm>`_ Perl implementation of a JSON-RPC client which will make the rpc calls and results handling simpler.

To install JSON::RPC::Client open the CPAN tool from the terminal:

.. code-block:: bash

	perl -MCPAN -e shell

And from within the cpan console, install the RPC Client:

.. code-block:: bash

	install JSON::RPC::Client

And then exit the cpan tool:

.. code-block:: bash

	quit

You must close the terminal window so that when we run the code below it will pick up the new environment variables written.

Create a new file named "liquidrpcperl.pl" and paste the code below into it:

.. code-block:: Perl

	use JSON::RPC::Client;
	use Data::Dumper;

	my $client = new JSON::RPC::Client;

	$client->ua->credentials(
	    'localhost:18884', 'jsonrpc', 'user1' => 'password1'
	    );

	my $uri = 'http://localhost:18884/';
	my $obj = {
	    method  => 'getwalletinfo',
	    params  => [],
	};

	my $res = $client->call( $uri, $obj );

	if ($res){
	    if ($res->is_error) { print "Error : ", $res->error_message; }
	    else { print Dumper($res->result); }
	} else {
	    print $client->status_line;
	}

Run the code from the command line (remember that this must be a new terminal window from the one we used before to install the RPC Client):

.. code-block:: bash

	perl liquidrpcperl.pl

The output will show wallet information.

You now have a functioning setup which you can use as a building block for further development.
