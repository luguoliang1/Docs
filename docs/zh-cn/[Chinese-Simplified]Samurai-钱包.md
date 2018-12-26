> PlatON开源的桌面版钱包客户端

## 什么是Samurai

**Samurai**是PlatON开源的图形化桌面版钱包客户端，支持基础的普通钱包管理，基于多重签名技术的共享钱包管理，同时为开发者提供Wasm合约的部署与运行，PlatON私用链的可视化搭建，以及基于PlatON经济模型下的候选节点竞选申请与投票，参与PlatON生态治理等功能。未来，Samurai将持续为不同用户提供更加基础、更加丰富的应用功能，让大家更好的了解和使用PlatON生态服务。

Samurai核心功能如下：

- 搭建PlatON私用网络
- 创建钱包/导入钱包（支持使用Keystore，助记词，私钥）
- 支持共享钱包（基于多重签名技术）
- 查看钱包余额及交易明细
- 发送及提取ATP
- 合约部署与测试运行
- 候选节点竞选申请与质押管理
- 参与候选节点投票

请注意，Samurai仍处于测试阶段，可能会出现一些问题。 本指南将帮助您解决一些最常见的问题。如有更对问题欢迎提交到[issues](https://github.com/PlatONnetwork/wiki/issues) ！


## 如何下载安装PlatON Samurai客户端

Samurai适用于Windows/MacOS/Linux,下载请点击[这里](https://github.com/PlatONnetwork/Samurai/releases)

### Windows安装

```
1. Windows安装程序可从[这里](https://github.com/PlatONnetwork/Samurai/releases/download/v0.2.0/Samurai.Setup.0.2.0.5.exe)下载。
2. 双击安装程序并按照说明进行操作
3. 安装程序会在桌面上添加Samurai的快捷方式。
```

### macOS安装

```
1. macOS安装程序可从[这里](https://github.com/PlatONnetwork/Samurai/releases/download/v0.2.0/Samurai.Setup.0.2.0.5.exe)下载。
2. 安装时双击打开samurai.dmg文件
3. 将其中的samurai.app文件拖到右侧Applications文件夹中，然后就能在Finder的应用程序列表中找到samurai。
```

### Linux安装

```
1. Linux应用程序压缩包可从[这里](https://github.com/PlatONnetwork/Samurai/releases/download/v0.2.0/Samurai.Setup.0.2.0.5.exe)下载。
2. 下载后直接解压缩可找到可执行文件samurai，该文件可直接运行。
```

## 如何使用Samurai

### 网络初始化

- [怎么加入PlatON测试网络](_网络初始化#join_net)

- [如何创建并加入PlatON本地私用网络](_网络初始化#create_private)

### 钱包

- [如何创建一个钱包](_钱包#create_wallet)

- [如何导入/恢复一个已有的钱包](_钱包#import_wallet)

- [如何发送、接收Energon](_钱包#send_recv_energon)

### 共享钱包

- [什么是共享钱包](_共享钱包#what_is)

- [如何创建一个共享钱包](_共享钱包#how_to_create)

- [如何添加已创建的共享钱包](_共享钱包#how_to_add)

- [如何使用共享钱包发送、接收Energon](_共享钱包#how_to_use)

### 交易

- [如何确认交易](_交易#comfire_txs)

### Wasm合约

- [什么是Wasm合约，能干什么](_Wasm合约#what_is_msc)

- [如何部署一个Wasm合约](_Wasm合约#how_to_deploy)

- [如何添加别人已部署的Wasm合约](_Wasm合约#how_to_add)

- [怎样运行wasm合约](_Wasm合约#how_to_run)

