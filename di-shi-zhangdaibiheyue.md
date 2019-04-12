# 第十章 代币合约

在UOS中，可以说一切皆为合约，UOS的系统代币“UOS”也是通过合约来发放的，合约就像是银行，规定代币的发放以及流转规则。用户可以在自己的账户上部署代币合约，就像在自己的土地上盖起一栋美联储。甚至可以定制合约，制定独特的代币的发放和流转规则。

## 10.1 UOS代币合约       <a id="b5d26a2e"></a>

UOS系统的代币合约部署于账户“uosio.token”

合约的action如下：

> create：创建一个代币，系统代币是“UOS”,精度为“0.0001”
>
> issue ：代币的创建者，系统代币的创建者为uosio.union
>
> transfer ：代币转账
>
> withdraw ：代币销毁

查看当前系统代币的发放状态：

> cluos -u [https://rpc3.uosio.org:8080](https://rpc3.uosio.org:8080) get currency stats uosio.token UOS

查看某用户的可用余额\(可用余额不包含抵押部分，抵押资金需要取消抵押后三天才能转化为可用余额\)：

> cluos -u [https://rpc3.uosio.org:8080](https://rpc3.uosio.org:8080) get currency balance uosio.token baodaotulong UOS

## 10.2 发行自己的代币       <a id="cd97cda2"></a>

UOS的每个用户均可以发行代币，可以部署和uosio.token一样的合约，也使用自己定义

假设编译好的代币合约文件夹\(uosio.token\)存放在目录~/uosio\_test/contracts

![](.gitbook/assets/32-mu-lu.png)

​ 将代币合约部署于bithumbshiny:

> cluos -u [https://rpc3.uosio.org:8080](https://rpc3.uosio.org:8080) set contract bithumbshiny ~/uosio\_test/contracts/uosio.token/ -p bithumbshiny@active
>
> 交易id:e54106395abe2208752a11d88b5ad86b43c0a7deb6fa93cf4e336cb215cd15bb

bithumbshiny自己创建代币，代币名称：RMC，精度为0.0001，最大发行为100亿：

> cluos -u [https://rpc3.uosio.org:8080](https://rpc3.uosio.org:8080) push action bithumbshiny create '\["bithumbshiny", "10000000000.0000 RMC"\]' -p bithumbshiny

直接给用户baodaotulong发放10个RMC:

> cluos -u [https://rpc3.uosio.org:8080](https://rpc3.uosio.org:8080) push action bithumbshiny issue '\["baodaotulong", "10.0000 RMC", ""\]' -p bithumbshiny

用户查看自己获得的RMC代币：

> cluos -u [https://rpc3.uosio.org:8080](https://rpc3.uosio.org:8080) get currency balance bithumbshiny baodaotulong RMC

baodaotulong给aaaaaaaaaaaa转账5个RMC:

> cluos -u [https://rpc3.uosio.org:8080](https://rpc3.uosio.org:8080) transfer -c bithumbshiny baodaotulong aaaaaaaaaaaa "5.0000 RMC"

bithumbshiny销毁自己的RMC:

> cluos -u [https://rpc3.uosio.org:8080](https://rpc3.uosio.org:8080) push action bithumbshiny withdraw '\["baodaotulong" , "5.0000 RMC" ,"" \]' -p baodaotulong

查看RMC代币发放状态：

> cluos -u [https://rpc3.uosio.org:8080](https://rpc3.uosio.org:8080) get currency stats bithumbshiny RMC

