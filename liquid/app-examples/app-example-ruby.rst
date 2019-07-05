----
Ruby
----

In this example we will be using Ruby to make a Remote Procedure Call (RPC) to the Liquid daemon (liquidd), which you will need to start using the config file settings :ref:`outlined above <config>`. 

Our aim is simply to make a call to Liquid using RPC by executing some basic Ruby code.

You can check if Ruby is installed on your operating system, and install it if not, by following the steps on the `Ruby language website <https://www.ruby-lang.org/en/documentation/installation/>`_.

Create a new file named "liquidrpcruby.rb" and paste the code below into it:

.. code-block:: Ruby

	require 'net/http'
	require 'uri'
	require 'json'

	class LiquidRPC
	  def initialize(service_url)
	    @uri = URI.parse(service_url)
	  end

	  def method_missing(name, *args)
	    post_body = { 'method' => name, 'params' => args, 'id' => 'jsonrpc' }.to_json
	    resp = JSON.parse( http_post_request(post_body) )
	    raise JSONRPCError, resp['error'] if resp['error']
	    resp['result']
	  end

	  def http_post_request(post_body)
	    http    = Net::HTTP.new(@uri.host, @uri.port)
	    request = Net::HTTP::Post.new(@uri.request_uri)
	    request.basic_auth @uri.user, @uri.password
	    request.content_type = 'application/json'
	    request.body = post_body
	    http.request(request).body
	  end

	  class JSONRPCError < RuntimeError; end
	end

	if $0 == __FILE__
	  liquid = LiquidRPC.new('http://user1:password1@127.0.0.1:18884')
	 
	  p liquid.getblockcount
	end

Execute the code from the command line:

.. code-block:: bash

	ruby liquidrpcruby.rb

The output will show the current block count.

You now have a functioning setup which you can use as a building block for further development.
