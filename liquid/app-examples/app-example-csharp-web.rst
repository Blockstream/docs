------------------
C# .NET Core (MVC)
------------------

The .NET Core framework runs on Linux, Windows and Mac OS. We will use C#, the most popular .NET language, within an MVC web application to make a Remote Procedure Call (RPC) to the Liquid daemon, which you will need to start using the config file settings :ref:`outlined above <config>`. 

Our aim is simply to make a call to Liquid using RPC by executing some basic C# code.

**Installing the .NET Core SDK**

Before starting, check that you have installed the required :ref:`prerequisites <csharp-reqs>` from the previous example.

**Creating the application**

We will be using the Model, View, Controller (MVC) pattern for our app. We will also be using an existing class to handle the calls to Liquid daemon and simplify our code. We will amend the default Home controller to call this class and pass the data it returns back to the relevant View using a new Model that we'll also create.

To get started, create a directory for our MVC app and move into it:

.. code-block:: bash

	mkdir LiquidMVC
	cd LiquidMVC

Use the dotnet tool to create a new template mvc site. This will set up the required folder structure, files, and configuration needed to serve the web app:

.. code-block:: bash

	dotnet new mvc

Check it is set up correctly by running the app:

.. code-block:: bash

	dotnet run

When it says the application has started, browse to https://localhost:5001

You should see the template site running, all we have to do is add our own code to what's already there.

Shut the app down using Ctrl+C.

Now we will start adding our own code so that we can query our Liquid node and display the results on the app's home page.

Create a new file named "dotnetcoreDynamicJSON-RPC.cs" in the LiquidMVC directory and paste in the `raw code <https://raw.githubusercontent.com/wintercooled/dotnetcoreDynamicJSON-RPC/master/dotnetcoreDynamicJSON-RPC.cs>`_ of the "Dynamic JSON RPC class" and save it. Alternatively, clone the `GitHub repository <https://github.com/wintercooled/dotnetcoreDynamicJSON-RPC>`_ into a different directory and copy the dotnetcoreDynamicJSON-RPC.cs file into your LiquidMVC directory.

This class is a C# wrapper class intended to enable simple dynamic JSON RPC calls to Bitcoin, Liquid, Elements, and other RPC enabled daemons. The project, along with examples of how to use it, can be found in the `repository <https://github.com/wintercooled/dotnetcoreDynamicJSON-RPC>`_. All we need for now though is to copy the code from the raw link

That class enables us to easily query our node. We’ll add the code to call it soon, first we need a way to pass the data that we get back from our node to the web page for display. For that we need a Model. We’ll create a simple one that you can add to later.

Create a new file named "ExampleNodeInfo.cs" in LiquidMVC/Models and paste the following into it:

.. code-block:: C#

	using System;

	namespace LiquidMVC.Models
	{
	    public class ExampleNodeInfo
	    {
		public string Balance { get; set; }
		
		public string Message { get; set; }
		//Add whatever other properties you want here
	    }
	}

Now we have a way to get data from our node and a Model that lets us pass the data to the View for display.

Open the HomeController.cs file in LiquidMVC/Controllers. We will be adding code to call the Dynamic JSON RPC class to get data from our Liquid node, populate our Model with the results and hand the Model to the landing page's View for display.

To the top of the HomeController.cs file, add the following:

.. code-block:: C#

	using DotnetcoreDynamicJSONRPC;

Then replace the Index method so it looks like the following. Note that some of the lines below wrap when viewed in a browser.

.. code-block:: C#

	public IActionResult Index()
	{
	    // We will be using a Liquid node in this example. 
	    // It is easy to switch to use a Bitcoin, Liquid or Elements node.
	    // You need to change these to make sure you can authenticate against the daemon you are running:
	    string rpcUrl = "http://localhost";
	    string rpcPort = "18884";
	    string rpcUsername = "user1";
	    string rpcPassword = "password1";

	    // For examples and notes on how to use the dotnetcoreDynamicJSON-RPC tool and its JSON helper methods please see:
	    // https://github.com/wintercooled/dotnetcoreDynamicJSON-RPC            

	    // Initialise an instance of the dynamic dotnetcoreDynamicJSON_RPC class.
	    dynamic dynamicRPC = new DynamicRPC(rpcUrl, rpcPort, rpcUsername, rpcPassword);

	    // Initialise our model that will be passed to the view
	    var nodeInfo = new ExampleNodeInfo();

	    if (dynamicRPC.DaemonIsRunning())
	    {
		try
		{
		    // Get the JSON result of the 'getwalletinfo' RPC on the Liquid node.
		    string balance = dynamicRPC.getwalletinfo();

		    // Use the DotnetcoreDynamicJSONRPC 'GetProperty' string helper to return the property value we want.
		    balance = balance.GetProperty("result.balance.bitcoin");

		    // Populate the model
		    nodeInfo.Balance = balance;
		}
		catch (Exception e)
		{
		    nodeInfo.Message = e.Message;
		}
	    }
	    else
	    {
		nodeInfo.Message = "Could not communicate with daemon";
	    }

	    // Return the view and the associated model we have populated
	    return View(nodeInfo);
	}

Next, edit the Index.cshtml file in LiquidMVC/Views/Home and replace all the existing content with the following code. The code takes our Model and displays the data in it on the default web page.

.. code-block:: text

	@model LiquidMVC.Models.ExampleNodeInfo

	@{
	    ViewData["Title"] = "Index";
	}

	<h2>Example Node Info</h2>

	@{
	    if (Model.Message != "")
	    {
		<h3>@Model.Message</h3>
	    }
	}

	<div>
	    <h4>Basic Wallet Info</h4>
	    <hr />
	    <dl class="dl-horizontal">
		<dt>
		    @Html.DisplayNameFor(model => model.Balance)
		</dt>
		<dd>
		    @Html.DisplayFor(model => model.Balance)
		</dd>
	    </dl>
	</div>

**Running the application**

Before you try running the code, make sure the Liquid daemon is running. Make sure your terminal is at the LiquidMVC directory level and run the following:

.. code-block:: bash

	dotnet run

When it says the application has started, browse to https://localhost:5001

The balance of the Liquid node’s wallet is displayed on the page.

That should have got you up and running and in order to extend your application, you can look at the examples on the Dynamic JSON RPC class GitHub `site <https://github.com/wintercooled/dotnetcoreDynamicJSON-RPC>`_.





