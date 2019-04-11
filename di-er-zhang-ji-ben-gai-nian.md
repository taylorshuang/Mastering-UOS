---
description: >-
  UOS看似庞大复杂，是因为它提出了一些与其他区块链截然不同的概念。本章介绍UOS系统中的一些基本概念，在阅读本章之前，希望大家注意的是:
  UOS同绝大多数区块链一样，通过许多节点冗余工作确保共识的准确，所以节点数目的增加带来的是链的安全性增加，并不能带来资源的增加。
---

# 第二章 基本概念

  
UOS看似庞大复杂，是因为它提出了一些与其他区块链截然不同的概念。本章介绍UOS系统中的一些基本概念，在阅读本章之前，希望大家注意的是: UOS同绝大多数区块链一样，通过许多节点冗余工作确保共识的准确，所以节点数目的增加带来的是链的安全性增加，并不能带来资源的增加。

## 2.1 账户 <a id="71053e1c"></a>

账户是对UOS用户的指代，于UOS而言，每一个账户即一个用户。UOS账户名称由小写字母a-z,数字1-5组成，普通用户的长度为12位，小于12位的称为短号，短号要通过竞拍，或者由其他短号创建获得。

用命令查看一个UOS的账户：

> cluos --url http://rpc.uos.iccob.com:9008 get account "baodaotulong"
>
> created: 2019-03-29T08:46:56.000
>
> permissions:
>
>     owner     1:    1 UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y
>
>        active     1:    1 UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y
>
> memory:
>
>     quota:     3.491 KiB    used:      3.49 KiB  
>
> net bandwidth:
>
>     staked:          0.1000 UOS           \(total stake delegated from account to self\)
>
>     delegated:       0.0000 UOS           \(total staked delegated to account from others\)
>
>     used:               432 bytes
>
>     available:        15.63 MiB  
>
>     limit:            15.63 MiB  
>
> cpu bandwidth:
>
>     staked:          0.1000 UOS           \(total stake delegated from account to self\)
>
>     delegated:       0.0000 UOS           \(total staked delegated to account from others\)
>
>     used:             2.939 ms  
>
>     available:        2.122 sec  
>
>     limit:            2.125 sec  
>
> UOS balances:
>
>     liquid:            1.3648 UOS
>
>     staked:            0.2000 UOS
>
>     unstaking:         0.0000 UOS
>
>     total:             1.5648 UOS
>
> producers:
>
>     staked:            0.2000 UOS
>
>     &lt;not voted&gt;
>
>     total:             0.0000 UOSUOS的

一个账户的基本信息包含权限组成、内存信息、cpu信息、net信息、余额信息、投票信息。

### 2.1.1 权限组成 <a id="196b358d"></a>

UOS的账户默认有两个权限：owner和active。拥有对应权限的私钥，便可以用使用该用户的相应权限。active权限用来进行一般操作，比如转账、购买内存、抵押、投票调用一般合约。owner权限能行使active权限的一切操作，并且能够修改用户的active权限组成和owner权限组成。

比如：baodaotulong的owner和active权限均是由公钥组成：

> UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y

那么，只要拥有上面公钥对应的私钥，我们便可以在地球上每一个有网络的地方使用“baodaotulong”账户的任何合法操作。我们把用户某权限的私钥称为该用户该权限的**权限私钥。**

账户的某一权限可以由公钥组成，也可以有其他账户的权限组成\(比如uosio.prods\)，或者由他们相互结合组成。若是A账户的X权限由B账户Y权限组成，则拥有B账户Y权限的“权限私钥”就可以解锁A的X权限。

对如下权限：

> active     1:    1 UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y

active的后的第一个“１”是**权限阈值，公钥前面的“１”是该公钥的权重，要解锁某一权限，需要权限私钥对应的权重之和要能大于或等于权限阈值。关于权限更多介绍，可以跳转到多重签名章节。**

内存信息、cpu信息、net信息和投票信息将在以后的章节中介绍。

## 2.2 交易 <a id="133498b1"></a>

