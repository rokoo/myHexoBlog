---
title: Hyperledger概念理解（一）
catalog: true
date: 2018-05-23 21:25:53
subtitle:
header-img: "/img/header_img/header-bg-dragon.jpg"
tags:
- Hyperledger
- concept
---
> 对Hyperledger中的一些概念记录下个人的理解，这是系列文章一

# Transactions（事务）
[Hyperledger 官方对事务描述如下](http://hyperledger-fabric.readthedocs.io/en/release-1.1/arch-deep-dive.html#transactions)
> Transactions may be of two types:
> - Deploy transactions create new chaincode and take a program as parameter. When a deploy transaction executes successfully, the chaincode has been installed “on” the blockchain.
> - Invoke transactions perform an operation in the context of previously deployed chaincode. An invoke transaction refers to a chaincode and to one of its provided functions. When successful, the chaincode executes the specified function - which may involve modifying the corresponding state, and returning an output.

Hyperledger的transactions（事务）有两种
* 部署事务
  以一个chaincode作为输入参数，部署事务成功，chaincode就被部署到区块链中
* 调用事务
  通过调用部署在区块链中的chaincode中某一个函数，当调用事务执行成功，chaincode函数将完成执中并返回执行结果
# Node(节点)
[Hyperledger官网对Nodes描述如下](http://hyperledger-fabric.readthedocs.io/en/release-1.1/arch-deep-dive.html#nodes)
> Nodes are the communication entities of the blockchain. A “node” is only a logical function in the sense that multiple nodes of different types can run on the same physical server. What counts is how nodes are grouped in “trust domains” and associated to logical entities that control them.
There are three types of nodes:
> - Client or submitting-client: a client that submits an actual transaction-invocation to the endorsers, and broadcasts transaction-proposals to the ordering service.
> - Peer: a node that commits transactions and maintains the state and a copy of the ledger (see Sec, 1.2). Besides, peers can have a special endorser role.
> - Ordering-service-node or orderer: a node running the communication service that implements a delivery guarantee, such as atomic or total order broadcast.

 节点是区块链中的连结实体，对应区块链网络中的一个物理服务器，节点有三种角色，分别是Client， Peer，Orderer，并且一个Node可以有同时拥有多个角色。
## Client（客户端）
客户端指发起事务请求、广播proposal到排序节点的个体节点。
## Peer（账本节点）
账本节点的主要职责是提交事务并且维护一份账本的副本。
## Orderer（排序节点）
排序节点承担交付保证的责任，接收收到足够背书的事务并且广播到peer节点，让peer节点提交事务并且更新账本。