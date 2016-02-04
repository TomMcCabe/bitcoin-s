#Bitcoin-S in Sidechain Elements Alpha

###Bitcoin-S:
* Bitcoin-S - Bitcoin implementation in Scala: https://github.com/bitcoin-s/bitcoin-s/blob/master/README.md

###Sidechain Elements Alpha
* Build and running alpha instructions: https://github.com/ElementsProject/elements/blob/alpha/alpha-README.md

###Using Bitcoin-S
Once Bitcoin-S and Elements are built, we can use Bitcoin-S to process sidechain RPCs with Elements in Scala.

After starting a server and creating an instance with Bitcoin-S in the SBT console (see Bitcoin-S build), we can use `ScalaRPCClient` and the `sendCommand` method to perform RPCs with any sidechain, just as we would with Bitcoin. 

```
cd bitcoin-s
sbt console
import org.bitcoins.protocol.server.ServerInitiation
import org.bitcoins.rpc.ScalaRPCClient
```

```
scala> val server = new ServerInitiation("alphad -rpcuser=<user> -rpcpassword=<pass> -testnet -rpcconnect=127.0.0.1 -rpcconnectport=18332 -tracksidechain=all -txindex -blindtrust=true -daemon") //see alpha-README in Elements for starting alphad outside of sbt console
server: org.bitcoins.protocol.server.ServerInitiation = org.bitcoins.protocol.server.ServerInitiation@4a71abb6

scala> Bitcoin server starting  

scala> val alpha = new ScalaRPCClient("alpha-cli", "-testnet") //starts the RPC client
```

The default Bitcoin-S RPCs are built for bitcoin and may not work for your blockchain, especially those RPCs that return detailed data. However, the basics should work: 

```
scala> alpha.getBlockCount
res2: Int = 121065

scala> alpha.getBalance
res3: Double = 0.00488335
```

Bitcoin-S is built to be used for any blockchain. Theoretically with the `sendCommand("pass_argument_as_string")`, we can execute any RPC on any blockchain and get the return value just as you would in a cmd shell. Continuing from the previous example:

```
scala> alpha.sendCommand("sendtoaddress 2NEdXaa9cfDH4DxBbZSLymonsexvFZ9ip4d .001")
res35: String = "160b6f2ab3285492e4b33aaff4232b1b35ea03de2d13a52ef66d424f3a14a87d" //returns txid
```
