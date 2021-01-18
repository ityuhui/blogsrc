title: 以太坊编程系列 一
date: 2021-01-11 21:50:50

categories:
- 区块链和以太坊

tags:
- block chain
- ethereum
---

## 摘要

使用web3.js, infura, Gerli进行以太坊编程

<!--more-->

## 介绍

web3.js是一个javascript库，用于和以太坊节点通信（通过JSON-RPC）。

web3.js需要连接一个以太坊节点，如果在本地运行一个以太坊节点，该节点会将全部的以太坊区块链下载下来，消耗较多的计算和存储资源。

如果仅仅出于开发和测试的目的，不想部署本地的以太坊节点，可以到infura.io上申请一个远程的节点。infura.io提供以太坊节点服务，无需本地搭建节点，就可以连接到以太坊网络上，包括正式的Mainnet以及Görli, Ropsten等测试网络上。

Görli是一个使用权威证明（proof-of-authority）达成共识的以太坊测试网络。

## 步骤

### 1 到infura.io上注册并创建project

### 2 创建以太坊测试账户
可以使用一些开发工具例如```truffle develop```创建一些测试用的以太坊私钥和地址

### 3 从测试网的faucet获取一些测试币
通常在网页上操作，提供一个在以上步骤创建的账户地址即可

### 4 安装web3.js

### 5 编写代码
```javascript

let infura_project = "XXXXXX";
let account_address = "0xXXXXXX";

const Web3 = require('web3');

if (typeof web3 !== 'undefined') {
    web3 = new Web3(web3.currentProvider);
} else {
    web3 = new Web3(new Web3.providers.HttpProvider(infura_project));
}

async function showBlance() {
    let balanceWei = await web3.eth.getBalance(account_address)
    console.log(balanceWei)
}

showBlance();
```