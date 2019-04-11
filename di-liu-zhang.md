# 第六章 合约开发教程

## 6.1 环境部署

### 6.1.1 简介

本系列教程非常适合从开发人员的角度学习UOSIO的细节。

**您将学习什么**

* 如何快速启动节点
* 管理钱包和钥匙
* 创建账户
* 编写智能合约
* 合约编译及ABI
* 部署合约

**开发经验**

* C / C++ 开发经验

基于UOSIO的区块链使用WebAssembly \(WASM\)执行用户生成的应用程序和代码。WASM是一个新兴的web标准，得到了Google、Microsoft、Apple等行业领先公司的广泛支持。

目前，编译WASM的应用程序最成熟的工具链是clang/llvm及其C/ C++编译器。为获得最佳的兼容性，建议您使用UOSIO C++工具链。

第三方开发的其他工具包括:Rust、Python和solid。虽然这些语言可能看起来更简单，但是它们的性能可能会影响您可以编译的应用程序的规模。我们预估C++将是开发高性能和安全智能合约的最佳语言，并计划在可预见的未来使用C++。

* Linux开发经验

UOSIO支持环境：Ubuntu 16.04 及以上

* 命令行知识

UOSIO提供了各种工具，这些工具要求您具备基本的命令行知识，以便与之进行交互。

**C++环境设置**

我们可以使用任何文本编辑器，最好支持C++语法高亮显示。流行的编辑器有Sublime Text和Atom。另一个选项是IDE，它提供更复杂的代码完成和更完整的开发体验。欢迎您使用您个人喜欢的软件，如果您不确定使用什么，我们提供了一些选项供您选择。

可用的编辑器和IDE：

