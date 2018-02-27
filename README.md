## About

It is a DApp based on embark framework, which relies on nodejs, integrates with EVM blockchains (Ethereum), Decentralized Storages (IPFS), and Decentralized communication platforms (Whisper and Orbit).

## How to run it?

```
cd etherum_DApp
embark simulator
embark run
chrome open localhost:8000
```

---
## How to setup a private net?

### Geth

1. All peers must use the exact same genesis block specified by genesis.json:
2. Init blockchain use the genesis block you have defined.
3. Setup bootnode
```
bootnode --genkey=boot.key
bootnode --nodekey=boot.key
geth --datadir path/to/custom/data/folder --networkid 15 --bootnodes <bootnode-enode-url-from-above>
```
4. Running a Private Minner.
Since your network will be completely cut off from the main and test networks, you'll also need to configure a miner to process transactions and create new blocks for you.
```
geth <usual-flags> --mine --minerthreads=1 --etherbase=0x0000000000000000000000000000000000000000
```

[Reference] https://github.com/ethereum/go-ethereum/wiki/Setting-up-private-network-or-local-cluster

### Embark framework

See 'config/blockchain.json/#privatenet' & 'config/privatenet/gensise.json'

[Embark Configuration]
contract.json: 
	deployment.host: ip where contracts should to be deployed. 
	dappConnection: rpc service address

webserver.json:
	host&ip: address where setup web service


## Parameters & Configuriaton(Geth&Embark)

All blow commands are consistent in 'Geth' and 'Embark', except [NOTE].

```
--identity value       node name  
--rpc                  Enable the HTTP-RPC server 
--rpcaddr value        HTTP-RPC server listening domain (default: "localhost").
[NOTE] in embark/blockchain.json files, use 'rpcHost' in steadof '--rpcaddr'  
--rpcport value        HTTP-RPC server listening port (default: 8545) 
--rpcapi value         API's offered over the HTTP-RPC interface  
--rpccorsdomain value  指定可以访问rpc的domain地址，设置为“*”则任何地址都可以访问。 
--rpcvhosts value      Accept requests (server enforced). Accepts '*' wildcard.  (default: "localhost") 

--bootnodes value     Comma separated enode URLs for P2P discovery bootstrap (set v4+v5 instead for light servers)
(for geth, you can also create 'datadir/static-nodes.json' to set bootnodes statically. ["enode://dfd047d@[::]:30303"])

--mine                    Enable mining 
[NOTE]: in embark/blockchain.json files, use 'mineWhenNeeded'
--etherbase value         Public address for block mining rewards (default = first account created) (default: "0")
--targetgaslimit value    Target gas limit sets the artificial target gas floor for the blocks to mine (default: 4712388)
--gasprice "18000000000"  Minimal gas price to accept for mining a transactions

--fast                启动快速区块同步模式，在同步到最新区块后，转化为正常区块同步模式。注意：使用该选项必须从区块同步最初开始，当同步到最新区块后，可以正常同步区块，下次启动时就可以不用输入次选项，区块高度也会达到快速同步高度。   
--light               轻节点模式，只会同步区块头信息，可以完成基本的命令操作。 
--lightserv  value    设置轻节点模式的请求时间最大占比。由于轻节点不会同步区块内部信息，当查询区块信息时（交易信息，特定区块高度信息等）会向全节点其他 节点请求数据，设置最大请求时间占比。范围为：0-90，默认为0。 
--lightpeers value    设置轻节点模式下，允许连接的最大节点数，默认为20。  
--lightkdf            降低轻节CPU和RAM占有率。 


0: Olympic, Ethereum public pre-release testnet 
1: Frontier, Homestead, Metropolis, the Ethereum public main network  
1: Classic, the (un)forked public Ethereum Classic main network, chain ID 61  
1: Expanse, an alternative Ethereum implementation, chain ID 2  
2: Morden, the public Ethereum testnet, now Ethereum Classic testnet  
3: Ropsten, the public cross-client Ethereum testnet  
4: Rinkeby, the public Geth PoA testnet 
8: Ubiq, the public Gubiq main network with flux difficulty chain ID 8  
42: Kovan, the public Parity PoA testnet  
77: Sokol, the public POA Network testnet 
99: Core, the public POA Network main network 
7762959: Musicoin, the music blockchain 
[Other]: Could indicate that your connected to a local development test network.

```

## Geth API

```
web3.fromWei(web3.eth.getBalance(web3.eth.accounts[0]),'ether').toString(10)
web3.fromWei(web3.eth.getBalance(web3.eth.coinbase)).toString(10)

web3.eth.getBlock("latest").number
web3.eth.getBalance(web3.eth.coinbase).toString(10)
web3.eth.getBlock("latest").gasLimit

web3.net.peerCount
web3.net.listening
```

http://web3js.readthedocs.io/en/1.0/web3-eth.html?highlight=balance#getbalance


