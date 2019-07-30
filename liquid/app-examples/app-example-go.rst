--
Go
--

In this example we will be using Go to make a Remote Procedure Call (RPC) to the Liquid daemon, which you will need to start using the config file settings :ref:`outlined above <config>`. 

Our aim is simply to make a call to Liquid using RPC by executing some basic Go code.

To install Go: https://golang.org. Note the importance of setting the PATH environment variable.

Create a directory src/liquid inside your Go workspace directory (probably %HOME/go) so that the full path is: $HOME/go/src/liquid

With that directory create a file named "liquidrpcgo.go" and past the following into it:

.. code-block:: Go

	package main

	import (
	    "log"
	    "bytes"
	    "net/http"
	    "io/ioutil"
	    "encoding/json"
	)

	func main() {
	    url := "http://user1:password1@localhost:18884"
	    
	    var jsonStr = []byte(`{"jsonrpc":"1.0","method":"getblockcount","params":[]}`)

	    request, err := http.NewRequest("POST", url, bytes.NewBuffer(jsonStr))
	    
	    request.Header.Set("Content-Type", "application/json")

	    client := &http.Client{}
	    
	    response, err := client.Do(request)
	    
	    if err != nil {
		panic(err)
	    }
	    defer response.Body.Close()
	    
	    body, _ := ioutil.ReadAll(response.Body)
	    
	    log.Println(string(body))
	    
	    //Use as JSON...
	    var dat map[string]interface{}
	    
	    if err := json.Unmarshal(body, &dat); err != nil {
		panic(err)
	    }
	    
	    blocks := dat["result"].(float64)
	    log.Println(blocks)
	}

Compile and run the code:

.. code-block:: bash

	go build
	./liquid

The code prints out the current block count.

You now have a functioning setup which you can use as a building block for further development.
