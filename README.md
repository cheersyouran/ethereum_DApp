## About

It is a DApp based on embark framework, which relies on nodejs, integrates with EVM blockchains (Ethereum), Decentralized Storages (IPFS), and Decentralized communication platforms (Whisper and Orbit).

## How to run etherum_DApp in a private net?

### Embark
About private net:All peers must use the exact same genesis block specified by genesis.json
```
1. git clone https://github.com/cheersyouran/ethereum_DApp.git
2. cd etherum_DApp
3. add bootnodes to 'config/blockchain.json/privatenet.bootnodes'
	bootnode --genkey=boot.key
	bootnode --nodekey=boot.key
	can also create 'datadir/static-nodes.json' set ["enode://dfd047d@[::]:30303"])
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

```
1. geth --datadir=".embark/privatenet/datadir" init "config/private/genesis.json"
2. geth --identity "youran-mac"  --rpc  --rpccorsdomain "*" --datadir ".embark/privatenet/datadir" --port "30301"  --rpcapi "db,eth,net,web3" --networkid 100 console
3.
```

[Reference]
1. https://github.com/ethereum/go-ethereum/wiki/Setting-up-private-network-or-local-cluster
2. http://blog.lixf.cn/essay/2016/09/02/ethereum-1/

## Parameters & Configuriaton(Geth&Embark)

### Geth Params
All blow commands are consistent in 'Geth' and 'Embark', except [NOTE].

```
--identity value       node name  
--rpc                  Enable the HTTP-RPC server
--rpcaddr value        HTTP-RPC server listening domain (default: "localhost").[NOTE] in embark/blockchain.json files, use 'rpcHost' in steadof '--rpcaddr'  
--rpcport value        HTTP-RPC server listening port (default: 8545)
--rpcapi value         API's offered over the HTTP-RPC interface  
--rpccorsdomain value  指定可以访问rpc的domain地址，设置为“*”则任何地址都可以访问。
--rpcvhosts value      Accept requests (server enforced). Accepts '*' wildcard.  (default: "localhost")

--bootnodes value     Comma separated enode URLs for P2P discovery bootstrap (set v4+v5 instead for light servers)

--mine                    Enable mining.[NOTE] in embark/blockchain.json files, use 'mineWhenNeeded'
--etherbase value         Public address for block mining rewards (default = first account created) (default: "0")
--targetgaslimit value    Target gas limit sets the artificial target gas floor for the blocks to mine (default: 4712388)
--gasprice "18000000000"  Minimal gas price to accept for mining a transactions

--fast                启动快速区块同步模式，在同步到最新区块后，转化为正常区块同步模式。注意：使用该选项必须从区块同步最初开始，当同步到最新区块后，可以正常同步区块，下次启动时就可以不用输入次选项，区块高度也会达到快速同步高度。   
--light               轻节点模式，只会同步区块头信息，可以完成基本的命令操作。
--lightserv  value    设置轻节点模式的请求时间最大占比。由于轻节点不会同步区块内部信息，当查询区块信息时（交易信息，特定区块高度信息等）会向全节点其他 节点请求数据，设置最大请求时间占比。范围为：0-90，默认为0。
--lightpeers value    设置轻节点模式下，允许连接的最大节点数，默认为20。  
--lightkdf            降低轻节CPU和RAM占有率。

--gasLimit	 设置对GAS的消耗总量限制，用来限制区块能包含的交易信息总和，私有链所以填最大

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

## Geth API

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

[Reference] http://web3js.readthedocs.io/en/1.0/web3-eth.html?highlight=balance#getbalance
