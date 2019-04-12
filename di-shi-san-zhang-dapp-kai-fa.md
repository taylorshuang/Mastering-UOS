# 第十三章 DApp开发

## **概述**

DApp是Decentralized Application的缩写，译为:分散式的应用程序。App我们都知道，我们在智能移动端上安装的应用程序也就是App。而DApp比App多了一个‘D’，‘D’的意思是分散式的。所以，它的意思是分散式的应用程序/去中心化的应用程序。

## **准备**

为了能够开发UOS DApps，您需要懂得使用C / C ++技术开发，因为这是用于UOS智能合约的编程语言。在DApp开发之前，我们需要准备以下技能或工具：

1.   C / C ++

2.   Linux命令行知识

3.   编辑器或IDE（eclipse、clion、Sublime Text、vs code等）

4.   一个运行的本地测试网络节点

5.   前、后端服务器

## **基本流程**

1.  创建账户

   使用rpc命令cluos system newaccount。

2.  开发、编译合约

   使用合约编译工具uosio.cdt进行编译

3.  部署、调试合约

   使用rpc命令cluos set contract 部署合约。

   由于没有专用的调试工具，一般使用print函数打印信息进行调试。

4.  DApp前后端开发

   需要使用相关的api及uosjs相关库文件。

5.  合约及前后端联调

6.  测试

## **注意事项**

1.  通常情况下，DApp需要调用钱包，我们具有支持Chrome浏览器的钱包工具—Usmart。请到Chrome网上应用店查询Usmart并安装。

2.  uosio.cdt并不能用于编译老版本的合约，因此开发合约时请按照uosio.cdt的语法规则及合约格式。

3.  合约调试时，注意设置配置文件config.ini中contracts-console选项为true。

4.  uosjs还未完善，因此，DApp前后端开发会存在一些困难，后续我们将持续完善。

