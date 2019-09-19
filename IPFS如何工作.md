---
标题：IPFS如何工作  
权重: 2
---
> ref: https://github.com/ipfs/docs/blob/master/content/introduction/how-ipfs-works.md

IPFS是一个点对点(p2p)存储网络。内容可以通过对等点访问，这些对等点可以传递或存储信息(或者两者都可以)，这些对等点可以位于世界上的任何地方。IPFS知道如何通过它的内容地址找到您想要的内容，而不是通过它的存储位置。

## 了解 IPFS 的三个要点

让我们首先看看 _内容寻址_ 以及如何将该 _内容关联_ 在一起。IPFS 堆栈的“中间”部分将生态系统连接在一起; 一切都建立在能够通过关联的惟一标识符查找内容的基础上。

### 1. 内容寻址和关联数据

IPFS使用 _内容寻址_ 根据内容中的内容来标识内容，而不是根据内容的位置。根据内容查找条目是您一直在做的事情。例如，当你在图书馆找一本书的时候，你通常会问书名; 这就是内容寻址，因为你问的 **是什么** 。如果你要找那本书，你可以这样问:“我想要那本书，在二楼，第一层，第三层，左边四本书。”如果有人动了那本书，你就倒霉了!

这个道理在互联网上和你的电脑上都是一样的。现在，内容是按位置查找的，比如：

- `https://en.wikipedia.org/wiki/Aardvark`
- `/Users/Alice/Documents/term_paper.doc`
- `C:\Users\Joe\My Documents\project_sprint_presentation.ppt`

相比之下，使用IPFS协议的每一段内容都有一个[*内容标识符*]({{<relref "guides/concepts/cid.md">}})，或者说 CID，即内容的哈希值。哈希值对于它所来自的内容是惟一的，即使与原始内容相比它可能看起来很短。 _如果哈希对您来说是新的知识，请查看[关于哈希的概念指南]({{<relref "guides/concepts/hashes.md">}})以获得介绍。_

通过哈希进行内容寻址已经成为分布式系统中连接数据的一种广泛使用的方法，从返回代码的提交到运行加密货币的区块链。然而，这些系统中的底层数据结构不一定是可互操作的。