UOS中，任何一个操作最后均归结为合约的调用，**交易实际是由用户授权的一个或多个合约调用的组合。**

### 2.2.1 交易组成 <a id="085f31e0"></a>

一个交易包含一个或多个action,每一个action对应一次合约的直接调用。比如一条转账交易，是调用uosio.token合约的transfer操作，一条交易可以用一个json来描述，可以通过http命令把某个合法交易的json发送给节点打包执行。如id为如下的交易

> 366616970c7995a935e3263571bd543937a6512cc358c66a91ed449fbb0dcd21

> "trx": {
>
>       "expiration": "2019-04-03T04:10:59",　
>
>       "ref\_block\_num": 8415,
>
>       "ref\_block\_prefix": 725978374,
>
>       "max\_net\_usage\_words": 0,
>
>       "max\_cpu\_usage\_ms": 0,
>
>       "delay\_sec": 0,
>
>       "context\_free\_actions": \[\],
>
>       "actions": \[
>
>         {
>
>           "account": "uosio.token",
>
>           "name": "transfer",
>
>           "authorization": \[
>
>             {
>
>               "actor": "uosiopokerbp",
>
>               "permission": "active"
>
>             }
>
>           \],
>
>           "data": {
>
>             "from": "uosiopokerbp",
>
>             "to": "gongyingfund",
>
>             "quantity": "195.0000 UOS",
>
>             "memo": ""
>
>           },
>
>           "hex\_data": "50cf559056ea30d590a65e6c3acf266530c11d000000000004554f530000000000"
>
>         }
>
>       \],
>
>       "transaction\_extensions": \[\],
>
>       "signatures": \[
>
>         "SIG\_K1\_KbJgJmMUKBVmmhCg6yUsJKifeZDbAFpQd78bbDvTkVemCbjbNUq2meDxXWUMCpCV9tsR5YWV9qCXbsY41AQUunZ5bwY83q"
>
>       \],
>
>       "context\_free\_data": \[\]
>
>     }

expiration：交易的过期时间，交易在UOS上的最迟执行时间，如果交易在expiration还未执行，则此交易再也不会被执行。

ref\_block\_num：交易的参考区块号

ref\_block\_prefix：交易的参考区块的hash前缀

max\_net\_usage\_words：交易的net使用上限，为０不设限制

max\_cpu\_usage\_ms：交易的cpu使用上限，为０不受限制

delay\_sec：交易的延迟时间，为０代表非延迟交易

context\_free\_actions：上下文无关的行为。

### 2.2.3 actions <a id="9c17f6a6"></a>

此交易的具体调用的合约，上文中的转账交易直接调用的合约的文字解释如下：

> uosiopokerbp用户使用自己的active权限，调用是uosio.token合约的transfer方法，传入参数为：一个json:
>
> {
>
>             "from": "uosiopokerbp",
>
>             "to": "gongyingfund",
>
>             "quantity": "195.0000 UOS",
>
>             "memo": ""
>
> }

### 2.2.4 signatures <a id="df476357"></a>

此交易的签名。需要与actions声明的调用权限一致，如果声明了uosiopokerbp的active权限，就需要用uosiopokerbp账号active权限的权限私钥进行签名。

在区块链浏览器上查看这条交易：