* [Sublime Text](https://www.sublimetext.com/)
* [Atom Editor](https://atom.io/)
* [CLion](https://www.jetbrains.com/clion/)
* [Eclipse](http://www.eclipse.org/downloads/packages/release/oxygen/1a/eclipse-ide-cc-developers)
* [Visual Studio Code](https://code.visualstudio.com/)

**开发环境的操作系统**

如果您使用linux的操作系统，包括但不限于Ubuntu，您就可以轻松地开始开发。

如果您在Windows上进行开发，很遗憾我们目前没有提供powershell端口和指令。将来我们可能会附加powershell命令。与此同时，建议您使用Ubuntu系统的虚拟机，并在这个虚拟机中设置您的开发环境。如果您是一个熟悉移植Linux指令的高级Windows开发人员，您遇到的问题会少很多。

### 6.1.2 准备

**第一步: 安装二进制文件**

本文档使用预编译二进制文件。为了让您尽快开始，这是最好的选择。

以下命令将下载该二进制文件。暂时仅限Ubuntu 16.04。

提示：如果您的系统上已经安装了以前版本的uosio，请卸载之后再继续。

\(此处是测试链的安装包，主链安装包为**UOS\_Mainnet.tar.gz**\)

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
wget ftp://tools.ulord.one/UOS_Testnet.tar.gz
```
{% endcode-tabs-item %}
{% endcode-tabs %}

下载完成后，解压并安装：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
tar -xzvf UOS_Testnet.tar.gz
cd uos
sudo ./install.sh
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**第二步: 新建并进入合约开发目录**

您需要选择一个目录来工作，建议在本地驱动器的某个地方创建一个合约目录。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
mkdir contracts
cd contracts
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**第三步: 获取并保存该目录**

获取该目录的路径并保存，以便后续使用，您可以使用以下命令获取绝对路径。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
pwd
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 6.1.3 安装CDT

UOSIO合约开发工具包\(简称CDT\)，是合约编译相关工具的集合。后续教程主要使用CDT编译合约和生成ABI。

**提示**：如果您已经安装之前的uosio.cdt版本。请卸载之后再继续。

**Ubuntu**

下载

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
wget ftp://tools.ulord.one/uosio.cdt.tar.gz
```
{% endcode-tabs-item %}
{% endcode-tabs %}

解压

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
tar -xzvf uosio.cdt.tar.gz
```
{% endcode-tabs-item %}
{% endcode-tabs %}

安装

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cd uosio.cdt
sudo ./install.sh
```
{% endcode-tabs-item %}
{% endcode-tabs %}

以上命令需要sudo权限，因为uosio.cdt将在本地安装各种二进制文件。系统将要求您输入计算机的帐户密码。

安装uosio.cdt将使编译后的二进制文件全局化，以便在任何地方访问它。对于本文档，**强烈建议您不要跳过uosio.cdt的安装步骤**，如果不安装将使您很难遵循此文档和其他文档，并且通常更难进行合约编译。

### 6.1.4 启动节点和设置

说明：为简化操作，以下节点仅是简单的本地环境部署，并没有加载系统合约。

**第一步: 启动节点和钱包**

**步骤1.1: 启动kuosd**

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
kuosd &
```
{% endcode-tabs-item %}
{% endcode-tabs %}

您将看到如下的输出:

```text
info  2018-11-27T06:54:24.789 thread-0  wallet_plugin.cpp:42          plugin_initialize    ] initializing wallet plugin
info  2018-11-27T06:54:24.795 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/kuosd/stop
info  2018-11-27T06:54:24.796 thread-0  wallet_api_plugin.cpp:73      plugin_startup       ] starting wallet_api_plugin
info  2018-11-27T06:54:24.796 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/create
info  2018-11-27T06:54:24.796 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/create_key
info  2018-11-27T06:54:24.796 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/get_public_keys
```

按回车键退出。

**步骤1.2: 启动noduos**

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
noduos -d ~/uosdata --config-dir ~/uos/cfg --genesis-json ~/uos/cfg/genesis.json
 --contracts-console -filter-on=‘*’ >> noduos.log 2>&1 &
```
{% endcode-tabs-item %}
{% endcode-tabs %}

这些设置实现以下功能：

1. 在开发目录下的uosio目录中为区块链数据和配置分配工作目录。这里我们分别使用~/uos/data和~/uos/cfg。
2. 运行noduos，同时该命令加载所有基本插件。

**第二步: 检查安装**

**步骤 2.1: 检查节点是否正常运行**

运行以下命令

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
tailf noduos.log
```
{% endcode-tabs-item %}
{% endcode-tabs %}

您应该会在控制台中看到如下的输出:

```text
info  2018-12-01T08:19:56.000 thread-0  producer_plugin.cpp:1490      produce_block        ] Produced block 000001a484763d72... #420 @ 2018-12-06T08:19:56.000 signed by uosio [trxs: 0, lib: 419, confirmed: 0]
info  2018-12-01T08:19:57.001 thread-0  producer_plugin.cpp:1490      produce_block        ] Produced block 000001a581e0e64f... #421 @ 2018-12-06T08:19:57.000 signed by uosio [trxs: 0, lib: 420, confirmed: 0]
info  2018-12-01T08:19:58.000 thread-0  producer_plugin.cpp:1490      produce_block        ] Produced block 000001a6c001325e... #422 @ 2018-12-06T08:19:58.000 signed by uosio [trxs: 0, lib: 421, confirmed: 0]
info  2018-12-01T08:19:59.000 thread-0  producer_plugin.cpp:1490      produce_block        ] Produced block 000001a7b44d8a1d... #423 @ 2018-12-06T08:19:59.000 signed by uosio [trxs: 0, lib: 422, confirmed: 0]
```

按ctrl+c关闭日志。

**步骤2.2: 检查钱包**

打开shell并运行以下命令。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet list
```
{% endcode-tabs-item %}
{% endcode-tabs %}

您将看到以下输出，现在还没有钱包。

```bash
Wallets:
[]
```

**步骤2.3: 检查noduos端点**

您需要检查RPC API是否正常工作。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos get info
```
{% endcode-tabs-item %}
{% endcode-tabs %}

您将看到以下输出，表示运行成功。

```bash
{
  "server_version": "1f18fbff",
  "chain_id": "cb1025fad7e421fa75e769f4cdd76a2fc4db639ea7732fd299c6d28d14767771",
  "head_block_num": 3814186,
  "last_irreversible_block_num": 3814163,
  "last_irreversible_block_id": "003a3313f676cea154e9d122072cf6f494890dd0062108a455f40f3451757954",
  "head_block_id": "003a332ad48da528e53a52a78c33a9bc7e55024968f19eee556d6e444afe2ce8",
  "head_block_time": "2019-03-14T07:25:10.000",
  "head_block_producer": "uoszhongguo1",
  "virtual_block_cpu_limit": 450000000,
  "virtual_block_net_limit": 2097152000,
  "block_cpu_limit": 449900,
  "block_net_limit": 2097152,
  "server_version_string": "1.3.2-90-g1f18fbf"
}
```

### 6.1.5 创建开发钱包

**第一步: 创建钱包**

第一步是创建一个钱包。使用“cluos wallet create -n test”创建一个“test”钱包，使用选项--to-console。如果在生产环境中使用该命令，明智的做法是使用--file选项，这样您的钱包秘钥就不会出现在bash历史记录中。由于我们是以开发为目，而不是实际使用，因此--to-console选项不会带来安全隐患。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet create -n test --to-console
```
{% endcode-tabs-item %}
{% endcode-tabs %}