这就是[IPLD项目](https://ipld.io/)发挥作用的地方。**散列标识内容，IPLD在数据结构之间进行转换**。由于不同的分布式系统以不同的方式构造它们的数据，所以IPLD提供了一些库，用于组合可插入模块(针对每种可能的IPLD节点类型的解析器)，以便跨许多链接节点解析路径、选择器或查询(允许您在不考虑底层协议的情况下研究数据)。IPLD提供了一种在内容可寻址数据结构之间进行转换的方法:“哦，您使用git风格，不用担心，我可以跟踪这些链接。哦，你用以太灵，我找到你了，我也能找到那些链接!”

IPFS协议使用IPLD将原始内容转换为IPFS地址。IPFS对于如何将数据分解为DAG有自己的偏好和约定(下面将详细介绍DAG); IPLD使用这些约定将IPFS网络上的内容链接在一起。

**IPFS生态系统中的其他所有内容都建立在这个核心概念之上:链接的、可寻址的内容是使其余部分工作的基本连接元素**

### 2. IPFS 将文件转化为 DAGs

IPFS和许多其他分布式系统利用了一个名为[定向无环图](https://en.wikipedia.org/wiki/Directed_acyclic_graph)的数据结构，即DAGs。具体来说，它们使用 _Merkle-DAGs_，即其中每个节点都有一个标识符，该标识符是节点内容的哈希的 DAGs_。听起来是不是很熟悉? 这涉及到我们在前一节中讨论过的 _CID_ 概念。理解这个 CID-linked-data 概念的另一种方法是: 根据数据对象的哈希值来标识数据对象(类似于 Merkle-DAG 节点)，即 _内容寻址_。_(查看 [关于Merkle-DAGs的概念指南]({{<relref "guides/concepts/merkle-DAG.md">}}) 可以更深入地讨论这个主题。_

IPFS 使用了一个用于表示目录和文件的 Merkle-DAG，但是您可以用许多不同的方式构造一个 Merkle-DAG。例如，Git使用一个包含许多版本的 Merkle-DAG。

要构建 Merkle-DAGs 来表示内容，IPFS通常首先将其拆分为 _blocks_。将它分成块意味着文件的不同部分可以来自不同的源，并且可以快速地进行身份验证。(如果你曾经使用过BitTorrent，你可能会注意到，当你下载一个文件时，BitTorrent可以同时从多个对等点获取它;道理是一样的。

Merkle-DAGs有点像[“turtles all the way down”](https://ipfs.io/ipfs/QmXoypizjW3WknFiJnKLwHCnL72vedxjQkDDP1mXWo6uco/wiki/Turtles_all_the_way_down.html)场景；**所有** 东西都是CID。您有一个具有CID的文件。如果一个文件夹中有几个文件怎么办?该文件夹有一个CID，该CID包含下面文件的CID。反过来，这些文件由块组成，每个块都有一个CID。您可以看到如何将计算机上的文件系统表示为DAG。希望你们也能看到，Merkle-DAGs是如何形成的。对于这个概念的可视化探索，请查看我们的[IPLD Explorer](https://explore.ipld.io/#/explore/QmSnuWmxptJZdLJpKRarxBMS2Ju2oANVrgbr2xWbie9b2D)。

Merkle-DAGs 的另一个有用特性是将内容分成块，如果您有两个类似的文件，它们可以共享部分的Merkle-DAG; 即，不同的Merkle-DAGs的一部分可能指向相同的数据。例如，如果您更新一个网站，只有更改过的文件才会获得新的内容地址。您的旧版本和新版本可以引用相同的块来表示其他所有内容。这可以使大型数据集(如基因组研究或天气数据)的传输版本更有效，因为您只需要传输新或已更改的部分，而不是每次都创建全新的文件。

### 3. 分布式哈希表

因此，概括一下，IPFS允许您为内容提供 CIDs，并使用IPLD将该内容关联到一个 Merker-DAG 中。现在让我们进入最后一部分:如何查找和移动内容。

要查找 _发现_ 后承载内容的对等点，IPFS使用[_分布式哈希表_](https://en.wikipedia.org/wiki/Distributed_hash_table)(DHT)。哈希表是 key-value 数据库。分布式哈希表是一个将表分割到分布式网络中的所有对等点的哈希表。要找到内容，可以询问对等点。

[libp2p项目](https://libp2p.io/)是IPFS生态系统的一部分，它提供DHT并处理相互连接和通信的对等点。(注意，与IPLD一样，libp2p也可以用作其他分布式系统的工具，而不仅仅是IPFS。)

一旦您知道了您的内容在哪里(即，哪些对等点或对等点正在存储构成您要查找内容的每个数据块)，您就可以 **再次** 使用 DHT 来查找这些对等点的当前位置(_路由（网络位置）_)。因此，为了获得内容，您使用libp2p两次查询DHT。

您已经发现了您的内容，并且找到了该内容的当前位置 — 现在您需要连接到该内容并 _交换_ 得它。要从其他对等点请求块并将块发送给其他对等点，IPFS当前使用一个名为[_Bitswap_](https://github.com/ipfs/specs/tree/master/bitswap)的模块。Bitswap 允许您连接到拥有您想要的内容的对等点，并向它们发送您的 _wantlist_(您感兴趣的所有块的列表)，并让它们向您发送您所请求的块。一旦这些块到达，您可以通过散列它们的内容来获得 CID 来验证它们。(如果需要，这些 CID 还允许删除数据块。)

还讨论了[其他内容复制协议](https://github.com/ipfs/camp/blob/master/DEEP_DIVES/24-replication-protocol.md)，其中开发得最多的是[_Graphsync_](https://github.com/ipld/specs/blob/master/block-layer/graphsync/graphsync.md)。还有一个正在讨论的提议是[扩展Bitswap协议](https://github.com/ipfs/go-bitswap/issues/186)，以围绕请求和响应添加功能。

#### 关于 libp2p 的笔记

使 libp2p 点对点等连接特别有用的是 _connection multiplexing_。传统上，系统中的每个服务都将打开一个不同的连接，以便与其他同类服务进行远程通信。使用IPFS，您只打开一个连接，然后将其上的所有内容进行多重处理。对于你的相邻节点之间需要交流的每件事，你只发送每件事的很小的一部分，而另一端知道如何将这些块数据排序。

这很有用，因为建立连接通常很难建立，维护起来也很昂贵。使用多路复用，一旦有了连接，就可以在上面做任何需要做的事情。

## 一切都是模块

如果你注意到这个讨论，IPFS生态系统由许多支持各种分布式的不同部分的模块库构成。你当然可以独立使用栈中的任意部分，或用新奇的方式组合。

## 总结

IPFS生态系统为内容提供 CID，并通过生成 IPLD-Merkle-DAGs 将这些内容关联在一起。您可以使用libp2p提供的DHT发现内容，并打开到该内容的任何提供者的连接，然后使用多路连接下载它。所有这些都是由堆栈的“中间”连接在一起的唯一标识符;这是IPFS的基础。

<!--Next, we’ll look at how IPFS is an interconnected network of equal peers, each with the same abilities (no client-server relationships), and what that means for system architectures. We’ll also touch on another useful project in the ecosystem -- IPFS Cluster -- that can help make sure your content is always available, even on a network like IPFS that supports peers dropping in and out at will.-->