[https://explorer.uosio.org/transactions/5513440/366616970c7995a935e3263571bd543937a6512cc358c66a91ed449fbb0dcd21](https://explorer.uosio.org/transactions/5513440/366616970c7995a935e3263571bd543937a6512cc358c66a91ed449fbb0dcd21)

## 2.3 内存、cpu、net <a id="18a6625f"></a>

uos的资源包含：cpu、net、内存\(ram\)。用户的每个操作都需要链来执行，执行需要花费一定的cpu资源，而每个操作都是用数据来进行表达的，需要在区块中占据一定的容量，用net资源计量。合约需要非易失性的存储某些数据，事实上，这些非易失性数据是存在与每一个节点上，是物理受限的，这些以ram计量。

这一节我们还是用“baodaotulong”这个账号举例：

> cluos --url http://rpc.uos.iccob.com:9008 get account "baodaotulong"
>
> created: 2019-03-29T08:46:56.000
>
> permissions:
>
>     owner     1:    1 UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y
>
>        active     1:    1 UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y
>
> memory:
>
>     quota:     3.491 KiB    used:      3.49 KiB  
>
> net bandwidth:
>
>     staked:          0.1000 UOS           \(total stake delegated from account to self\)
>
>     delegated:       0.0000 UOS           \(total staked delegated to account from others\)
>
>     used:               432 bytes
>
>     available:        15.63 MiB  
>
>     limit:            15.63 MiB  
>
> cpu bandwidth:
>
>     staked:          0.1000 UOS           \(total stake delegated from account to self\)
>
>     delegated:       0.0000 UOS           \(total staked delegated to account from others\)
>
>     used:             2.939 ms  
>
>     available:        2.122 sec  
>
>     limit:            2.125 sec  
>
> UOS balances:
>
>     liquid:            1.3648 UOS
>
>     staked:            0.2000 UOS
>
>     unstaking:         0.0000 UOS
>
>     total:             1.5648 UOS
>
> producers:
>
>     staked:            0.2000 UOS
>
>     &lt;not voted&gt;
>
>     total:             0.0000 UOSUOS的

### 2.3.1 内存 <a id="3b4d929f"></a>

UOS的节点本质运行在具体的物理计算机上的一个程序，这个程序将一些常用的数据储存在计算机的内存之中，账户的基本信息便是其中的一种。由于UOS的区块链特性，为了确保“共识”的准确，每一个节点运行的程序是一样的，所以每一个节点的UOS程序在内存中存储的是同一份状态数据，因此内存资源的容量于节点数目无关，并不会因为节点的增多而增多。因为计算机物理内存资源有限并且成本高昂，所以UOS的内存是付费使用的。

比如：baodaotulong购买了“3.491KB”的内存，已经使用了“3.49KB”的内存，那么他只有0.001KB的内存用来存放自己的某些数据了。

UOS的内存容量现为８G,这意味这每个UOS节点程序要求自己计算机的内存里面有８G的容量是用来存“常用的状数据”的。这些常用的包含合约本身，以及合约内部自定义的一些数据。

**UOS的内存**是根据bancor协议进行自动计费的。

公式如下：

> 得到的容量\(Byte\) = \(当前剩余的容量 \* 付款的UOS\)/\(池中的UOS　+　付款的UOS\)
>
> 得到的UOS = \(池中的UOS　\*　卖出的RAM\) / \(剩余的容量　＋　卖出的RAM\)

内存资金池的初始值参数为500W,所以由第一个公式，当有500W的UOS入场内存时，池中的UOS为1000W，计算出：得到的容量= 总容量/2 = 4GB 。

接下来，详细价格如下表：

​                                     ![](https://cdn.nlark.com/yuque/358/2019/png/127101/1554712015489-31603e0c-5670-44b1-80f9-a319e11b6075.png)                                           ​

                                                          UOS内存价格表.png

作图如下：

​                                        ![](https://cdn.nlark.com/yuque/358/2019/png/127101/1554712015491-fb3034ed-73e9-42d0-a9cc-d3e002fe5b32.png)                                           ​

                                                             UOS内存价格曲线.png

### 2.3.2 cpu和net <a id="017a5f8c"></a>

在考虑UOS资源的时候,要意识到UOS的节点是运行在真实计算机上的一个进程，并且每台这样的计算机运行的程序都是一致的，故UOS的资源并不因为节点的增加而增加。一个交易传输到UOS的出块节点，该节点会先执行这个交易，然后打包进区块。因此，UOS的cpu描述的是交易执行所需要的时间，net描述的是交易本身所占容量的大小。cpu和net是可再生资源，用户使用后，无论多少，都会随时间慢慢恢复，可用的总量与自己的抵押有关。

baodaotulong用户抵押了0.1000 UOS的cpu，已使用了 2.939ms，可用 2.122s，意味着baodaotulong在接下来的24小时内，如果发起交易，则这些交易的执行时间之和不能大于2.122s。

baodaotulong用户抵押了0.1000 UOS的net，已使用了432bytes，可用15.63MB，意味着baodaotulong在接下来的24小时内，如果发起交易，则这些交易的容量之和不能大于15.63MB。

我们可用用区块链浏览器查看交易消耗的cpu和net情况：  
[https://explorer.uosio.org/transactions/5513440/366616970c7995a935e3263571bd543937a6512cc358c66a91ed449fbb0dcd21](https://explorer.uosio.org/transactions/5513440/366616970c7995a935e3263571bd543937a6512cc358c66a91ed449fbb0dcd21)

> "cpu\_usage\_us": 609
>
> "net\_usage\_words": 16

cpu和net资源的平均使用量，是一种改进的指数移动平均模型：

​                  ![image.png](https://cdn.nlark.com/yuque/358/2019/png/127101/1554715003701-6f12048e-46fb-4950-b89b-6a8024b47aa4.png)                                           ​ 

其中:

VX: 资源的平均使用量；

VX’: 上次资源的平均使用量；

unit: 本次交易消耗的资源量；

Precision:精度补偿系数，防止unit过小出现整除为零；

Windowsize:窗口大小；

Futuredistance:上个更新时刻为周期起点，当前时刻离周期终点的距离。

当每个块都有更新时，Futuredistance+1 = Windowsize，上试退化成普通的指数平均模型。

当前的cpu更新使用量：

​                        ![image.png](https://cdn.nlark.com/yuque/358/2019/png/127101/1554715224683-20a1980f-0cee-4e07-a28f-0c3579862f9e.png)                                           ​

对某个用户而言，Windowsize的大小是24小时的区块数量。

而用户cpu和net资源的可用量与自身抵押和UOS的繁忙程度有关，UOS用虚拟资源的概念表述。UOS上的交易越多，UOS越繁忙，UOS的虚拟资源越少，用户可使用的资源就越少，用户抵押的越多，可使用的资源就越多。实际上，UOS区块的cpu和net的更新使用量计算公式也是依照以上两个公式，不过unit为一个区块总的cpu和net使用量，Windowsize为60。

下面的命令是查询UOS系统的全局参数，以cpu为例，当区块的cpu当前更新使用量小于target\_block\_cpu\_usage\_pct\*max\_block\_cpu\_usage，UOS会判定系统处于闲暇，抬升UOS的虚拟cpu大小。反之亦然，虚拟cpu的取值区间为\[max\_block\_cpu\_usage，max\_block\_cpu\_usage\*1000\]。

> cluos -u https://rpc3.uosio.org:8080 get table uosio uosio global
>
> {
>
>  "rows": \[{
>
>      "max\_block\_net\_usage": 2097152,
>
>      "target\_block\_net\_usage\_pct": 1000,
>
>      "max\_transaction\_net\_usage": 524288,
>
>      "base\_per\_transaction\_net\_usage": 12,
>
>      "net\_usage\_leeway": 500,
>
>      "context\_free\_discount\_net\_usage\_num": 20,
>
>      "context\_free\_discount\_net\_usage\_den": 100,
>
>      "max\_block\_cpu\_usage": 450000,
>
>      "target\_block\_cpu\_usage\_pct": 1000,
>
>      "max\_transaction\_cpu\_usage": 150000,
>
>      "min\_transaction\_cpu\_usage": 100,
>
>      "max\_transaction\_lifetime": 3600,
>
>      "deferred\_trx\_expiration\_window": 600,
>
>      "max\_transaction\_delay": 3888000,
>
>      "max\_inline\_action\_size": 4096,
>
>      "max\_inline\_action\_depth": 4,
>
>      "max\_authority\_depth": 6,
>
>      "max\_ram\_size": "8589934592",
>
>      "total\_ram\_bytes\_reserved": "4578540092",
>
>      "total\_ram\_stake": "57069206716",
>
>      "last\_producer\_schedule\_update": "2019-04-08T10:21:10.000",
>
>      "last\_pervote\_bucket\_fill": "1554716648000000",
>
>      "pervote\_bucket": 78924164,
>
>      "perblock\_bucket": 221709552,
>
>      "total\_unpaid\_blocks": 287821,
>
>      "total\_activated\_stake": "31137840000",
>
>      "thresh\_activated\_stake\_time": "1548746649000000",
>
>      "last\_producer\_schedule\_size": 13,
>
>      "total\_producer\_vote\_weight": "30137840000.00000000000000000",
>
>      "last\_name\_close": "2019-04-07T11:43:02.000"
>
>    }
>
>  \],
>
>  "more": false
>
> }

## 2.4 多重签名 <a id="6d400d4e"></a>

UOS的多重签名有两种方式：一种是手动多重签名，一种是利用uosio.msig合约进行多重签名。

### 2.4.1 手动多重签名 <a id="f8d58fc5"></a>

设置baodaotulong的账户的active权限为多公钥控制：

> cluos -u https://rpc3.uosio.org:8080  set account permission baodaotulong active '{"threshold":2,"keys":\[{"key":"UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y","weight":1},{"key":"UOS7piQ2XhQi5DdQmBfny6QvQqCJfaph5GYRqYcEozxA6b3Fq36om","weight":1}\],"accounts":\[\]}' -p baodaotulong@active

baodaotulong账户的组成如下：

> cluos -u http://rpc.uos.iccob.com:9008 get account baodaotulong
>
> permissions:
>
>     owner     1:    1 UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y
>
>        active     2:    1 UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y, 1 UOS7piQ2XhQi5DdQmBfny6QvQqCJfaph5GYRqYcEozxA6b3Fq36om

发起一条转账交易,不广播，不签名\(**构造一个没有签名的交易json**\)：

> cluos -u https://rpc3.uosio.org:8080 transfer -d -s -j -x 3600 baodaotulong aaaaaaaaaaaa "0.0010 UOS" &gt;tr.json

tr.json如下：

> {
>
>  "expiration": "2019-04-09T02:28:42",
>
>  "ref\_block\_num": 35586,
>
>  "ref\_block\_prefix": 3431691675,
>
>  "max\_net\_usage\_words": 0,
>
>  "max\_cpu\_usage\_ms": 0,
>
>  "delay\_sec": 0,
>
>  "context\_free\_actions": \[\],
>
>  "actions": \[{
>
>      "account": "uosio.token",
>
>      "name": "transfer",
>
>      "authorization": \[{  "actor": "baodaotulong",
>
>          "permission": "active"
>
>        }
>
>      \],
>
>      "data": "c0268d3a5393a839608c31c618638c310a0000000000000004554f530000000000"
>
>    }
>
>  \],
>
>  "transaction\_extensions": \[\],
>
>  "signatures": \[\],
>
>  "context\_free\_data": \[\]
>
> }

然后分别用avtive权限的两个私钥签名：

> cluos -u http://rpc.uos.iccob.com:9008 sign tr.json

分别复制签名字段：

> "signatures": \[
>
>  "SIG\_K1\_K2KC8742bKq6YrDnHUqhHVerU6NRDq26SAJGMSSnn5bWZzLKMma8XtNwFVjMBKYTcrnStnfiQr5NZuPuA1FDLntK53vzJF"
>
>  \]
>
> .......
>
> "signatures": \[
>
>  "SIG\_K1\_K8REbV6MX2uebfx5whW4dVeXsGBj9ejdifmkBtKzpkURMifSNXwEwz6RXP8vjBpxquMt8BWTK6Xu4ZkqLMqzk4SK5JkmXQ"
>
>  \]

将tr.json的signatures字段用上面两个签名填充：

> "signatures": \[   "SIG\_K1\_K2KC8742bKq6YrDnHUqhHVerU6NRDq26SAJGMSSnn5bWZzLKMma8XtNwFVjMBKYTcrnStnfiQr5NZuPuA1FDLntK53vzJF", "SIG\_K1\_K8REbV6MX2uebfx5whW4dVeXsGBj9ejdifmkBtKzpkURMifSNXwEwz6RXP8vjBpxquMt8BWTK6Xu4ZkqLMqzk4SK5JkmXQ"
>
>  \]

最后将交易发送到rpc节点：

> cluos -u http://rpc.uos.iccob.com:9008 push transaction -s tr.json

会返回一个json，包含交易id:

> 41454b52b491f22468abbe8967df570a4961b63452cdb20743e65885b6bca7c0

浏览器查询：[https://explorer.uosio.org/transactions/5999587/41454b52b491f22468abbe8967df570a4961b63452cdb20743e65885b6bca7c0](https://explorer.uosio.org/transactions/5999587/41454b52b491f22468abbe8967df570a4961b63452cdb20743e65885b6bca7c0)

### 2.4.2 调用uosio.msig合约的多重签名 <a id="d43843bc"></a>

使用uosio.misg合约合约同样达到多重签名的目的，但是多重签名合约只面向权限组成为账户的情况，手动多重签名则没有这个限制：

更改账户的额权限，便于测试：

> cluos -u https://rpc3.uosio.org:8080  set account permission baodaotulong active '{"threshold":2,"keys":\[\],"accounts":\[{"permission":{"actor":"bithumbshiny","permission":"active"},"weight":1},{"permission":{"actor":"testertester","permission":"active"},,"weight":1}\]}' -p baodaotulong@active

查看权限：

> cluos -u https://rpc3.uosio.org:8080 get account baodaotulongcreated: 2019-03-29T08:46:56.000
>
> permissions:
>
>     owner     1:    1 UOS5A4sYXU6SA5kyKjbu76r9xapgrzotjhLEgPSZzDzMBJpw1559Y
>
>        active     2:    1 bithumbshiny@active, 1 testertester@active

baodaotulong的active权限阈值为２，需要bithumbshiny的active权限以及testertester的active权限才能解锁。

发起一个提案：baodaotulong往aaaaaaaaaaaa转0.001个UOS。

> cluos -u  https://rpc3.uosio.org:8080 multisig propose transf   '\[{"actor": "bithumbshiny", "permission": "active"},{"actor": "testertester", "permission": "active"}\]' '\[{"actor": "baodaotulong", "permission": "active"}\]' uosio.token transfer '{"from":"baodaotulong","to": "aaaaaaaaaaaa", "quantity": "0.0010 UOS", "memo": ""}' bithumbshiny -p bithumbshiny
>
> 交易id:35fb04bc93d0e9b3a33e5bb5c94f2cf9e2d2cd9ea7500e7cf8cc99b254bd0d89

查看提案：

> //查看提案hex编码
>
> cluos -u https://rpc3.uosio.org:8080 get table uosio.msig bithumbshiny proposal
>
> //查看提案赞同信息
>
> cluos -u https://rpc3.uosio.org:8080  get table uosio.msig bithumbshiny approvals2

赞同提案，需要两个权限均赞同：

> cluos -u https://rpc3.uosio.org:8080 multisig approve bithumbshiny transf '{"actor": "bithumbshiny", "permission": "active"}' -p bithumbshiny

> cluos -u https://rpc3.uosio.org:8080 multisig approve testertester transf '{"actor": "testertester", "permission": "active"}' -p testertester

执行提案：

> cluos -u https://rpc3.uosio.org:8080 multisig exec bithumbshiny transf -p bithumbshiny
>
> 交易id:14255495bcafc1d0e6977b47234a600eba8cbe3a2e5df8f12ad84b8a0d750333

以上是对某一个合约执行发起提案，我们也可以对具体某一个交易发起提案，这要求我们先构造好一个交易json\(见手动多重签名\)：

> cluos multisig propose\_trx \[OPTIONS\] proposal\_name requested\_permissions transaction \[proposer\]
>
> Positionals:
>
>  proposal\_name TEXT          proposal name \(string\) \(required\)
>
>  requested\_permissions TEXT  The JSON string or filename defining requested permissions \(required\)
>
>  transaction TEXT            The JSON string or filename defining the transaction to push \(required\)
>
>  proposer TEXT               Account proposing the transaction

共识主节点是UOS的出块节点和备用出块节点的统称，他们不仅仅提供UOS的出块以及验证，而且可以给其他ulord侧链提供共识服务，普通账户如果自己给自己抵押的cpu和net之和大于100000UOS,则可以申请成为活跃的出块用户，得票最多的前21位用户将有资格出块。将某个"活跃的出块用户"以及他在计算机上运行的UOS进程合称为一个共识主节点。

申请成为共识主节点：

> cluos system regproducer \[OPTIONS\] account producer\_key \[url\] \[ulord\_addr\] \[location\]
>
> Positionals:
>
>  account TEXT                The account to register as a producer \(required\)
>
>  producer\_key TEXT           The producer's public key \(required\)
>
>  url TEXT                    url where info about producer can be found
>
>  ulord\_addr TEXT             the ulord address  associative with candidate
>
>  location UINT=0             relative location for purpose of nearest neighbor scheduling

producer\_key是出块公钥，用来打包块的时候签名，他的私钥不需要存放在钱包里。

共识主节点列表的查看命令为：

> cluos -u http://rpc.uos.iccob.com:9008 get table uosio uosio producers

共识主节点的状态如下，需要uosio权限即是需要主节点发起提案，经由2/3投票通过。​

| BP状态 | 需要权限 | 可否撤回抵押 | 可否出块 | 可否提奖 | 可否更改为其他状态 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| active | bp自己 | 否 | 可 | 可 | 可 |
| release | uosio | 可 | 否 | 否 | 可 |
| remove | uosio | 抵押扣除 | 否 | 否 | 否 |

​

共识节点的奖池有三个：uosio.bvpay、uosio.bpay 、uosio.vpay

他们之间的关系是，uosio.bvpay是奖池源头，uosio.bpay是出块奖池，uosio.vpay是投票奖池。

​                                          ![](https://cdn.nlark.com/yuque/358/2019/png/127101/1554776846545-16377014-3c42-4db1-a48c-c663a4de4131.png)                                           ​

                                                                        bvpay.jpg

uosio.bvpay奖池源奖金的0.8将会进入出块奖池，奖池源的0.2将会进入投票奖池。

查看总奖池：

> cluos --url http://rpc.uos.iccob.com:9008 get table uosio.bvpay uosio.bvpay bpvtpay
>
> {
>
>  "rows": \[{
>
>      "id": 0,
>
>      "start\_line": 1554185715,
>
>      "laster\_line": 1554340049,
>
>      "dead\_line": 1556777715,
>
>      "inherit": "0.0000 UOS",
>
>      "quantity": "244518.9665 UOS"
>
>    },{
>
>      "id": 1,
>
>      "start\_line": 1556777715,
>
>      "laster\_line": 1556777715,
>
>      "dead\_line": 1559369715,
>
>      "inherit": "0.0000 UOS",
>
>      "quantity": "0.0000 UOS"
>
>    }
>
>  \],
>
>  "more": false
>
> }

id = 0代表当前奖励周期，id = 1代表下一次奖励周期，周期长度为30天。

假如现在的时间戳在周期０内，那么bp发起一次领奖，能领取多少钱呢？

假设为领取时刻为ｔ,则奖池源发放的奖金为：

> out\_all = \(t - laster\_line\_0\) \* quantity\_0 \* 10000 / \(dead\_line\_0 - laster\_line\_0\);   \(2-1\)

奖金会分为两部分流入uosio.bpay与uosio.vpay。其中，0.8\*out\_all 进入uosio.bpay,0.2进入uosio.vpay。

查看系统合约global参数：

> cluos --url http://rpc.uos.iccob.com:9008 get table uosio uosio global
>
> {
>
>  "rows": \[{
>
>      "max\_block\_net\_usage": 2097152,
>
>      "target\_block\_net\_usage\_pct": 1000,
>
>      "max\_transaction\_net\_usage": 524288,
>
>      "base\_per\_transaction\_net\_usage": 12,
>
>      "net\_usage\_leeway": 500,
>
>      "context\_free\_discount\_net\_usage\_num": 20,
>
>      "context\_free\_discount\_net\_usage\_den": 100,
>
>      "max\_block\_cpu\_usage": 450000,
>
>      "target\_block\_cpu\_usage\_pct": 1000,
>
>      "max\_transaction\_cpu\_usage": 150000,
>
>      "min\_transaction\_cpu\_usage": 100,
>
>      "max\_transaction\_lifetime": 3600,
>
>      "deferred\_trx\_expiration\_window": 600,
>
>      "max\_transaction\_delay": 3888000,
>
>      "max\_inline\_action\_size": 4096,
>
>      "max\_inline\_action\_depth": 4,
>
>      "max\_authority\_depth": 6,
>
>      "max\_ram\_size": "8589934592",
>
>      "total\_ram\_bytes\_reserved": "4607367427",
>
>      "total\_ram\_stake": "57844214019",
>
>      "last\_producer\_schedule\_update": "2019-04-04T05:46:45.000",
>
>      "last\_pervote\_bucket\_fill": "1554340049000000",
>
>      "pervote\_bucket": 27244585,
>
>      "perblock\_bucket": 100966841,
>
>      "total\_unpaid\_blocks": 183296,
>
>      "total\_activated\_stake": "29178000000",
>
>      "thresh\_activated\_stake\_time": "1548746649000000",
>
>      "last\_producer\_schedule\_size": 12,
>
>      "total\_producer\_vote\_weight": "28178000000.00000000000000000",
>
>      "last\_name\_close": "2019-04-03T10:10:13.000"
>
>    }
>
>  \],
>
>  "more": false
>
> }

里面有4个参数与奖励有关

> "pervote\_bucket": 27244585　　－－－　当前投票奖励资金池
>
> "perblock\_bucket": 100966841　－－－　当前块奖励资金池
>
> "total\_unpaid\_blocks": 183296　－－－　当前未结算的区块数
>
> "total\_producer\_vote\_weight": "28178000000.00000000000000000"　－－－　当前投票总量

下面查看bp的具体情况，以“uoskkkkonebp”为例:

> cluos --url http://rpc.uos.iccob.com:9008 get table -l 1 -L uoskkkkonebp --index 1 uosio uosio producers
>
> {
>
>  "rows": \[{
>
>      "owner": "uoskkkkonebp",
>
>      "total\_votes": "1000000000.00000000000000000",
>
>      "producer\_key": "UOS8VjUQXSovjJ5LuubcAUK9r5LYda7j5dLTJcfrBHm6VSP2L3d8Y",
>
>      "is\_active": 1,
>
>      "is\_remove": 0,
>
>      "release": 0,
>
>      "url": "https://uoskkkkonebp.com/chain",
>
>      "unpaid\_blocks": 7577,
>
>      "last\_claim\_time": "1554259259000000",
>
>      "location": 0,
>
>      "ulord\_addr": "UWKPFY3dVd1z5zHancEFLy2XYpawbZWHNn"
>
>    }
>
>  \],
>
>  "more": true
>
> }

uoskkkkonebp的获得票数为1000000000.00000000000000000，未结算的生产区块数量为7385，最后一次领取奖励的时间戳为：1554259259000000

那么如果ｔ时刻\(t &gt; 1554259259000000 + 3600\)

uoskkkkonebp能领取出块的奖励为m,则:

> m =  \(out\_all\*0.8 + perblock\_bucket\)\*unpaid\_blocks / total\_unpaid\_blocks   \(2-2\)

uoskkkkonebp能领取的投票奖励为n,则:

> n= \(out\_all\*0.2 + pervote\_bucket\)\*total\_votes / total\_producer\_vote\_weight  \(2-3\)
>
> 注：当n &gt;= 100 0000时，即投票奖励大于100UOS,n才有效,否则不会发放投票奖励

uoskkkkonebp能够领取的总奖励为：m+n ,由\(2-1\)、\(2-2\)、\(2-3\)式可算出，注意最后得到的单位是“分”，要转化为"元",需要除以10000.