cluos将会返回一个秘钥，**保存该秘钥**以便接下来使用。

```bash
Creating wallet: test
Save password to use in the future to unlock this wallet.
Without password imported keys will not be retrievable.
"PW5KBfdsViLUJ1vr4YQMRkc6qj8BR3wpZpYRhVgjvgyy1VzQy7sWP"
```

注意：使用cluos wallet create --to-console将创建一个名为“default”钱包。

**拓展：关于钱包**

加密货币中对于钱包的一个常见误解是认为它是用来存储代币的。事实上，钱包并不存储代币。钱包的作用是将私钥存储在加密文件中并签署交易。

用户通常通过接口构建事务对象，将该事务发送到要签名的钱包，然后钱包返回具有签名的事务，该签名随后被广播到网络中。当/如果网络确认该事务是有效的，它将被包含到区块链上的区块中。

**第二步: 打开钱包**

默认情况下，在启动kuosd时钱包是关闭的，请先运行以下命令：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet open -n test
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行以下命令获取钱包列表：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet list 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回如下，此时，钱包已被打开，但是还没有解锁。

```bash
Wallets:
[
  "test"
]
```

**第三步: 解锁钱包**

刚才您获得了该钱包的秘钥，现在需要使用它来解锁钱包。运行以下命令：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet unlock -n test
```
{% endcode-tabs-item %}
{% endcode-tabs %}

系统会提示您输入密码，复制秘钥（**创建钱包时生成的秘钥**）然后粘贴并按回车键。

再次运行以下命令：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet list
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回如下：

```bash
Wallets:
[
  "test *"
]
```

特别注意星号（\*）。这意味着钱包目前已解锁。

**第四步: 导入开发私钥**

每个新的UOSIO链都有一个名为“uosio”的默认“系统”用户。此帐户通过加载系统合约来对链进行配置，这些合约规定了对链的管理和共识。每条新的UOSIO链都带有一个开发私钥，这个私钥是相同的。加载此私钥代表系统用户\(uosio\)签署事务。接来下我们将开发私钥导入钱包。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet import -n test
```
{% endcode-tabs-item %}
{% endcode-tabs %}

将提示您输入私钥，请输入下面提供的uosio开发私钥：

```bash
5KWHUBmukJaqccDQ3EGfbR3jpYnMfo4Ys98fGtXY8GEMasyhTRM
```

**提示：**该私钥可以在配置文件config.ini中找到。

**警告：**

切勿将开发私钥用于生产帐户！这样做会导致您无法访问您的帐户，此私钥是公开的。

您现在已经有一个默认钱包，已解锁并导入了一个私钥。

### 6.1.6  创建测试账户

**什么是账户？**

帐户是存储在区块链上的授权集合，用于标识发送者/接收者。它具有灵活的授权结构，使其可以由个人或一组人拥有，具体取决于如何配置权限。您需要拥有一个帐户才能向区块链发送或接收有效的交易。

该系列教程主要使用两个“用户”bob和alice。

**创建测试账户**

我们将创建两个账户，分别为bob和alice。

首先，使用cluos wallet create\_key导入私钥并打印公钥：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet create_key -n test
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回结果如下：

```bash
Created new private key with a public key of: "UOS6PvswBMZqjdQPsa7M6WTPiUfVikMSeThUBi3rzksb29PRoDKWo"
```

使用命令[cluos create account](https://developers.eos.io/eosio-cleos/reference#cleos-create-account)创建账户：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos create account uosio bob UOS6PvswBMZqjdQPsa7M6WTPiUfVikMSeThUBi3rzksb29PRoDKWo
```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后，您会看到事务已被广播的反馈消息，返回如下，表示创建Bob账户成功。

```bash
executed transaction: 40c605006de...  200 bytes  153 us
#         uosio <= uosio::newaccount            {"creator":"uosio","name":"bob","owner":{"threshold":1,"keys":[{"key":"UOS6PvswBMZqjdQPsa7M6WTPiUfVi...
warning: transaction executed locally, but may not be confirmed by the network yet    ]
```

请尝试自己创建alice账户。

**注意：该文档中owner和active权限使用相同的私钥，在生产环境中owner/active请使用不同的私钥。**

UOSIO具有独特的授权结构，为您的帐户增加了安全性。您可以在使用与您的active权限相关联的私钥时保持owner私钥的安全，从而最大限度地减少帐户的风险。这样，如果您的active私钥全部遭到入侵，您可以使用owner私钥重新获得对帐户的控制权。

**故障排除**

如果您在创建帐户时遇到错误，请确保您的钱包已解锁。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet list
```
{% endcode-tabs-item %}
{% endcode-tabs %}

