## About

It is a demo-DApp based on embark framework, which relies on nodejs, integrates with EVM blockchains (Ethereum), Decentralized Storages (IPFS), and Decentralized communication platforms (Whisper and Orbit).

The following is something about go-ethereum and Embark framework.

## How to run a private net(Embark & Geth)?

### Embark
All peers must use the exact SAME genesis block specified by genesis.json!

```
1. git clone https://github.com/cheersyouran/ethereum_DApp.git
2. cd etherum_DApp
3. add bootnodes to 'config/blockchain.json/privatenet.bootnodes'
		bootnode --genkey=boot.key
		bootnode --nodekey=boot.key
		or create 'datadir/static-nodes.json' set ["enode://dfd047d@[::]:30303"])
4. embark blockchain privatenet
5. embark run privatenet
6. chrome open localhost:8000
```

If you want to create a private chain with your own account, you can't use 'embark blockchain private' command. Following the blow steps:
```
1. delete '/datadir' folder.
2. put your account json file into 'datadir/keystore/*'
3. modify account&coinbase 'config/privatenet/gensise.json' 
4. geth --datadir=".embark/privatenet/datadir" init "config/private/genesis.json"
5. embark blockchain privatenet
```

### Geth

All peers must use the exact SAME genesis block specified by genesis.json

```
Node1:
1. geth --datadir=".embark/privatenet/datadir" init "config/private/genesis.json"
2. geth --networkid 100 --identity "youran-mac"  --rpc  --rpccorsdomain "*" --datadir ".embark/privatenet/datadir" --port "30301"  --rpcapi "db,eth,net,web3" --networkid 100 console --unlock=aecb2ceca6605d76a7dacf71dc22870768914021 --password config/private/password -ws --wsport 8546 --wsaddr localhost --mine 

geth --networkid 100 --datadir=".embark/privatenet/datadir" --password config/private/password  --port 30301 --rpc --rpcport 8545 --rpcaddr localhost --rpccorsdomain="localhost:8000,10.89.155.200:8000"  --ws --wsport 8546 --wsaddr localhost    --maxpeers 25 --mine   --shh  --rpcapi "eth,web3,net,shh" --wsapi "eth,web3,net,shh" --unlock=aecb2ceca6605d76a7dacf71dc22870768914021 js .embark/privatenet/js/mine.js


Node2:
1. same as above.
2. admin.appPeer("enode://.....")

Node3:
	.....
```

```
geth console --networkid 100 --datadir=".embark/privatenet/datadir" --password config/private/password  --port 30301 --rpc --rpcport 8545 --rpcaddr localhost --rpccorsdomain="localhost:8000,10.89.155.200:8000"  --ws --wsport 8546 --wsaddr localhost    --maxpeers 25 --mine   --shh  --rpcapi "eth,web3,net,shh" --wsapi "eth,web3,net,shh" --unlock=aecb2ceca6605d76a7dacf71dc22870768914021 js .embark/privatenet/js/mine.js
```

## Confusing Parameters (Geth & Embark)

### Geth Params
All blow commands are consistent in 'Geth' and 'Embark', except [NOTE].

```
--identity value       node name  
--rpc                  Enable the HTTP-RPC server
--rpcaddr value        HTTP-RPC server listening domain (default: "localhost").[NOTE] embark/blockchain.json, use 'rpcHost'.  
--rpcport value        HTTP-RPC server listening port (default: 8545)
--rpcapi value         API's offered over the HTTP-RPC interface  
--rpccorsdomain value  which domains can access to rpc service. * is acceptable. 
--mine                 Enable mining.[NOTE] embark/blockchain.json, use 'mineWhenNeeded' or 'mine'.
--targetgaslimit value    Target gas limit sets the artificial target gas floor for the blocks to mine (default: 4712388)

--gasprice "18000000000"  Minimal gas price to accept for mining a transactions
--gasLimit	 		设置对GAS的消耗总量限制，用来限制区块能包含的交易信息总和，私有链所以填最大
--fast                快速同步模式，同步到最新后转化为正常同步模式。注意：使用该选项必须从区块同步最初开始，下次启动时就可以不用输入次选项，区块高度也会达到快速同步高度。   
--light               轻节点模式，只会同步区块头信息，可以完成基本的命令操作。
--lightserv  value    设置轻节点模式的请求时间最大占比。由于轻节点不会同步区块内部信息，当查询区块信息时（交易信息，特定区块高度信息等）会向全节点其他节点请求数据，设置最大请求时间占比。范围为：0-90，默认为0。
--lightpeers value    设置轻节点模式下，允许连接的最大节点数，默认为20。  
--lightkdf            降低轻节CPU和RAM占有率。
	
```
---

### Embark Configuration files
contract.json:
		deployment.host: ip where contracts should to be deployed.
		dappConnection: rpc service address

webserver.json:
		host&ip: address where setup web service

blockchain.json:
		rpcHost: same as --rpcaddr in Geth
		mineWhenNeeded: same as --mine in Geth

## Common web3 API

```
web3.fromWei(web3.eth.getBalance(web3.eth.accounts[0]),'ether').toString(10)
web3.fromWei(web3.eth.getBalance(web3.eth.coinbase)).toString(10)

web3.fromWei(web3.eth.getBalance(web3.eth.accounts[0]),'ether').toString(10)

web3.eth.getBlock("latest").number
web3.eth.getBalance(web3.eth.coinbase).toString(10)
web3.eth.getBlock("latest").gasLimit

web3.net.peerCount
web3.net.listening

web3.eth.getBlock(0)
web3.eth.blockNumber


SimpleStorage.set(1000).send({from: web3.eth.defaultAccount});
```

## Using JSON-RPC

Although Web3.js help us box JSON-RPC, we can also call it by ourselves.

1. net_peerCount
```
curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":74}' http://localhost:8545
```
2. ...

## RPC and IPC

In Geth, We can use both RPC and IPC to communicate with node. However, Embark only provide PRC service, but not IPC service.(Since I didn't find any IPC related source code.)

So, DO NOT use 'embark blockchain' to start geth if you want to use IPC service. You can run 'geth' manually, and then you can use either 'Javascript console(JSRE REPL)' or 'JSRE script mode' to deal with IPC.

```
geth --exec 'loadScript("/tmp/checkbalances.js")' attach
```

## Reference

1. http://web3js.readthedocs.io/en/1.0/web3-eth.html?highlight=balance#getbalance
2. https://ethereum.gitbooks.io/frontier-guide/content/cli.html
3. https://github.com/ethereum/go-ethereum/wiki/Setting-up-private-network-or-local-cluster
4. http://blog.lixf.cn/essay/2016/09/02/ethereum-1/

---

Author: ZHANG Youran



