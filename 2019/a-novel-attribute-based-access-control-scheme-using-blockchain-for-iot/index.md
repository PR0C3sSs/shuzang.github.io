# A Novel Attribute-Based Access Control Scheme Using Blockchain for IoT


Ding, Sheng, et al. "A Novel Attribute-Based Access Control Scheme Using Blockchain for IoT." *IEEE Access* 7 (2019): 38431-38441.

DOI: [10.1109/ACCESS.2019.2905846](https://doi-org-s.webvpn.neu.edu.cn/10.1109/ACCESS.2019.2905846)

KeyWord: Access control, attribute-based access control, blockchain, consortium blockchain, IoT

注：插图经过重新绘制，照片来自原论文截图。

## 1. 摘要

随着智能设备数量的急剧增加，物联网近年来得到越来越多的关注和快速发展。它通过现有的网络基础设施有效地将物理世界与Internet集成在一起，以便在智能设备之间共享数据。然而，其复杂的大规模网络结构给物联网系统带来了新的安全风险和挑战。为了保证数据的安全性，传统的访问控制技术由于其复杂的访问管理和集中性导致可靠性不足，不适合直接用于物联网系统的访问控制。本文提出了一种新的基于属性的用于物联网系统的访问控制方案，大大简化了访问管理。我们使用区块链技术来记录属性的分发，以避免单点故障和数据篡改。访问控制过程也进行了优化，以满足对物联网设备进行高效和轻量化计算的需要。安全性和性能分析表明，该方案能够有效抵御多种攻击，并能在物联网系统中得到有效的实现。

<!--more-->

## 2. 引言

物联网无疑是最有前途的技术之一，近年来引起了学术界和产业界的广泛关注。物联网是一种新型的架构框架，通过现有的网络基础设施将物理世界与互联网结合起来。它的目标是连接所有的智能设备，包括物理设备、车辆和家用电器，并使它们能够通过互联网自动收集和共享数据。根据Gartner的预测，2017年，全球有超过84亿的联网企业加入了该网络，比2016年增长了31%，到2020年将达到204亿。然而，连接设备数量的急剧增加给物联网系统带来了新的安全风险和挑战。

由于物联网设备分布广泛，因此很难实施严格的安全控制，使其面对恶意实体的各种攻击比较脆弱。有必要保护物联网设备免受未经授权的访问，因为这些设备通常包含许多有价值和敏感的数据，未经授权的访问通常会导致严重的数据泄露。众所周知，访问控制是保证数据安全的重要技术之一。传统的访问控制技术如任意访问控制（DAC）、基于身份的访问控制（IBAC）不适合在物联网系统中实施，因为由于大量未知身份，几乎不可能为物联网系统中的每个人制作访问控制列表（ACL）。另一种常见的技术强制访问控制（MAC）通常由中央管理员执行，这就存在单点故障的问题。由于物联网设备的位置或功能不同，可能属于不同的管理机构，集中访问控制模式不适合物联网系统。

基于属性的访问控制（ABAC）提供了一种灵活、动态和细粒度的访问控制。它将角色或身份抽象为属性权威发布的一组属性。建立在一组属性之上的由布尔公式描述的访问策略用于定义有效和授权的访问。不再需要为系统中的每个角色分配角色或制作访问控制列表。相反，属性权限只需要管理定义在系统中的每个属性，并将它们分发给适当的用户。这样，访问管理就可以有效地简化，因为属性的数量远远小于系统中的用户数量。

区块链是技术巨头和商业界感兴趣的另一个热门话题。它是一种开放、透明和分布式的分类账，能够以可验证和永久的方式有效地记录双方之间的交易。一旦记录，区块链上的数据就不能被篡改，除非达成新的共识。将物联网与区块链技术相结合是一个有前途的趋势，预计将确保物联网系统的信任并减少总体开销。它可以帮助物联网建立一个分散的、可信的、可公开验证的数据库，从而使数以十亿计的连接物通过它实现分布式信任。

本文提出了一种新的基于属性的物联网系统访问控制方案。我们的主要贡献总结如下：

1）针对物联网系统提出了一种新的基于属性的访问控制方案。不再需要为系统中的每个人创建ACL或分配角色。每个设备都可以由一组属性来描述，这些属性在系统中预先定义，并由属性权威根据其身份或能力颁发。除非有足够的属性匹配访问策略，否则不允许任何人访问。

2）我们使用区块链记录属性的分布。属性权威共同维护一个公开和可信的“交易”分类账。一旦被记录，区块中的数据就不能被更改，任何人都可以在需要时随时查询区块链。

3）简化了访问控制协议，双方只需做一些简单的签名和散列操作即可。这样，我们的方案对物联网系统中计算能力和能源供应有限的设备更有效。

论文组织如下：第二节对相关工作进行了总结，第三节对前期工作进行了总结。我们在第4节中提出了利用区块链对物联网系统进行基于属性的访问控制方案的详细构造。第5节和第6节分别是安全性和性能分析。最后，我们将在第7节中得出结论。

## 3. 相关工作

为了确保严格的访问控制，身份验证是在共享或交换数据之前确认通信参与者身份的必要机制。沙尔曼等人提出了用于物联网的基于身份的认证方案。在他们的方案中，每个设备都由它所属的网关管理，并且拥有一个由控制器授予的虚拟IPv6地址作为其唯一标识。在与其他人进行身份验证时，授权地址可以用作证书。Porambage等人为分布式物联网应用中的无线传感器网络设计了一种轻量级的认证机制。每个传感器节点将首先从相应的簇头中获得一个凭证，作为将来身份验证的先决条件。针对节点地理分布的差异及其相互关系，讨论了四种通信链路。希夫拉杰等利用一次性密码（OTP）技术，设计了一种基于身份的椭圆曲线密码系统（IBE-ECC）的端到端的轻量级认证方案。由于不需要存储密钥，密钥大小也很小，因此该方案对物联网系统是有效的。参考文献[5]提出了一种分布式基于能力的物联网系统访问控制方案，其中智能事物可以实现轻量级的端到端授权。能力通常是一种可沟通的、不可原谅的权威象征。

许多企业倾向于充当集中的受信任权限，并基于OAuth协议强制授权[6]。该项目连接所有基于IP的智能对象（calipso）实现了一个集中的系统模型，其中智能对象将授权委托给一个名为iot-oas的强大服务器。然而，[7]表明，在资源受限的设备中运行所有OAuth逻辑几乎是不可能的，因为它有很高的通信和计算开销。

基于角色的访问控制（RBAC）[8]是限制授权用户访问权限的常用方法。它根据用户在系统中的角色授予用户特定的权限。然而，它不适合物联网系统，因为这种接入模式不够灵活和可扩展。一旦一个设备被分配给一个角色，它就只能以固定的方式访问数据。与RBAC相比，基于属性的访问控制（ABAC）[9]更加灵活和可扩展，能够提供更细粒度的访问控制。实体可以在属性上定义访问策略以限制有效访问。针对物联网感知层，提出了一种基于ABAC模型的高效访问控制方案。只有在策略中具有满足属性子集的用户才能获得访问授权。张等[11]，[12]提出了两种基于属性的加密方案，以确保细粒度的访问控制和数据安全。

上面提到的大多数访问控制方案都面临着一个共同的问题，即需要一个可靠的中心来确保信任。由于物联网设备分布在世界各地，因此不可能有一个集中的节点来管理所有这些设备。一般来说，每一个都是由附近或其所属的当局管理的。在这些部门之间建立信任是实现分布式管理的基础。因此我们认为使用区块链可以建立一个分布式的一致性信任。它于2008年首次由Nakamoto[13]提出，现在这是数字货币比特币的基础技术。

Ouaddah等人[14]对物联网的不同访问控制方案进行了广泛的审查，结论是传统的访问控制机制不适用于资源受限的环境。Hardjono和Pentland[15]详细描述了基于区块链的访问控制和身份管理的设计。他们在[15]中提出的chainechor系统为系统中的实体提供匿名但可验证的身份。共识节点可以引用匿名成员的公钥列表来强制访问控制。同一实体进行的交易是不可链接的。哈尔特等人[16]系统总结了物联网中区块链的实现。

在Ouaddah[17]中，引入了一个使用名为FairAccess的区块链的分散授权管理框架。它使用新类型的交易来授予或撤销用户的访问权。Novo[18]提出了基于区块链的物联网系统的全分布式访问控制方案。与FairAccess不同，管理系统的策略规则是通过生成单个智能合约来定义的，访问控制策略是通过在[18]中为此智能合约创建交易来定义的。我们使用联盟区块链而不是私有区块链来建立一个更加分散的系统。由属性组成的访问策略由物联网设备根据其安全要求自行定义。要获得访问授权，所涉及的设备必须证明其对满足策略的相应属性的所有权。

## 4. 方案

### A. 系统模型

我们首先介绍了我们使用区块链实施物联网的访问控制方案的系统模型，如图3所示，有两个主要实体，属性权威和物联网设备。属性权威同时充当联盟区块链中的联盟节点和密钥生成中心（KGC）。我们最多允许n个属性权威中的（n-1）/3个是拜占庭节点，因此至少有4个联盟节点。为了便于理解，我们只使用最少数量的联盟节点来构建图3中的系统模型。

![](https://ieeexplore.ieee.org/ielx7/6287639/8600701/8668769/graphical_abstract/access-gagraphic-2905846.jpg)

1）属性权威

属性权威是区块链的管理者和属性的分发者。为了共同维护分布式账本，他们需要在属性分配上达成共识。属性的授权以交易的形式记录。由某个属性权威授权的属性交易首先放置在自己的交易池中。其他属性权威机构必须在记录到区块链之前验证其有效性。一旦成功记录，任何人都不能篡改区块数据，除非所有联盟节点达成新的共识，并从需要修改的区块生成新的链。考虑地理分布因素，每个属性权威管理不同区域的设备。

同时，当物联网设备向系统注册时，属性权威也充当密钥生成中心（KGC）。每个属性授权机构将使用基于身份的加密技术，根据每个附属设备的身份向其颁发一对公钥和密钥。通过公/私密钥对，通信中涉及的设备可以相互进行身份验证，并就会话密钥达成一致。

2）物联网设备

这些设备负责在物联网系统中收集、处理和共享数据。他们不参与交易验证，只有区块链的读取权限。为了确保有效的访问和数据安全，数据请求者在交换数据之前需要从数据所有者那里获得访问授权。数据请求者使用属性权限分配的属性来证明它们具有相应的权限。只有在有一组符合数据所有者访问策略的属性时，才允许访问。

### B. 安全模型

在我们的方案中，由于各种恶意攻击成为拜占庭节点，属性权威可能非常容易受到攻击。我们限制拜占庭节点的数量不超过n个节点中的n−1/3个。有了这个限制，属性分配的共识就可以正常地达成。每个属性权限的密钥都是安全的，因此没有人能够伪造每个联盟节点的签名。他们知道彼此的公钥，这样他们就可以验证每个签名的有效性。

这些设备是不可信的，因为它们可能相互勾结，在利益驱动下，当它们中没有一个独立地具有满意的属性集时，它们就与其他设备进行身份验证。恶意设备甚至可能有意篡改区块链或干扰属性权限以达成共识。

### C.构造

1）系统初始化

设λ为安全参数。系统初始化算法以安全参数λ为输入，输出系统的全局参数。所有系统成员都需要在系统规定的相同椭圆曲线上达成一致。设e为椭圆曲线加群，其中椭圆曲线离散对数问题（ecdlp）是一个难处理的问题。设g为e的一个元素，具有大的素数阶r，然后选择两个安全散列函数h1：0，1→z r，h2：0，1→0，1λ，将任意大小的位串映射成一个新的固定大小的位串。设g为e的一个元素，具有较大的素数r，所有的属性权限共享一个skm∈z r，并将其作为主私钥，相应的主公钥pkm就是skmg。最后，系统参数发布为λ，e，g，pkm，h1，h2。

2）注册

每个设备在系统中都有一个直观的身份ID。注册时，设备所属的属性权威在验证其身份的基础上，根据基于身份的密码学，在安全通道中向其颁发一对公钥和密钥。

3）地址生成

每个设备使用一个地址及其ID来申请一个属性i。为了生成一个地址，它随机选择一个k∈z r作为密钥（sk），因此kg是公钥（pk）。为了生成一个相应的地址，设备可以散列pkkid（k表示这里的连接），并通过base58检查编码对结果进行编码。因此，地址为：address=base58检查[h2（pkkid）]。

4）属性请求

每个属性权威都有一对公钥和密钥。公钥用于生成自己的地址aa，密钥用于签署交易。设备与之交互的属性权威将验证申请人是否能够拥有该属性I。如果设备通过验证，属性权限将生成一个交易：a a i−→address。然后，属性权威用它的密钥sigsk[h1（a a i−→addressktimestamp）]对该交易的散列和时间戳进行签名。

最后，属性权威打包交易、签名和时间戳，并将其放入自己的交易池中。

这些联盟节点将定期选择一个块生成者。其职责是将其交易池中的交易打包成一个块，并将其广播给其他联盟节点，以达成共识。块生成者根据时间戳对交易进行排序，并计算所选交易的merkle根。块头由最后一个块头的散列、生成该块的时间戳和merkle根组成。如图5所示，数据块制造商使用PBFT[5]协议将这个新数据块广播给其他联盟节点。在预准备阶段，其余联盟节点将验证新块的有效性，并以相同的方式向其他节点广播。一旦接收到2F相同的块，他们将在准备阶段向其他人广播确认消息。如果一个节点接收到2F+1确认消息，它会将新块追加到区块链中。

5）访问控制

Alice和Bob之间的访问控制协议如图4所示执行：

![两个通信实体的访问控制](https://picped-1301226557.cos.ap-beijing.myqcloud.com/59078893-c12f5780-8913-11e9-92f9-6692ec0d89bc.png)

- Alice首先使用她的身份ID<sub>A</sub>向Bob发起一个通信请求，并使用基于身份的验证和密钥协议（AKA）与Bob生成会话密钥k。使用任意对称密钥算法，k可以保证Alice和Bob之间后续通信的安全性。为了简单起见，我们只描述下面消息交换的过程并忽略每个通信的对称加密。
- 然后Bob返回一个随机数n和一个访问策略p，表示谁可以与他通信。
- Alice选择一个满足策略的子集，并使用其相应地址已被发出匹配属性i的每个密钥对随机数n进行签名。