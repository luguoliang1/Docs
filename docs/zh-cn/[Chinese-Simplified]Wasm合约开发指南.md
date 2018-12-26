> 编写Wasm合约的快速教程

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**内容列表**

- [Wasm合约概述](#Wasm%e5%90%88%e7%ba%a6%e6%a6%82%e8%bf%b0)
    - [Wasm合约简述](#Wasm%e5%90%88%e7%ba%a6%e7%ae%80%e8%bf%b0)
    - [pWASM虚拟机](#pWASM%e8%99%9a%e6%8b%9f%e6%9c%ba)
- [安装Wasm合约编译器](#%e5%ae%89%e8%a3%85Wasm%e5%90%88%e7%ba%a6%e7%bc%96%e8%af%91%e5%99%a8)
    - [安装环境要求](#%e5%ae%89%e8%a3%85%e7%8e%af%e5%a2%83%e8%a6%81%e6%b1%82)
    - [配置合约开发环境](#%e9%85%8d%e7%bd%ae%e5%90%88%e7%ba%a6%e5%bc%80%e5%8f%91%e7%8e%af%e5%a2%83)
    - [合约发布及测试](#%e5%90%88%e7%ba%a6%e5%8f%91%e5%b8%83%e5%8f%8a%e6%b5%8b%e8%af%95)
    - [常见问题](#%e5%b8%b8%e8%a7%81%e9%97%ae%e9%a2%98)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


## Wasm合约概述

### Wasm合约简述

Wasm合约是编译成Webassembly字节码，并部署和运行在PlatON上。用户只需要通过Samurai客户端提交Wasm合约代码到PlatON上，就可以对外发布服务，且只需要为执行代码过程中消耗的资源支付能量。

Wasm合约采用`c++`语法进行合约的编写，为了更好与`PlatON`平台的进行整合，同时提供了
的丰富类库 - [Wasm合约内置库](https://platonnetwork.github.io/pwasmdoc/doc/index.html)。结合二者可以让我们更容易的编写出高性能、功能强大的智能合约。

### pWASM虚拟机

`pWASM`虚拟机 是Wasm合约的运行环境。通过沙盒化运行，进行完全隔离来保障合约运行环境的安全性。合约的逻辑无法访问网络、文件系统和其他外部的资源。
甚至Wasm合约之间的访问也是受限的。

## 安装Wasm合约编译器

### 安装环境要求

> 本指南涉及Wasm合约开发环境为 `Windows` 系统，更多环境敬请期待。

* cmake 用于生成构建文件 [CMake下载](https://cmake.org/download/)
* mingw 用于执行构建文件 [Mingw64下载](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/8.1.0/threads-posix/seh/x86_64-8.1.0-release-posix-seh-rt_v6-rev0.7z/download)
* git-bash `window`环境下强烈建议安装 `gitbash`，很多执行脚本需要借助此工具完成[安装包下载](https://git-scm.com/downloads)

**CMake安装**

一键式安装，根据提示一步一步安装即可。安装过程中，注意允许添加PATH环境变量。如果错过，请手动将cmake下bin文件夹所在路径添加到PATH。

**mingw安装**

下载后无需执行安装动作，配置`PATH`环境变量即可。


### 配置合约开发环境

#### 下载官方合约开发工具包

- `window` 版本：[点击下载](https://download.platon.network/pWASM.zip)
- `linux`&nbsp;&nbsp;&nbsp;版本：点击下载

从上述地址下载后会得到一个压缩包，解压到工作目录 `D:\` 。以下关于合约环境搭建的操作均在该目录下完成。
另外假定获取包名为：`PlatON-Contract.tar.gz` 。解压完成后得到Wasm源码包：`PlatON-Contract` ，该目录文件结构如下：

```txt
├── CMakeLists.txt
├── CMakeModules
├── boost
├── rapidjson
├── docs
├── external
├── hello
├── libc
├── libc++
├── platonlib
├── README.md
├── script
└── user
```

部分文件夹功能概述：
* `external` 用于存放编译过程中需要的二进制包，主要包含`abi`与`bin`文件的生成工具链
* `hello` 存放了入门示例合约
* `user` 能量合约范例，仅供参考。同时也用于存放脚本自动生成合约模板文件
* `platonlib` PlatON 专供的合约与区块链交互的库。具体参考：[PlatON合约内置库](https://platonnetwork.github.io/pwasmdoc/doc/index.html)


#### 编译及构建

构建环境使用了 `cmake` 的规范，构建前请确保本机的 `cmake` 环境已准备就绪。在工作目录下dos窗口执行如下命令：

**Windows 命令行**

```shell
D:\> cd PlatON-Contract
D:\PlatON-Contract> mkdir build
D:\PlatON-Contract> cd build 
D:\PlatON-Contract> cmake .. -G "MinGW Makefiles" -DCMAKE_SH="CMAKE_SH-NOTFOUND" -DWASM_ROOT=..\external-DCLANG_ROOT=..\external -DPLATON_TOOL_ROOT=..\external
```

```
参数选项：`-DWASM_ROOT` `-DCLANG_ROOT` `-DPLATON_ROOT` 用于指明构建必须工具链所存放的根目录。
```

编译完成后，会在 `build` 目录生成对应的 `CMake` 文件，可用于进行下一步构建：
```text
├──build
  ├── CMakeCache.txt
  ├── CMakeFiles
  ├── cmake_install.cmake
  ├── hello
  ├── libc
  ├── libc++
  ├── Makefile
  ├── platonlib
  └── user
...
```

##### 编译Wasm开发环境

Windows 下执行 `git-bash.exe` 文件以打开 `git-bash` 窗口。假设工具包根目录 {PlatON-Contract} 为 `D:\PlatON-Contract`。进入 `build` 目录下执行:

```shell
$ cd {PlatON-Contract}/build
$ mingw32-make.exe
```

编译完成后，目录 `hello` 和 `user` 下会生成编译后的文件，主要有：
```text
build/
  ├─hello/
    ├── CMakeFiles
    ├── cmake_install.cmake
    ├── hello.bc
    ├── hello.cpp.abi.json
    ├── hello.cpp.bc
    ├── hello.s
    ├── hello.wasm
    ├── hello.wast
    └── Makefile
```

```text
build/
  ├─user/energon/
    ├── CMakeFiles
    ├── cmake_install.cmake
    ├── Makefile
    ├── energon.bc
    ├── energon.cpp.abi.json
    ├── energon.cpp.bc
    ├── energon.s
    ├── energon.wasm
    └── energon.wast 
```

**主要文件简介：**

* `.bc` 连接标准库后的LLVM字节码文件，含所有依赖库；
* `.json` 合约对应的接口描述文件；
* `.s` 汇编文件；
* `.wasm` 合约编译后的二进制文件，字节码指令集； 
* `.wast` 合约编译后的可被认为读懂的指令文件；
* `.cpp.bc` ；LLVM 字节码文件，仅包含合约代码本身的；

**提示：**
如果在编译中出现如下错误信息，请不要惊慌，正常输出。

```code
Could not auto-detect compilation database for file "/workspace/Platon-contract/hello/hello.cpp"
No compilation database found in /workspace/Platon-contract/hello or any parent directory
json-compilation-database: Error while opening JSON database: No such file or directory 
```

#### 编写第一个Wasm合约

下面我们演示如何创建一个自己的`hello world` 合约，主要的功能是完成智能合约的数据上链存储与查询功能，并通过事件把结果返回给调用者。

##### 初始化合约开发模板文件

使用脚本 `{PlatON-Contract}/script/autoproject.sh` 可以快速构建工程目录。命令如下：

```shell
$ cd {PlatON-Contract}
$ ./script/autoproject.sh ./ firstdemo 
```

该 `firstdemo` 工程会默认在 `user` 目录生成。此时新生成的工程目录结构为：

```text
├──build/ 
├──user/
  ├──firstdemo/
  ├── firstdemo.cpp
  └── CMakeLists.txt 
```

目录文件简述：
* `firstdemo.cpp` 为合约源文件，新的合约逻辑可以基于此文件进行更改
* `CMakeLists.txt` CMake 的描述文件

默认生成的文件内容为空，后续的合约内容需要自行编写。


##### 一个简单合约示例

```c++
#include <stdlib.h>
#include <string.h>
#include <string>
#include <platon/platon.hpp>

namespace demo {
    class FirstDemo : public platon::Contract
    {
        public:
            FirstDemo(){}
			
            /// 实现父类: platon::Contract 的虚函数
            /// 该函数在合约首次发布时执行，仅调用一次
            void init() 
            {
                platon::println("init success...");
            }

            /// 定义Event.
            PLATON_EVENT(Notify, uint64_t, const char *)

        public:
            void invokeNotify(const char *msg)
            {	
                // 定义状态变量
                platon::setState("NAME_KEY", std::string(msg));
                // 日志输出
                platon::println("into invokeNotify...");
                // 事件返回
                PLATON_EMIT_EVENT(Notify, 0, "Insufficient value for the method.");
            }

            const char* getName() const 
            {
                std::string value;
                platon::getState("NAME_KEY", value);
                // 读取合约数据并返回
                return value.c_str();
            }
    };
}

// 此处定义的函数会生成ABI文件供外部调用
PLATON_ABI(demo::FirstDemo, invokeNotify)
PLATON_ABI(demo::FirstDemo, getName)
```

##### 编译合约

1. 合约编译是为了将`.cpp` 源码编译成所需要的`abi`与`wasm`二进制文件（虚拟机指令集）。
2. 使用示例中的合约替换默认生成的 `firstdemo.cpp` 文件，完成合约部署。
3. 打开 `git-bash` 命令窗口，执行编译命令：

```shell
$ cd {PlatON-Contract}/build/
$ mingw32-make.exe 
```

执行完成后，在`build/user`目录下会生成测试目录：`firstdemo`，同时也生成了目标文件，例如:

```text
├──build/ 
  ├──user/
    ├──firstdemo/
      ├── firstdemo.s
      ├── firstdemo.bc 
      ├── firstdemo.wast 
      ├── firstdemo.wasm 
      ├── firstdemo.cpp.abi.json
      ├── firstdemo.cpp.bc 
      └── cmake_install.cmake 
```

**注意：**
上述目录中，在进行合约发布需要用到的是 `firstdemo.wast` 和 `firstdemo.cpp.abi.json`。

### 合约发布及测试

#### 测试工具介绍

PlatON 平台提供了合约测试工具`ctool` ：

- Windows版本 [点击下载](https://download.platon.network/ctool.exe)
- Linux  点击下载

**ctool工具用法**

```bash
$ ./ctool.exe -cmd <command> [-addr contractAddress] [--func funcInfo] -abi <abi_path> -code <wasm_path> [-config <config_path>]
```

**参数：**

* `command` ： 待执行的命令。更多功能选项请使用 `./ctool.exe help` 查询
* `contractAddress `：
* `funcInfo` ：
* `abi_path` ：ABI文件的路径。示例中为：`firstdemo.cpp.abi.json` 文件所在路径
* `wasm_path` ：WASM文件路径。示例中为：`firstdemo.wasm` 文件所在路径
* `config_path` ：配置文件路径。配置文件用于设置[PlatON节点](https://github.com/PlatONnetwork/wiki/wiki/%5BChinese-Simplified%5D%E5%BF%AB%E9%80%9F%E6%8C%87%E5%8D%97)及账户信息

**提示：**
如果命令行中未明确指明配置文件路径，则会在当前工作路径读取文件：`config.json`。 

配置文件示例：
```JSON
{
  "url":"http://127.0.0.1:8545",
  "gas": "0x9788888",
  "gasPrice": "0x50de00",
  "from":"0x60ceca9c1290ee56b98d4e160ef0453f7c40d219"
} 
```

配置文件字段简述：
- `url` PlatON开放的`JSON-RPC`地址信息； 
- `gas` 本次交易所付出的能量数；
- `gasPrice` 本次交易付出的能量单位值；
- `from` 交易发送者的地址，注意，需要节点保证了该账户已进行了解锁动作；


##### 发布合约

在编译目录`{PlatON-Contract}/build/user/firstdemo` 中创建 `config.json` 配置文件后进行下列操作。

```shell
$ cd {PlatON-Contract}/build/user/firstdemo 
$ ./ctool.exe -cmd deploy -abi ./firstdemo.cpp.abi.json -code ./firstdemo.wasm --config ./config.json 
```

命令执行成功后，会返回合约地址：
```
trasaction hash: 0xdb0f9a28fcd447702e8d5961f47144d1ea830979e3c984acc8f72c0dca8bdcfc
contract address: 0x43355c787c50b647c425f594b441d4bd751951c1
```
如果下一步需要进行合约调用则需要记录该地址信息！

##### 调用合约（发送交易）

合约发布成功后，接下来进行合约调用，示例合约对外暴露了两个方法:`invokeNotify` `getName`，此处先发送交易调用`invokeNotify`，然后调用`getName`获取值。操作如下：

**发送交易**

```shell 
$ cd {PlatON-Contract}/build/user/firstdemo
$ ./ctool.exe -cmd invoke -addr "0x43355c787c50b647c425f594b441d4bd751951c1" --func 'invokeNotify("HelloWorld")' --abi ./firstdemo.cpp.abi.json --config ./config.json
```

**交易查询**

```shell 
$ cd {PlatON-Contract}/build/user/firstdemo
$ ./ctool.exe -cmd invoke -addr "0x43355c787c50b647c425f594b441d4bd751951c1" --func 'getName()' --abi ./firstdemo.cpp.abi.json --config ./config.json
```

> 实际操作中 `invokeNotify` 传入的参数在 `getName` 中查询出来，表示合约发布、交易、查询过程已全部成功。

### 常见问题

#### 基础问题

##### 合约无法发布问题？

**A**: 请确保配置文件中的能量设置合理，使用的账户已在节点侧解锁。

#### 高级问题


#### 更多问题

如果你有其他问题，或者你的问题在这里找不到答案，请在此提交一个 [issue](https://github.com/PlatONnetwork/PlatON-Go/issues/new) 。
