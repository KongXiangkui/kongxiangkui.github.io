---
title: IPFS学习
date: 2020-05-20 09:58:16
tags:
categories:
toc: true
---
IPFS由Juan Benet（胡安·贝内特）在2014年5月份发起。2015年，他创建 的项目“IPFS”在Y Combinator创业孵化竞赛中获奖并得到了天使投资，同时成立了协议实验室。
IPFS本质上是一种内容可寻址、版本化、点对点超媒体的分布式存储、传输协议，目标是补充甚至取代过去20年里使用的超文本媒体传输协议（HTTP），希望构建更快、更安全、更自由的互联网时代。
<!-- more -->
参考：
- [IPFS的关系族谱、技术架构及工作原理](https://www.8btc.com/media/578007)
- [filecoin文档](https://docs.filecoin.io/)
## IPFS基础

协议实验室团队在开发IPFS时，采用高度模块集成化的方式：

- IPFS底层模块：
  - IPLD：一个转换中间件，将现有的异构数据结构统一成一种格式，方便不同系统之间的数据交换和互操作。
  - LibP2P：IPFS核心中的核心，面对各式各样的传输层协议以及复杂的网络设备，它可以帮助开发者迅速建立一个可用P2P网络层，快速且节约成本。libp2p的主要功能包括：发现节点、连接节点、发现数据、传输数据。它类似现实世界的快递公司，连接着千千万万个节点，除了负责分发数据，还负责查找数据。
  - Multiformats：一系列hash加密算法和自描述方式的集合，它具有SHA1\SHA256 \SHA512\Blake3B等6种主流的加密方式，用以加密和描述nodeID以及指纹数据的生成，它在现有协议基础上对值进行自我描述改造，即从值上就可以知道是如何产生的。
  
- IPFS的激励层：
  - Filecoin：由于IPFS是一个开源的协议，所有人都可以免费利用IPFS进行各种开发，目前IPFS网络中的节点数量还不够多，网络还不够稳定。为了让IPFS能够快速普及推广，协议实验室基于IPFS网络创建了**Filecoin 区块链项目**， 用以激励参与IPFS节点并存储数据的矿工。Filecoin把这些应用的数据价值化，通过类似比特币的激励政策和经济模型，让更多的人去创建节点，去让更多的人使用IPFS。Filecoin是IPFS的经济激励系统，承载着IPFS的价值传递，维系着IPFS生态的发展。

> 现在IPLD支持BTC、ETH、EOS等主流公链的区块数据。IPLD中间件可以把不同的区块结构统一成一个标准进行传递，为开发者提供了成功性比较高的标准，不用担心性能、稳定和bug。

IPFS应用了这几个模块的功能，集成为一种容器化的应用程序，运行在独立节点上，并以Web服务的形式，供大家使用访问。

## IPFS架构

IPFS有八层子协议栈，从低往高分别为身份、网络、路由、交换、对象、文件、命名、应用，每个协议栈各司其职，又互相搭配。

### 身份层和路由层

身份和路由层可以一起解释，对等节点身份信息的生成以及路由规则是通过Kademlia协议生成制定，KAD协议实质是构建了一个分布式松散Hash表（distributed hash table），简称DHT，每个加入这个DHT网络的人都要生成自己的身份信息，然后才能通过这个身份信息去负责存储这个网络里的资源信息和其他成员的联系信息。

### 网络层

网络层是IPFS技术的核心层，使用的libp2p可以支持任意传输层协议。ICE NAT traversal框架整合STUN、TURN和其他类型的NAT协议，该框架可以让客户端利用各种NAT方式打通网络，从而完成NAT通信，这对于IPFS的p2p网络非常重要。

### 交换层

交换层是类似迅雷、电驴这样的BT工具，IPFS团队把BitTorrent进行了创新，叫作Bitswap，它增加了信用和帐单体系来激励节点去分享，用户在发送给其他节点数据可以增加信用值，从其他节点接受数据降低信用值。如果用户只去接收数据而不分享数据，信用分会越来越低而被其他节点忽略掉。

### 对象层和文件层

对象层和文件层也可以结合来谈，他们共同管理IPFS上80%的数据结构。大部分数据对象都是以Merkle DAG的结构存在，这为内容寻址和数据去重提供了便利；文件层是一个新的数据结构，和DAG并列，采用Git一样的数据结构来支持版本快照。

### 命名层

具有自我验证的特性（当其他用户获取该对象时，使用指纹公钥进行验签，即验证所用的公钥是否与NodeId匹配，这验证了用户发布对象的真实性，同时也获取到了可变状态），并且加入了IPNS这个巧妙的设计来使得加密后的DAG对象名可定义，增强可阅读性。

### 应用层

IPFS核心价值就在于上面运行的应用程序，可以利用它类似CDN的功能，在成本很低的带宽下，去获得想要的数据，从而提升整个应用程序的效率。

## IPFS工作原理

IPFS是基于文件内容进行寻址的。IPFS为每一个文件分配一个独一无二的哈希值（文件指纹：根据文件的内容进行创建），即使是两个文件内容只有1个比特的不同，其哈希值也是不相同的。所以IPFS是基于文件内容进行寻址，而不像传统的HTTP协议基于域名寻址。

IPFS为文件建立文件版本管理。IPFS在整个网络范围内去掉重复的文件，并且为文件建立版本管理，也就是说，每一个文件的变更历史都将被记录，可以很容易回到文件的历史版本查看数据。

根据哈希值进行文件查询。当查询文件的时候，IPFS网络根据文件的哈希值（全网唯一）进行查找。由于每个文件的哈希值全网唯一，所以查询将很容易进行。每个节点除了存储自己需要的数据，还存储了一张哈希表，用来记录文件存储所在的位置，用来进行文件的查询、下载。

IPNS。IPNS允许用户使用一个私钥来对IPFS哈希附加一个引用，使用一个公钥哈希表示你的网站是最新版本。如果你使用过比特币，可能会对此比较熟悉，一个比特币地址也是一个公钥，如果该链接不起作用，不用担心，能够通过更改公钥所指向的内容，而公钥却永远保持不变。这样，网站的更新问题就得到了解决。接下来，只需要保证这些网站的位置是人类可读的，所有问题就解决了。

人类可读的可变地址。IPFS/IPNS哈希是一些很大的、难看的字符串，而且不容易记住。所以IPFS允许用户使用现有的域名系统（Domain Name System，DNS）来为IPFS/IPNS内容提供人类可读的链接。它允许用户通过在域名服务器上将哈希插入TXT记录来实现这一点。

IPFS HTTP网关，新旧网络之间的桥梁。通过一个HTTP网关，IPFS可以实现从HTTP到IPFS的过渡，在浏览器完全支持IPFS之前，现在已经允许当前的Web浏览器访问IPFS。用户很快就可以切换到IPFS，完成Web网络的存储、分发和服务。
IPFS协议是开源的，支持任何团队和个人免费存储、下载数据，如何才能让IPFS快速普及和发展，让更多的节点参与者愿意拿出自己的电脑硬盘存储其他人的数据呢？这就需要IPFS的激励层——Filecoin了。
