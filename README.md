#Bitcoin-S 

Bitcoin-S is an implementation of the Bitcoin protocol in the Scala programming language. 

##Prerequisites:
* Scala: http://www.scala-lang.org/
* SBT: http://www.scala-sbt.org/download.html
* Bitcoin-Core: https://bitcoin.org/en/download


##Getting Started

Import/build the project using your preferred IDE. 

We can start a bitcoin server in a Scala console by first importing the serverInitiation file and creating a instance of the ServerInitiation class.
```
scala> import org.scalacoin.protocol.server.ServerInitiation
import org.scalacoin.protocol.server.ServerInitiation

scala> val testNet = new ServerInitiation("bitcoind -testnet")
testNet: org.scalacoin.protocol.server.ServerInitiation = org.scalacoin.protocol.server.ServerInitiation@13ad926

```

##RPCs
  
For a complete list of RPCs for bitcoin and their descriptions, refer to https://bitcoin.org/en/developer-reference#remote-procedure-calls-rpcs. 

##Demonstration

Bitcoin-S has a RPC client built in. Bitcoin uses a separate program for RPCs. Starting a server uses `bitcoind`, RPCs use `bitcoin-cli`. 

In a Scala console, import the file and create a new ScalaRPCClient instance. The `sendCommand` method will take official bitcoin RPCs and return their result. In this example, we create an RPC tool for the bitcoin test network and get the block count.

```
scala> import org.scalacoin.rpc.ScalaRPCClient
import org.scalacoin.rpc.ScalaRPCClient

scala> val testNet = new ScalaRPCClient("bitcoin-cli","-testnet")
testNet: org.scalacoin.rpc.ScalaRPCClient = org.scalacoin.rpc.ScalaRPCClient@106ca0dc

scala> testNet.sendCommand("getblockcount")
res8: String = "651604"
```

We've also predefined some RPCS to return parsed JSON values to easily interact with the data objects:

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

To demonstrate since it's a small object, let's get the memory pool info. Returning MemPoolInfo using `sendCommand` will return the value as a string. We can also use our `getMemPoolInfo` RPC to return it as JSON values, which we can be used to handle the metadata inside the JSON objects:

```
scala> testNet.sendCommand("getmempoolinfo")
res10: String =
"{
    "size" : 2,
    "bytes" : 475
}
"

scala> testNet.getMemPoolInfo
res11: org.scalacoin.protocol.blockchain.MemPoolInfo = MemPoolInfoImpl(2,475)

scala> testNet.getMemPoolInfo.size
res12: Int = 2

scala> testNet.getMemPoolInfo.bytes
res13: Int = 475

scala> testNet.stop
res14: String = "Bitcoin server stopping"
```
