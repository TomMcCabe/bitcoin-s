##Bitcoin-S 

Bitcoin-S is an implementation of the Bitcoin protocol in the Scala programming language. 

* Scala: http://www.scala-lang.org/
* Bitcoin-Core: https://bitcoin.org/en/download

#####GETTING STARTED

Import/build the project using your preferred IDE. 

We can start a bitcoin server in a Scala console by first importing the serverInitiation file:

    import org.scalacoin.protocol.server.ServerInitiation

Create a instance of the serverInitiation class: 

    val createNewServer = new ServerInitiation
  
Now start the server of your choice:

    createNewServer.initiateServer([Server as a string, network optional])

This will start a bitcoin server on the test network: 

    createNewServer.initiateServer("bitcoind -testnet")
  
There are 3 predefined methods for bitcoin on the main, test, and regression networks.
  
    createNewServer.bitcoinMain
    createNewServer.bitcoinTestNet
    createNewServer.bitcoinRegTest

#####RPCs
  
For a complete list of RPCs for bitcoin and their descriptions, refer to https://bitcoin.org/en/developer-reference#remote-procedure-calls-rpcs. 

#####Demonstration

Bitcoin-S has a RPC client built in. Bitcoin uses a separate program for RPCs. Starting a server uses 'bitcoind', RPCs use 'bitcoin-cli'. 

In a Scala console, import it:

    import org.scalacoin.protocol.rpc.ScalaRPCClient

Create a new instance:

    val RPC = new ScalaRPCClient(server[string], network[string][optional])

Example creating a RPC for the bitcoin test network:

    val testNet = new ScalaRPCClient("bitcoin-cli", "-testnet")

Here's the base command to build an RPC. With this, you can send a bitcoin RPC through the Scala console:

    sendCommand([Bitcoin RPC][String])

Getting the blockcount:

    testNet.sendCommand("getblockcount")
Returns: 

    String ="651500"

We've predefined some RPCS to return parsed JSON values to easily interact with the data objects:

    getBlockCount
    getBestBlockHash
    getBlockChainInfo
    getMiningInfo
    getNetworkInfo
    getMemPoolInfo
    getPeerInfo
    getTxOutSetInfo
    getWalletInfo
    getDifficulty
    getNewAddress
    getRawChangeAddress
    getBalance

To demonstrate since it's a small object, let's get the memory pool info. Returning MemPoolInfo using sendCommand will return the value as a string:

    testNet.sendCommand("getmempoolinfo")
Returns:

    String =
    "{
    "size" : 19,
    "bytes" : 5300
    }
    "

However, using our RPC:

    testNet.getMemPoolInfo
Returns JSON value:

    org.scalacoin.protocol.blockchain.MemPoolInfo = MemPoolInfoImpl(17,4948)
Which can be used to handle the metadata:

    testNet.getMempoolInfo.size 
Returns:

    Int = 19

Stop the server with `stop` RPC:

    testNet.stop