如果钱包已解锁，您应该在钱包名后面看到一个星号（\*），如下所示：

```bash
Wallets:
[
  "test *"
]
```

至此，开发环境已经部署完成。随后，将向您简单介绍合约编写和调用。

## 6.2 Hello合约

### 6.2.1 合约编写

创建合约目录（该文档示例为/root/contracts/）,并在该目录下创建一个名为“hello”的新目录，或者通过系统GUI创建一个名为“hello”的目录，进入该目录。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cd /root
mkdir contracts && cd contracts
mkdir hello && cd hello
```
{% endcode-tabs-item %}
{% endcode-tabs %}

创建一个新文件“hello.cpp”并使用您喜欢的编辑器\(教程使用vim\)打开它。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
touch hello.cpp
vim hello.cpp
```
{% endcode-tabs-item %}
{% endcode-tabs %}

使所需要的库包含在文件中。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
#include <uosiolib/uosio.hpp>
#include <uosiolib/print.hpp>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在合约中使用命名空间uosio，可以使代码更简洁。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
using namespace uosio;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* uosiolib/uosio.hpp将UOSIO C和C ++ API加载到合约中。

创建一个标准的C ++ 11类。合约类需要继承uosio::contract。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
#include <uosiolib/uosio.hpp>
#include <uosiolib/print.hpp>
using namespace uosio;
class hello : public contract {};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

