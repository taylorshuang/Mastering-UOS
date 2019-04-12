# 第十一章 uosio.cdt

## uosio.cdt概述

CDT 的英文全程是 Contract Development Toolkit，翻译成中文就是 「合约开发工具」。CDT 是专门用于编译 C++11 为 .wasm 和创建 ABI 文件的工具。

CDT 的官方网址为 https://github.com/UOSIO/uosio.cdt，当前最新的版本为 1.5.0

## 构建 CDT 环境

我们接下来的教程将主要使用 CDT 来构建合约和生成 ABI。

### 编译安装 uosio.cdt

在编译 CDT 之前，我们先要将远程代码 clone 到本地，本地 uosio.cdt 的保存位置并不重要，因为我们将在以后的步骤中将 uosio.cdt 安装为本地二进制文件。

现在，你可以将 uosio.cdt clone 到先前创建的 contracts 目录，或者你认为适合的本地系统上的任何其他位置。

clone v1.5.0 的 uosio.cdt 代码到本地的目录

https://github.com/UlordChain/uosio.cdt.git

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
git clone --recursive https://github.com/UlordChain/uosio.cdt.git

```
{% endcode-tabs-item %}
{% endcode-tabs %}

仓库有点大，所以 clone 可能会消耗那么点时间，大约 5-30 分钟左右。

clone 完成后，然后进入uosio.cdt 目录

#### 编译 uosio.cdt

运行下面的命令开始构建：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
./build.sh
```
{% endcode-tabs-item %}
{% endcode-tabs %}

构建可能需要需要一些时间，构建完成后，运行下面的命令安装

#### 安装uosio.cdt

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
sudo ./install.sh
```
{% endcode-tabs-item %}
{% endcode-tabs %}

上面的命令需要使用 sudo 超级用户运行，因为 uosio.cdt 的各种二进制文件将在本地安装。系统将要求您提供计算机的帐户密码。安装 uosio.cdt 将使编译后的二进制文件全局化，因此可以在任何地方访问它。

强烈建议您不要跳过 uosio.cdt 的全局安装步骤，如果不安装将使得合约编译变得困难，并且容易出现错误。

检查安装是否正确，输入以下命令查看当前的 uosio.cdt 工具的版本：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
uosio-cpp  --version
```
{% endcode-tabs-item %}
{% endcode-tabs %}

输出结果应该如下：

```text
uosio-cpp version 1.5.0
```

## 合约编译示例

合约编写参见[第九章的helo.cpp](https://mastering-uos.gitbook.io/mastering-uos/di-jiu-zhang-he-yue-kai-fa-jiao-cheng#62-hello-he-yue) 的编写。

编写完以后使用 uosio-cpp 进行编译。

您可以将代码编译为Web程序集（.wasm），如下所示：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
uosio-cpp -o hello.wasm hello.cpp --abigen
```
{% endcode-tabs-item %}
{% endcode-tabs %}

会生成 hello.abi  hello.wasm 编译后的合约文件。以后就可以使用这两个文件进行合约的发布了。

