----
Java
----

In this example we will be using Java to make a Remote Procedure Call (RPC) to the Liquid daemon, which you will need to start using the config file settings :ref:`outlined above <config>`. 

Our aim is simply to make a call to Liquid using RPC by executing some basic Java code.

The example code doesn't require any external dependencies, but you do need the Java compiler (javac) as well as the Java runtime environment itself. These can be installed on Ubuntu using the command:

.. code-block:: bash

	sudo apt install default-jdk

Create a new file named "liquidrpcjava.java" and paste the code below into it:

.. code-block:: Java

	import java.io.BufferedReader;
	import java.io.IOException;
	import java.io.InputStreamReader;
	import java.io.OutputStream;
	import java.net.HttpURLConnection;
	import java.net.URL;
	import java.net.Authenticator;
	import java.net.PasswordAuthentication;

	public class liquidrpcjava {
		
		public static void main (String []args) throws IOException{
			URL url = new URL ("http://127.0.0.1:18884");
			String rpcuser ="user1";
			String rpcpassword ="password1";
			
			Authenticator.setDefault(new Authenticator() {
		    protected PasswordAuthentication getPasswordAuthentication() {
		        return new PasswordAuthentication (rpcuser, rpcpassword.toCharArray());
		    }
		});
	  
			HttpURLConnection conn = (HttpURLConnection)url.openConnection();
			conn.setRequestMethod("POST");
			conn.setRequestProperty("Content-Type", "application/json; utf-8");
			conn.setRequestProperty("Accept", "application/json");
			conn.setDoOutput(true);
					
			String json = "{\"method\": \"getwalletinfo\", \"jsonrpc\": \"2.0\"}";
			
			try(OutputStream os = conn.getOutputStream()){
				byte[] input = json.getBytes("utf-8");
				os.write(input, 0, input.length);			
			}
			
			try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), "utf-8"))){
				StringBuilder sb = new StringBuilder();
				String line = null;
				
				while ((line = br.readLine()) != null) {
					sb.append(line.trim());
				}
				
				System.out.println(sb.toString());
			}
		}
	}

Compile and run the code from the command line:

.. code-block:: bash

	javac liquidrpcjava.java
	java liquidrpcjava

The output will show wallet information.

You now have a functioning setup which you can use as a building block for further development.
