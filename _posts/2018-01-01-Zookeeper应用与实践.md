---
layout: post
title: Zookeeper应用与实践
subtitle: 详细介绍Zookeeper的应用与实践
date: 2018-01-01
author: chengweii
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - Zookeeper
    - 分布式
---

# Zookeeper

## 工作原理
Zookeeper的核心是原子广播，这个机制保证了各个Server之间的同步。实现这个机制的协议叫做Zab协议。Zab协议有两种模式，它们分别是恢复模式（选主）和广播模式（同步）。当服务启动或者在领导者崩溃后，Zab就进入了恢复模式，当领导者被选举出来，且大多数Server完成了和leader的状态同步以后，恢复模式就结束了。状态同步保证了leader和Server具有相同的系统状态。  
为了保证事务的顺序一致性，zookeeper采用了递增的事务id号（zxid）来标识事务。所有的提议（proposal）都在被提出的时候加上了zxid。实现中zxid是一个64位的数字，它高32位是epoch用来标识leader关系是否改变，每次一个leader被选出来，它都会有一个新的epoch，标识当前属于那个leader的统治时期。低32位用于递增计数。
每个Server在工作过程中有三种状态：  
LOOKING：当前Server不知道leader是谁，正在搜寻  
LEADING：当前Server即为选举出来的leader  
FOLLOWING：leader已经选举出来，当前Server与之同步  

## 基础概念

### 数据模型
* 树形结构
* 版本控制
* 读写控制

#### 数据节点
* 临时节点：会话
* 持久节点：永久存储
* SEQUENTIAL Node：附带自增id

### 集群

#### 集群角色

##### Leader
为客户端提供读和写服务，事务请求的唯一调度和处理者，调度集群内其他服务器

##### Follower
提供读服务，所有写服务都需要转交给Leader角色，处理非事务请求，转发事务请求给Leader，参与选举，参与事务请求提交投票(超半数以上确认)

##### Observer
提供读服务，一般为了增强zk集群的读请求并发能力，处理非事务请求，转发事务请求给Leader，不参与选举、事务请求提交投票

#### 会话
TCP长连接，通过心跳检测会话是否存活。

#### CAP理论运用

#### 服务器状态
* LOOKING：当服务器处于此状态时，表示当前没有Leader，需要进入选举流程
* FOLLOWING：跟随者状态，表明当前服务器角色是Follower
* OBSERVING：观察者状态，表明当前服务器角色Observer
* LEADING：：领导者状态，表明当前服务器角色Leader

#### 集群通信

##### 基于TCP协议
为了避免重复创建两个节点之间的tcp连接，zk按照myid数值方向来建立连接，即小数的节点发起大的节点连接，比如id为1的向id为2的发起tcp连接

##### 端口
* 通信和数据同步端口(默认2888)
* 投票端口(默认3888)

### 分布式系统协调

#### 集群成员管理
* 当前集群中的机器数量
* 集群中机器的运行状态
* 集群中节点的上下线操作
* 集群节点的统一配置

#### 锁
支持节点同步创建的机制，具体分布式锁的使用（乐观锁、悲观锁），是由客户端通过是否创建节点成功的机制辅以一定代码实现的。

#### 选主（Master选举）
保证集群间数据节点数据一致，必须要将写动作放到主节点上，然后由主节点同步到其他节点。
选举算法：FastLeaderElection，
* 临时节点、持久节点
* 顺序节点

#### 同步

##### ZAB协议
* 发现(选举Leader)
* 同步(同步Leader数据到Follower、Observer)
* 广播(通知客户端完成同步)
> 为了保证数据的最终一致性，仅主节点支持写入，然后由主节点同步到其他节点，另外一半以上的节点写入成功后才会返回写入成功。

#### 发布/订阅
主要应用于分布式配置中心场景

##### 负载均衡
通过基于注册的发布订阅，客户端实现具体的负载均衡算法，来实现服务查找的负载均衡。

##### JNDI
实现资源查找的命名服务，仅支持查找，具体资源由客户端自己创建。

##### 分布式队列
支持节点的顺序写入，但使用场景较少，一般都用MQ实现分布式队列。

## 功能使用

### 安装部署

### 操作命令
* ls
* get
* create
* set
* stat
* sync
* delete
* quit
#### 检查dubbo服务情况

```
##1.登录到zookeeper任意节点，找到zookeeper安装路径
whereis zookeeper  
##2.转到控制脚本目录
cd /usr/local/zookeeper/zk/bin/  
##3.登录zookeeper客户端
sh zkCli.sh -server 127.0.0.1:2181  
##4.查看服务列表
ls /dubbo

```

### Watcher(同步实现)

### ACL(访问权限控制)


