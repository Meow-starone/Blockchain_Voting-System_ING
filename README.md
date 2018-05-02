---
title: 基于Ethereum的简易投票Dapp
date: 2018-05-02 00:00:45
tags: 
- 区块链
---

- 合约开发流程：<br>
  合约代码编写（Solidity） --> 合约编译（solc）--> 合约部署（web3）
- 开发语言及工具：<br>
区块链节点：ganache-cli<br>
基础环境：node<br>
合约开发语言：Solidity<br>
合约编译器：solc<br>
合约访问库：web3.js
- 已完成的工作：利用ganache节点仿真器创建账户，进而链上记录投票行为
- 准备完成的工作：引入Truffle框架构建代码，并加入Token激励机制


### Github地址

https://github.com/Meow-starone/Blockchain_Voting-System_ING

### 演示Demo

http://8000.0bcc71cc8eb1ffc1fcd374acb10ccf06.x.hubwiz.com/


### 投票合约
#### 设计
构造函数，用来初始化候选人名单。<br>
投票方法Vote()，每次执行就将指定的候选人得票数加 1<br>
得票查询方法totalVotesFor()，执行后将返回指定候选人的得票数
#### 编译
在node控制台里用web3js库编译和部署合约， 并与区块链进行交互
``` bash
~$ cd ~/repo/chapter1
~/repo/chapter1$ node
> Web3 = require('web3')
> web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
> web3.eth.accounts

> code = fs.readFileSync('Voting.sol').toString()
> solc = require('solc')
> compiledCode = solc.compile(code)
```


### 合约部署
``` bash
 > abiDefinition = JSON.parse(compiledCode.contracts[':Voting'].interface)
> VotingContract = web3.eth.contract(abiDefinition)
> byteCode = compiledCode.contracts[':Voting'].bytecode
> deployedContract = VotingContract.new(['Rama','Nick','Jose'],{data: byteCode, from: web3.eth.accounts[0], gas: 4700000})
> deployedContract.address
'0x9a0036b01f999f8c046ea5fa7b5dddabe24ed8de' <- 这是我生成的部署地址
> contractInstance = VotingContract.at(deployedContract.address)
```


### 网页交互
#### 节点的RPC API地址
``` bash
web3 = new Web3(new Web3.providers.HttpProvider("http://8545.0bcc71cc8eb1ffc1fcd374acb10ccf06.x.hubwiz.com/"));
```
HttpProvier()对象的构造函数参数是web3js库需要链接的以太坊节点RPCAPI的URL，要调整为ganache的访问端结点<br>

#### 指定合约地址
``` bash
contractInstance = VotingContract.at('0x9a0036b01f999f8c046ea5fa7b5dddabe24ed8de');
````

运行Web服务器即可看到效果
