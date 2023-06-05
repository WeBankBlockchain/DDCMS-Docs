# DDCMS-Contract

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)


DDCMS-Contract用于追踪DDCMS的使用过程，数据目录生命周期中的每一个关键环节均会在链上留痕、存证，保证系统的可追溯、可监管。 它由三个模块构成：AccountContract, ProductContract, DataSchemaContract，由系统运营方部署。

- AccountContract: 负责账户管理，包括机构的注册、审核等功能。系统中的数据提供方、见证机构，均需要在AccountContract中注册并审核，才能使用DDCMS。系统运营方则会在部署合约时自动注册。
- ProductContract: 负责业务管理，包括业务的创建、审核等功能。其中，业务的创建由数据提供方进行，其审核由见证方进行，当票数超过半数，即通过审核。
- DataSchemaContract：负责数据目录管理，包括数据目录的创建、审核等。其中，数据目录的创建由数据提供方进行，其审核由见证方进行，当票数超过半数，即通过审核。



## 快速开始
从依赖关系来讲，依赖关系如下：DataSchemaContract->ProductContract->AccountContract.因此，部署顺序需要按照相反顺序，按照AccountContract->ProductContract->DataSchemaContract的顺序部署。

### 链准备
首先需要有一条FISCO BCOS链，如果没有，可以部署一个。FISCO BCOS支持Air版、Pro版、Max版，对应不同的能力和复杂性，具体部署方式可以参考[链搭建文档](https://fisco-bcos-doc.readthedocs.io/zh_CN/latest/docs/tutorial/air/index.html#)。

接下来需要和FISCO BCOS交互，可以使用控制台或者webase。

### 私钥准备
只有系统运营方可以部署合约，所以需要一个系统运营方私钥，可以使用webase或者控制台生成，并以该私钥作为身份来操作webase或控制台。

### 部署合约

然后，依次部署三个合约。

以控制台为例，先将contracts目录下的合约拷贝到控制台contracts/solidity目录下，以系统运营方身份启动控制台后执行部署：
```
[group0]: /apps> deploy AccountContract
transaction hash: 0xade6832deb8f9641009336b955a3637bffbbc4fc9ffadb8a9de03ab6200f6093
contract address: 0x59fc2d7bbbb013ac9fc74a4c79bd25fdce02cba2
currentAccount: 0xcbe29631c0933c4319694dafc50723093f0de937

[group0]: /apps> deploy ProductContract 0x59fc2d7bbbb013ac9fc74a4c79bd25fdce02cba2
transaction hash: 0xefcc3503414490eb28dff330b9d42701bd1359f1fc784014f109ba16212fbfac
contract address: 0x24829babadbdc5fe4205353f6a2a6a79e7eef6a5
currentAccount: 0xcbe29631c0933c4319694dafc50723093f0de937

[group0]: /apps> deploy DataSchemaContract 0x59fc2d7bbbb013ac9fc74a4c79bd25fdce02cba2 0x24829babadbdc5fe4205353f6a2a6a79e7eef6a5
transaction hash: 0x27c43f0ca480a5f349205b2dcad6cc045ce4636ee4e41fbddfebb68b60104625
contract address: 0xa085baa5914e1d082c94ce24baf49473caa33ff1
currentAccount: 0xcbe29631c0933c4319694dafc50723093f0de937
```

其中：
- ProductContract部署参数为AccountContract地址，请替换为实际的地址
- DataSchemaContract部署参数为AccountContract地址和ProductContract地址，请替换为实际的地址

