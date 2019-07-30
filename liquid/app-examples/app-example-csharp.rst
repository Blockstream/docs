------------
C# .NET Core
------------

The .NET Core framework runs on Linux, Windows and Mac OS. We will use C#, the most popular .NET language, to make a Remote Procedure Call (RPC) to the Liquid daemon, which you will need to start using the config file settings :ref:`outlined above <config>`. 

Our aim is simply to make a call to Liquid using RPC by executing some basic C# code.

.. _csharp-reqs:

**Installing the .NET Core SDK**

You can skip this step if you already have the .NET Core SDK installed. The installation instructions are based upon Linux and will vary depending on which OS you are using.

Before installing the .NET Core SDK, you'll need to register the Microsoft key used to validate the required repository, register the product repository itself, and then install the required dependencies.

Open a command prompt and run the following two commands.

Note that the command you run after "wget" is dependant on the which distribution of Linux you are running. Please check the .NET SDK `download page <https://dotnet.microsoft.com/download/linux-package-manager/rhel/sdk-current>`_ to get the right one. There are only two lines below so note the text wrap. The first starts with "wget", the second with "sudo".

.. code-block:: bash

	wget -q packages-microsoft-prod.deb https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
	sudo dpkg -i packages-microsoft-prod.deb

Now we can install the .NET Core SDK:

.. code-block:: bash

	sudo apt-get install apt-transport-https
	sudo apt-get update
	sudo apt-get install dotnet-sdk-2.1.4

**Creating the application**

Create a simple template console app:

.. code-block:: bash

	dotnet new console -o liquiddotnet
	cd liquiddotnet
	dotnet run

That will create a file in $HOME/liquiddotnet called Program.cs, execute the code within and output the following:

.. code-block:: bash

	Hello World!

Edit the Program.cs file in $HOME/liquiddotnet using a text editor, change the contents to the following and then save the file: (note that some of the lines below wrap when viewed in a browser)

.. code-block:: C#

	using System;
	using System.Net.Http;
	using System.Threading.Tasks;

	namespace liquiddotnet
	{
	    class Program
	    {
		private static HttpClient client = new HttpClient();
		
		static void Main(string[] args)
		{
		    MainAsync().GetAwaiter().GetResult();
		}

		private static async Task MainAsync()
		{
		    string url = "http://user1:password1@localhost:18884";

		    var contentData = new StringContent(@"{""method"": ""getwalletinfo"", ""jsonrpc"": ""2.0""}", System.Text.Encoding.UTF8, "application/json");
		    using (HttpResponseMessage response = await client.PostAsync(url, contentData))
		    using (HttpContent content = response.Content)
		    {
		        string data = await content.ReadAsStringAsync();
		        
		        if (null != data)
		        {
		            Console.WriteLine(data);
		        }
		    }
		}
	    }
	}

**Running the application**

Before you try running any code, make sure the required daemon is running.

Move to the the directory with the file in, compile and run the application:

.. code-block:: bash

	cd
	cd liquiddotnet
	dotnet run

Which outputs the results of the getwalletinfo command.

As an application would be making multiple calls to the Liquid daemon via RPC you will probably want to move the code that actually does the request and response work into its own function. An example of how to do this is the `dynamic JSON RPC class <https://github.com/wintercooled/dotnetcoreDynamicJSON-RPC>`_, a C# wrapper class intended to enable simple dynamic JSON RPC calls to Bitcoin, Liquid, Elements and other RPC enabled daemons.

The code above is a starting point to get you up and running and you now have a functioning setup which you can use as a building block for further Liquid development.