添加公共访问说明符和using声明。这样能让我们写出更简洁的代码。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
#include <uosiolib/uosio.hpp>
#include <uosiolib/print.hpp>
using namespace uosio;
class hello : public contract {
  public:
      using contract::contract;
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

本着hello world的精神编写一个接受“name”参数的action，然后输出该参数。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
#include <uosiolib/uosio.hpp>
#include <uosiolib/print.hpp>
using namespace uosio;
class hello : public contract {
  public:
      using contract::contract;
      void hi( name user ) {
         print( "Hello, ", name{user});
      }
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

上述action接收一个name类型的user参数，UOSIO附带了许多类型定义，name是您遇到最常见的参数之一。使用先前\#include的库函数uosio::print，连接字符串并打印。

在action中添加C++11样式属性，这样abi生成器可以生成更可靠的输出。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
#include <uosiolib/uosio.hpp>
#include <uosiolib/print.hpp>
using namespace uosio;
class [[uosio::contract]] hello : public contract {
  public:
      using contract::contract;
      [[uosio::action]]
      void hi( name user ) {
         print( "Hello, ", name{user});
      }
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

最后，添加UOSIO\_DISPATCH宏来处理调度hello合约的action。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
UOSIO_DISPATCH ( hello, (hi))
```
{% endcode-tabs-item %}
{% endcode-tabs %}

把所有代码组合到一起。至此就完成了hello合约的编写。

{% code-tabs %}
{% code-tabs-item title="hello.cpp" %}
```cpp
#include <uosiolib/uosio.hpp>
#include <uosiolib/print.hpp>
using namespace uosio;
class [[uosio::contract]] hello : public contract {
  public:
      using contract::contract;
      [[uosio::action]]
      void hi( name user ) {
         print( "Hello, ", name{user});
      }
};
UOSIO_DISPATCH ( hello, (hi))
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 6.2.2 合约编译及账户创建

您可以将代码编译为Web程序集（.wasm），如下所示：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
uosio-cpp -o hello.wasm hello.cpp --abigen
```
{% endcode-tabs-item %}
{% endcode-tabs %}

创建合约帐户hello。（创建账户详见 UOS合约开发—环境 文档）

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos create account uosio hello YOUR_PUBLIC_KEY
```
{% endcode-tabs-item %}
{% endcode-tabs %}

提示：1.首先要将uosio账户的私钥导入该钱包；2.YOUR\_PUBLIC\_KEY通过cluos wallet create\_key -n test生成。

### 6.2.3 合约部署及调用

使用cluos set contract命令将编译得到的wasm文件广播到网络中。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos set contract hello /root/contracts/hello -p hello@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**提示**：合约路径可以是绝对路径，也可以是相对路径。

现在合约已经部署，向链上推送一个该合约的action。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos push action hello hi '["bob"]' -p bob@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行结果：

```bash
executed transaction: 4c10c1426c16b1656e802f3302677594731b380b18a44851d38e8b5275072857  244 bytes  1000 cycles
#    hello.code <= hello.code::hi               {"user":"bob"}
>> Hello, bob
```

该合约将允许任何帐户调用该action。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos push action hello hi '["bob"]' -p alice@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行结果：

```bash
executed transaction: 28d92256c8ffd8b0255be324e4596b7c745f50f85722d0c4400471bc184b9a16  244 bytes  1000 cycles
#    hello.code <= hello.code::hi               {"user":"bob"}
>> Hello, bob
```

正如预期的那样，控制台输出也是“Hello，bob”。

在这种情况下，“alice”是授权它的人，而且user只是一个参数。接下来我们修改合约，要求授权用户“alice”必须与action中“hi”的参数相同。我们将使用require\_auth方法，此方法将name作为参数，并将检查执行操作的用户是否与提供的参数匹配。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
void hi(  name user ) {
   require_auth( user );
   print( "Hello, ", name{user} );
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

更新合约代码如下：

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
#include <uosiolib/uosio.hpp>
#include <uosiolib/print.hpp>
using namespace uosio;
class [[uosio::contract]] hello : public contract {
  public:
      using contract::contract;
      [[uosio::action]]
      void hi(  name user ) {
         require_auth( user );
         print( "Hello, ", name{user});
      }
};
UOSIO_DISPATCH( hello, (hi))
```
{% endcode-tabs-item %}
{% endcode-tabs %}

重新编译合约

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
uosio-cpp -o hello.wasm hello.cpp --abigen
```
{% endcode-tabs-item %}
{% endcode-tabs %}

更新合约

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos set contract hello /root/contracts/hello -p hello@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

尝试再次执行操作action，但这次授权账户与参数不匹配。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos push action hello hi '["bob"]' -p alice@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

正如预期的那样，require\_auth暂停了事务并提示错误。

运行结果：

```bash
Error 3090004: Missing required authority
Ensure that you have the related authority inside your transaction!;
If you are currently using 'cluos push action' command, try to add the [relevant](**http://google.com**) authority using -p option.
```

通过对合约的更改，验证user的内容与授权用户相同。再试一次，但这一次，传入alice作为参数，并且用alice账户进行授权。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos push action hello hi '["alice"]' -p alice@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行结果：

```bash
executed transaction: 235bd766c2097f4a698cfb948eb2e709532df8d18458b92c9c6aae74ed8e4518  244 bytes  1000 cycles
#    hello <= hello::hi               {"user":"alice"}
>> Hello, alice
```

至此，一个简单完整的hello合约已经完成！接下来，我们将尝试更复杂的合约编写和探索！



## 6.3 通讯录合约

### 6.3.1 创建一个新目录和文件

之前，我们已经创建了一个合约目录（/root/contracts），现在我们进入该目录并创建一个新的目录（addressbook）。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cd contracts
mkdir addressbook
cd addressbook
```
{% endcode-tabs-item %}
{% endcode-tabs %}

创建一个新的文件。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
touch addressbook.cpp
```
{% endcode-tabs-item %}
{% endcode-tabs %}

使用您喜欢的编辑器打开该文件。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
vim addressbook.cpp
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 6.3.2 编写合约类框架

在之前的教程中，我们创建了一个hello合约，学习了基础知识。您应该了解下面的结构，此处我们修改类名为addressbook。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
#include <uosiolib/uosio.hpp>

using namespace uosio;

class [[uosio::contract]] addressbook : public uosio::contract {
  public:

  private: 

};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 6.3.3 为表创建数据结构

在配置和实例化表之前，需要编写表示通讯录的数据结构，我们姑且称之为“架构”。由于它是一个通讯录，该表将包含人员信息，因此创建一个名为“person”的结构体（struct）。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
struct person {};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

定义multi\_index表的模式时，需要使用唯一值作为主键。

对于此合约，我们使用名为“key”的name类型字段。此合约将为每个用户提供一个唯一条目，因此该“key”将是基于用户且唯一的name类型字段。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
struct person {
         name key; 
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

此外，该结构体应该为每个条目或人员存储一些相关的信息。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
struct person {
 name key;
 string first_name;
 string last_name;
 string street;
 string city;
 string state;
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

基本架构现已完成。接下来，定义一个primary\_key方法以供multi\_index迭代器使用。每个multi\_index架构都需要一个主键。要实现此目的，您只需创建一个名为primary\_key\(\)的方法并返回一个值，在本例中我们使用结构中定义的“key”成员。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
struct person {
 name key;
 string first_name;
 string last_name;
 string street;
 string city;
 string state;

 uint64_t primary_key() const { return key.value;}
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

提示：当表中包含数据时，无法修改表的模式。如果需要更改表的架构，首先需要删除其所有行。

### 6.3.4 配置多索引表

现在已经定义了表的结构，现在我们需要配置表。uosio::multi\_index的构造需要进行命名和配置，从而使用我们前面定义的结构体。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
typedef uosio::multi_index<"people"_n, person> address_index;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

1. 使用\_n运算符定义uosio::name类型并为该表命名。该表包含许多不同的“person”，因此将表命名为“people”。
2. 传入上一步中定义的结构体person。
3. 声明该表的类型。此类型将用于稍后实例化该表。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
//configure the table
typedef uosio::multi_index<"people"_n, person> address_index;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

使用上述multi\_index配置，定义一个名为people的multi\_index表，该表存储person类型的数据。

到目前为止，我们的代码是这样的。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
#include <uosiolib/uosio.hpp>

using namespace uosio;

class [[uosio::contract]] addressbook : public uosio::contract {

  public:

  private:
    struct [[uosio::table]] person {
      name key;
      std::string first_name;
      std::string last_name;
      std::string street;
      std::string city;
      std::string state;

      uint64_t primary_key() const { return key.value;}
    };
    typedef uosio::multi_index<"people"_n, person> address_index;
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 6.3.5 构造函数

使用C++类时，您应该创建的第一个公共方法是构造函数。

我们的构造函数将负责初始化合约。

UOSIO合约继承了contract类。使用合约的名称\(code\)和接收者\(receiver\)初始化我们的contract父类。这里的重要参数是code，该参数指定部署合约的账户名。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
addressbook(name receiver, name code, datastream<const char*> ds) : contract(receiver, code, ds) {}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 6.3.6 向表中添加数据

多索引表的主键被定义为强制此合约仅为每个用户存储一条记录。为使其生效，需要建立一些关于合约的设定。

1. 授权修改通讯录的唯一帐户是用户。
2. 表的primary\_key是唯一的，基于用户名。
3. 就可用性而言，合约应该能够通过单个操作创建和修改表的某一行。

在uos中，链上某个账户是唯一的，因此在这个特定用例中name作为**primary\_key**是理想的候选者。

接下来，为用户定义添加或更新记录的操作。此操作需要接受某些值并创建或修改记录。

格式化定义可以使代码更容易阅读。为了增强用户体验和简化接口，用一个方法负责创建和修改记录。我们将其命名为“upsert”，即“update”和“insert”的组合。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
void upsert(
  name user, 
  std::string first_name, 
  std::string last_name, 
  std::string street, 
  std::string city, 
  std::string state
) {}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

此前，有人提到只有用户自己才能控制自己的记录，因为这个合约是选择性加入的。为此，我们使用uosio.cdt支持的require\_auth方法。此方法接受一个name类型的参数，并验证执行事务的帐户是否和该参数匹配。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state) {
  require_auth( user );
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在，我们将实例化表。我们已经配置了multi\_index表，并将其声明为address\_index。要实例化一个表，请考虑它的两个必须参数：

1. “code”，代表合约帐户。可以通过范围\_code变量访问该值。
2. “scope”，用于确保合约的唯一性，此例中，由于只有一个表，我们也可以使用“\_code.value”。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state) {
  require_auth( user );
  address_index addresses(_code, _code.value);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

接下来，查询迭代器，将其设置为可变的，因为此迭代器将被多次使用。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state) {
  require_auth( user );
  address_index addresses(_code, _code.value);
  auto iterator = addresses.find(user.value);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

至此，权限验证和表格实例化已经完成。接下来，编写用于创建或修改表的逻辑。检测特定用户是否已存在。

为此，请传递user参数来使用表的find方法。find方法将返回一个迭代器。将迭代器与end方法进行比较。“end”方法是“null”的别名。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state) {
  require_auth( user );
  address_index addresses(_code, _code.value);
  auto iterator = addresses.find(user.value);
  if( iterator == addresses.end() )
  {
    //The user isn't in the table
  }
  else {
    //The user is in the table
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

使用multi\_index的emplace方法在表中创建记录。此方法接受两个参数，即支付存储该记录开销的“付款人”和回调函数。

emplace方法的回调函数必须使用lamba来创建引用。在lamba函数体分配“row”的值并将该值传给upsert方法。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state) {
  require_auth( user );
  address_index addresses(_code, _code.value);
  auto iterator = addresses.find(user.value);
  if( iterator == addresses.end() )
  {
    addresses.emplace(user, [&]( auto& row ) {
      row.key = user;
      row.first_name = first_name;
      row.last_name = last_name;
      row.street = street;
      row.city = city;
      row.state = state;
    });
  }
  else {
    //The user is in the table
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

接下来，处理“upsert”函数的修改或更新。使用modify方法，并传递参数

* 在调用“upsert”操作时，前面定义的迭代器设置为声明的用户。
* “scope”或“ram payer”，付费账户，是指由谁提供存储数据所需的内存。
* 回调函数用于处理表的修改。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state) {
  require_auth( user );
  address_index addresses(_code, _code.value);
  auto iterator = addresses.find(user.value);
  if( iterator == addresses.end() )
  {
    addresses.emplace(user, [&]( auto& row ) {
      row.key = user;
      row.first_name = first_name;
      row.last_name = last_name;
      row.street = street;
      row.city = city;
      row.state = state;
    });
  }
  else {
    std::string changes;
    addresses.modify(iterator, user, [&]( auto& row ) {
      row.key = user;
      row.first_name = first_name;
      row.last_name = last_name;
      row.street = street;
      row.city = city;
      row.state = state;
    });
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在，该addressbook合约有一个基本动作：如果记录不存在，用户在表中创建一行，如果已经存在则修改它。

但是如果用户想要完全删除记录呢？

### 6.3.7 从表中删除记录

与前面的步骤类似，在addressbook类中创建一个公共方法，确保包含require\_auth，该函数根据操作的参数user进行验证，仅有记录的所有者可以修改。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
    void erase(name user){
      require_auth(user);
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

实例化表，在表格中每个帐户只有一个记录。通过调用find方法对迭代器进行赋值。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
...
    void erase(name user){
      require_auth(user);
      address_index addresses(_code, _code.value);
      auto iterator = addresses.find(user.value);
    }
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

合约不能删除不存在的记录，因此在删除之前验证记录确实存在。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
...
    void erase(name user){
      require_auth(user);
      address_index addresses(_code, _code.value);
      auto iterator = addresses.find(user.value);
      uosio_assert(iterator != addresses.end(), "Record does not exist");
    }
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

最后，调用表的erase方法，擦除迭代器。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
...
  void erase(name user) {
    require_auth(user);
    address_index addresses(_code, _code.value);
    auto iterator = addresses.find(user.value);
    uosio_assert(iterator != addresses.end(), "Record does not exist");
    addresses.erase(iterator);
  }
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

现在，合约基本完成。用户可以创建、修改和删除记录。但是，合约还没有准备好编译。

### 6.3.8准备ABI

完成以下步骤以完成合约。

#### **6.3.8.1 UOSIO\_DISPATCH**

在文件的底部，使用UOSIO\_DISPATCH宏，传入合约的名称，以及动作“upsert”和“erase”。

该宏将不同的action调用分派给合约中的相应方法进行处理。

在底部添加以下内容，将使我们的cpp文件与UOSIO的wasm解释器兼容。未能包含此声明可能会在部署合约时导致错误。

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
UOSIO_DISPATCH( addressbook, (upsert)(erase) )
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **6.3.8.2 ABI的action声明**

uosio.cdt包含一个ABI Generator，为了它能正常工作，我们的合约需要一些小的声明。

在upsert和erase函数之上添加以下C++11声明

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
[[uosio::action]]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

上述声明将提取操作的内容并在生成的ABI文件中创建必要的ABI结构描述。

#### **6.3.8.3 ABI的table声明**

向表中添加ABI声明。修改合约私有区域中定义的以下行：

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
struct person {
```
{% endcode-tabs-item %}
{% endcode-tabs %}

修改后代码如下：

{% code-tabs %}
{% code-tabs-item title="c++" %}
```cpp
struct [[uosio::table]] person {
```
{% endcode-tabs-item %}
{% endcode-tabs %}

该\[\[uosio::table\]\]声明将对表格添加必要的说明。

现在，我们的合约已经准备好进行编译了。

以下是我们addressbook合约的最终状态：

{% code-tabs %}
{% code-tabs-item title="addressbook.cpp" %}
```cpp
#include <uosiolib/uosio.hpp>
#include <uosiolib/print.hpp>

using namespace uosio;

class [[uosio::contract]] addressbook : public uosio::contract {

public:
  using contract::contract;

  addressbook(name receiver, name code,  datastream<const char*> ds): contract(receiver, code, ds) {}

  [[uosio::action]]
  void upsert(name user, std::string first_name, std::string last_name, std::string street, std::string city, std::string state) {
    require_auth( user );
    address_index addresses(_code, _code.value);
    auto iterator = addresses.find(user.value);
    if( iterator == addresses.end() )
    {
      addresses.emplace(user, [&]( auto& row ) {
       row.key = user;
       row.first_name = first_name;
       row.last_name = last_name;
       row.street = street;
       row.city = city;
       row.state = state;
      });
    }
    else {
      std::string changes;
      addresses.modify(iterator, user, [&]( auto& row ) {
        row.key = user;
        row.first_name = first_name;
        row.last_name = last_name;
        row.street = street;
        row.city = city;
        row.state = state;
      });
    }
  }

  [[uosio::action]]
  void erase(name user) {
    require_auth(user);

    address_index addresses(_self, _code.value);

    auto iterator = addresses.find(user.value);
    uosio_assert(iterator != addresses.end(), "Record does not exist");
    addresses.erase(iterator);
  }

private:
  struct [[uosio::table]] person {
    name key;
    std::string first_name;
    std::string last_name;
    std::string street;
    std::string city;
    std::string state;
    uint64_t primary_key() const { return key.value; }
  };
  typedef uosio::multi_index<"people"_n, person> address_index;

};
UOSIO_DISPATCH( addressbook, (upsert)(erase))
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 6.3.9 编译合约及部署

编译合约，在终端执行以下命令。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
uosio-cpp -o addressbook.wasm addressbook.cpp --abigen
```
{% endcode-tabs-item %}
{% endcode-tabs %}

然后，为合约创建一个addressbook帐户（创建账户详见《UOS合约开发—环境》文档）。

部署addressbook合约。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos set contract addressbook /root/contracts/addressbook -p addressbook@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行结果：

```bash
5f78f9aea400783342b41a989b1b4821ffca006cd76ead38ebdf97428559daa0  5152 bytes  727 us
#         uosio <= uosio::setcode               {"account":"addressbook","vmtype":0,"vmversion":0,"code":"0061736d010000000191011760077f7e7f7f7f7f7f...
#         uosio <= uosio::setabi                {"account":"addressbook","abi":"0e656f73696f3a3a6162692f312e30010c6163636f756e745f6e616d65046e616d65...
warning: transaction executed locally, but may not be confirmed by the network yet    ]
```

### 6.3.10 测试合约

调用“upsert”方法在表中添加一行。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos push action addressbook upsert '["alice", "alice", "liddell", "123 drink me way", "wonderland", "amsterdam"]' -p alice@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行结果：

```bash
executed transaction: 003f787824c7823b2cc8210f34daed592c2cfa66cbbfd4b904308b0dfeb0c811  152 bytes  692 us
#   addressbook <= addressbook::upsert          {"user":"alice","first_name":"alice","last_name":"liddell","street":"123 drink me way","city":"wonde...
```

检查alice能否为其他用户添加记录。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos push action addressbook upsert '["bob", "bob", "is a loser", "doesnt exist", "somewhere", "someplace"]' -p alice@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

正如所料，该合约阻止了alice创建/修改另一个用户的数据。

运行结果：

```bash
Error 3090004: Missing required authority
Ensure that you have the related authority inside your transaction!
If you are currently using 'cluos push action' command, try to add the relevant authority using -p option.
```

检索表格中alice的记录。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos get table addressbook addressbook people --lower alice --limit 1
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行结果：

```bash
{
  "rows": [{
      "key": "alice",
      "first_name": "alice",
      "last_name": "liddell",
      "street": "123 drink me way",
      "city": "wonderland",
      "state": "amsterdam"
    }
  ],
  "more": false
}
```

测试alice是否可以删除自己的记录。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos push action addressbook erase '["alice"]' -p alice@active
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行结果：

```bash
executed transaction: 0a690e21f259bb4e37242cdb57d768a49a95e39a83749a02bced652ac4b3f4ed  104 bytes  1623 us
#   addressbook <= addressbook::erase           {"user":"alice"}
warning: transaction executed locally, but may not be confirmed by the network yet    ]
```

检查记录是否已被删除。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos get table addressbook addressbook people --lower alice --limit 1
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行结果：

```bash
{
  "rows": [],
  "more": false
}
```

至此，如何配置表、实例化表、创建新行、修改现有行以及如何使用迭代器已经展示完成。也展示了如何针对空迭代器的结果进行测试，以及如何配置合约的ABI。

