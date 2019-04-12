# 第五章 cluos

## cluos简介

cluos是一个命令行工具，它可以调用noduos公开的API接口。使用cluos，您需要有一个noduos的节点（IP地址和端口号），并配置noduos加载插件'uosio::chain\_api\_plugin'。cluos包含其所有命令的文档。查看所有cluos相关命令的列表，只需运行它不带参数：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回如下：

```text
Command Line Interface to UOSIO Client
Usage: ./programs/cluos/cluos [OPTIONS] SUBCOMMAND

Options:
  -h,--help                   Print this help message and exit
  -u,--url TEXT=http://localhost:8888/
                              the http/https URL where noduos is running
  --wallet-url TEXT=http://localhost:8900/
                              the http/https URL where kuosd is running
  -r,--header                 pass specific HTTP header; repeat this option to pass multiple headers
  -n,--no-verify              don't verify peer certificate when using HTTPS
  -v,--verbose                output verbose actions on error

Subcommands:
  version                     Retrieve version information
  create                      Create various items, on and off the blockchain
  get                         Retrieve various items and information from the blockchain
  set                         Set or update blockchain state
  transfer                    Transfer UOS from account to account
  net                         Interact with local p2p network connections
  wallet                      Interact with local wallet
  sign                        Sign a transaction
  push                        Push arbitrary transactions to the blockchain
  multisig                    Multisig contract commands
  system                     Send uosio.system contract action to the blockchain.
```

要获得有关任何特定子命令的帮助，请不带任何参数运行其子命令：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos create
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回如下：

```text
Create various items, on and off the blockchain
Usage: ./cluos create SUBCOMMAND

Subcommands:
  key                         Create a new keypair and print the public and private keys
  account                     Create an account, buy ram, stake for bandwidth for the account
```

继续查看下一级子目录的帮助：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos create account
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回如下：

```text
Create an account, buy ram, stake for bandwidth for the account
Usage: ./programs/cluos/cluos create account [OPTIONS] creator name OwnerKey [ActiveKey]

Positionals:
  creator TEXT                The name of the account creating the new account (required)
  name TEXT                   The name of the new account (required)
  OwnerKey TEXT               The owner public key for the new account (required)
  ActiveKey TEXT              The active public key for the new account

Options:
  -h,--help                   Print this help message and exit
  -x,--expiration             set the time in seconds before a transaction expires, defaults to 30s
  -f,--force-unique           force the transaction to be unique. this will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times
  -s,--skip-sign              Specify if unlocked wallet keys should be used to sign transaction
  -j,--json                   print result as json
  -d,--dont-broadcast         don't broadcast transaction to the network (just print to stdout)
  -r,--ref-block TEXT         set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake)
  -p,--permission TEXT ...    An account and permission level to authorize, as in 'account@permission'
  --max-cpu-usage-ms UINT     set an upper limit on the milliseconds of cpu usage budget, for the execution of the transaction (defaults to 0 which means no limit)
  --max-net-usage UINT        set an upper limit on the net usage budget, in bytes, for the transaction (defaults to 0 which means no limit)
```

## 设置uosio.code权限

uosio.code权限：

在编写合约时，您可能要求您的合约能够广播内联action。要使您的合约具备此能力，您需要使用合约的active权限。但是，出于安全目的，合约不能与其主动权限签署，除非合约的帐户已配置为执行此操作。uosio.code是授予合约active权限的伪权威。以前，需要一个复杂且具有潜在风险的cluos命令来添加YOURCONTRACT@uosio.code到active权限，使用uosio.code帮助程序可以大大简化这一点。

将uosio.code添加到合约的active权限中

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos set account permission YOURCONTRACT active --add-code
```
{% endcode-tabs-item %}
{% endcode-tabs %}

从合约的活动权限中删除uosio.code

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos set account permission YOURCONTRACT active --remove-code
```
{% endcode-tabs-item %}
{% endcode-tabs %}

--add-code和--remove-code在后台做了什么？

使用--add-code和--remove-code时，cluos获取帐户的当前权限，并对active权限附加或移除YOURCONTRACT@uosio.code权限。建议使用--add-code选项，而不是以下命令，因为很容易出错，可能导致帐户被锁定。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos set account permission YOUR_CONTRACT active '{"threshold": 1,"keys": [{"key": "CURRENT_PUBLIC_KEY(S)_IN_ACTIVE_PERM","weight": 1}], "accounts": [{"permission":{"actor":"YOUR_CONTRACT","permission":"uosio.code"},"weight":1}]}' -p YOUR_CONTRACT@owner
```
{% endcode-tabs-item %}
{% endcode-tabs %}

当然，其他账户也可以附加或移除本合约的uosio.code权限，这种情况下则不能使用--add-code或--remove-code选项。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos set account permission OTHER_ACCOUNT active '{"threshold": 1,"keys": [{"key": "CURRENT_PUBLIC_KEY(S)_IN_ACTIVE_PERM","weight": 1}], "accounts": [{"permission":{"actor":"YOUR_CONTRACT","permission":"uosio.code"},"weight":1}]}' -p OTHER_ACCOUNT@owner
```
{% endcode-tabs-item %}
{% endcode-tabs %}



##  cluos RPC命令

### 1 version

查看cluos的版本信息。

子命令：

    client                      检索客户端的版本信息

#### 1.1 version client

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos version client
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 2 convert

事务或action格式转换。

子命令：

    pack\_transaction            封装json格式的事务

    unpack\_transaction          解封json格式的事务

    pack\_action\_data            封装json格式的action

    unpack\_action\_data          解封json格式的action

#### 2.1 pack\_transaction

将json格式的事务转换为封装（json）格式。

参数:

    transaction TEXT            string类型，必填，json格式的已签名事务

#### 2.2 unpack\_transaction

将json格式的封装事务还原为未封装格式。

参数:

    transaction TEXT            string类型，必填，已封装的json格式事务（包含封装好的事务和压缩选项）

#### 2.3 pack\_action\_data

将json格式的action转化为压缩格式。

参数:

    account TEXT                 string类型，必填，合约账户

    name TEXT                    string类型，必填，action名称

    unpacked\_action\_data TEXT    string类型，必填，json形式的action数据

#### 2.4 unpack\_action\_data

将压缩格式的action转化为json格式。

参数:

    account TEXT                string类型，必填，合约账户

    name TEXT                   string类型，必填，action名称

    packed\_action\_data TEXT     hex string类型，必填，封装的action数据

### 3 create

创建各种名目，无论是否连接到链上。

子命令:

    key                    创建并打印新的公私钥对

    account                在区块链上新建一个账户\(假设系统合约没有限制RAM的使用\)

#### 3.1 create key

创建公私钥对。

参数:

    无

#### 3.2 create account

创建账户。

参数:

    creator TEXT                string类型，必填，创建者账户名

    name TEXT                   string类型，必填，新建账户名

    OwnerKey TEXT               string类型，必填，新账户owner权限的公钥

    ActiveKey TEXT              string类型，新账户active权限的公钥

### 4 get

从链上获取各种信息。

子命令:

    info                        获取链的实时信息

    block                       检索链上某个区块

    account                     检索链上某个账户

    code                        检索某个账户的合约和ABI

    abi                         检索某个账户的ABI

    table                       检索数据库表格的内容

    scope                       检索某个合约的scopes和tables列表

    currency                    检索相关的标准代币信息

    accounts                    检索与给定公钥相关联的所有账户

    servants                    检索服务于给定账户的所有账户

    transaction                 检索链上某个事务

    actions                     检索与给定账户相关的所有action

    schedule                    检索生产者一览表

    transaction\_id              获取给定事务对象的事务id

#### 4.1 get info

获取区块链实时信息。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos get info
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### 4.2 get block

获取区块信息。

参数:

    block TEXT                  string类型，必填，要检索的区块高度或区块id

#### 4.3 get account

参数:

    name TEXT                  string类型，必填，将检索的账户名

    core-symbol TEXT            string类型，选填，所要查询的代币，预设为链的核心代币

#### 4.4 get code

参数:

    name TEXT                   string类型，必填，将检索合约的账户名

#### 4.5 get abi

参数:

    name TEXT                   string类型，必填，将检索的账户名

#### 4.6 get table

参数:

    account TEXT                string类型，必填，表格拥有者账户名\(required\)

    scope TEXT                  string类型，必填，指定表的范围

    table TEXT                  string类型，必填，合约abi中指定的表格名称

#### 4.7 get scope

参数:

    contract TEXT               string类型，必填，合约名称

#### 4.8 get currency

查询账户余额与代币状态。

子命令:

    balance                     检索账户下指定代币的余额

    stats                       检索指定代币的状态信息

**4.8.1 get currency balance**

查询账户余额。

参数:

    contract TEXT               string类型，必填，该代币的合约

    account TEXT                string类型，必填，查询余额的账户

    symbol TEXT                 string类型，如果合约有多种代币，可指定代币名

**4.8.2 get currency stats**

查询代币状态。

参数:

    contract TEXT               string类型，必填，该代币的合约

    symbol TEXT                 string类型，必填，如果合约有多种代币，可指定代币名

#### 4.9 get accounts

查询与公钥关联的所有账户。

参数:

    public\_key TEXT             string类型，必填，检索的公钥

#### 4.10 get servants

返回可控账户信息。

参数:

    account TEXT                string类型，必填，控制的账户名

#### 4.11 get transaction

查询一个区块链事务。

参数:

    id TEXT                     string类型，必填，将检索的事务ID

#### 4.12 get actions

查询账户所有操作。

参数:

    account\_name TEXT           string类型，必填，将查询的账户名

    pos INT                     整型，起始位置

    offset INT                  整型，偏移量，正数返回\[pos,pos+offset\]，负数返回\[pos-offset,pos\)

#### 4.13 get schedule

获取生产者信息。

#### 4.14 get transaction\_id

通过交易内容获取交易ID。

参数:

    transaction TEXT            string类型，必填，事务的json字符串或包含事务的文件名。

### 5 set

设置或更新区块链状态。

子命令:

    code                        设置wast或wasm路径

    abi                         创建或更新abi

    contract                     创建或更新账户合约

    account                     设置权限

    action                       设置操作权限

#### 5.1 set code

设置wast或wasm路径。

参数:

    account TEXT                string类型，必填，部署合约代码的账户名

    code-file TEXT              包含WASM文件的完整路径

#### 5.2 set abi

创建或更新abi。

参数:

    account TEXT                string类型，必填，部署合约abi的账户名

    abi-file TEXT               包含ABI文件的完整路径

#### 5.3 set contract

创建或更新账户合约。

参数:

    account TEXT                string类型，必填，部署合约的账户

    contract-dir TEXT            包含wasm和abi的路径

    wasm-file TEXT              包含WASM文件的相对于contract-dir的路径

    abi-file TEXT                包含ABI文件的相对于contract-dir的路径

#### 5.4 set account

设置权限。

子命令:

    permission                  设置处理帐户权限的参数

**5.4.1 set account permission**

参数:

    account TEXT                string类型，必填，设置权限的账户

    permission TEXT             string类型，必填，设置或删除的权限名

    authority TEXT              string类型，必填，设置该权限的公钥或控制权限，json字符串或文件，NULL表示删除该权限

    parent TEXT                 父权限，默认为active权限

#### 5.5 set action

设置操作权限。

子命令:

    permission                  设置处理帐户权限的参数

**5.5.1 set action permission**

参数:

    account TEXT                string类型，必填，设置权限的账户

    code TEXT                   string类型，必填合约拥有者

    type TEXT                   string类型，必填，action名称

    requirement TEXT            string类型，必填，设置执行该action的权限名，NULL表示删除

### 6 transfer

转账。

参数:

    sender TEXT                 string类型，必填，发送账户

    recipient TEXT               string类型，必填，接收账户

    amount TEXT                string类型，必填，发送的代币数量

    memo TEXT                 备注

### 7 net

与p2p网络进行交互。

子命令:

    connect                     开始一个新的连接

    disconnect                  关闭一个已经存在的连接

    status                      一个已连接节点的状态

    peers                       所有连接节点状态

#### 7.1 net connect

点对点连接。

参数:

    host TEXT                   string类型，必填，主机名：端口形式

#### 7.2 net disconnect

断开点对点连接。

参数:

    host TEXT                   string类型，必填，主机名：端口形式

#### 7.3 net status

返回指定URL节点状态。

参数:

    host TEXT                   string类型，必填，主机名：端口形式

#### 7.4 net peers

返回所有连接节点。

参数:

    无

### 8 wallet

与钱包进行交互。

子命令:

    create                       创建新的钱包

    open                        打开钱包

    lock                         锁定钱包

    lock\_all                      锁定所有钱包

    unlock                       解锁钱包

    import                       向钱包导入私钥

    remove\_key                   从钱包移除公私钥对

    create\_key                    在钱包中创建公私钥对

    list                          列出已打开的钱包, \* 表示已解锁

    keys                         列出所有解锁钱包中的公钥

    private\_keys                  列出已解锁钱包中的私钥

    stop                         停止kuosd进程

#### 8.1 create

创建新的钱包。

#### 8.2 open

打开钱包。

#### 8.3 lock

锁定钱包。

#### 8.4 lock\_all

锁定所有钱包。

#### 8.5 unlock

解锁钱包。

#### 8.6 import

向钱包导入私钥。

#### 8.7 remove\_key

移除公私钥对。

参数:

    key TEXT                    string类型，必填，公钥

#### 8.8 create\_key

生成公私钥对。

参数:

    key\_type K1/R1              公钥类型\(K1/R1\)

#### 8.9 list

列出所有钱包。

#### 8.10 keys

列出钱包中所有公钥。

#### 8.11 private\_keys

列出钱包中所有私钥。

#### 8.12 stop

停止钱包进程。

### 9 sign

事务签名。

参数:

    transaction TEXT            string类型，必填，事务的json字符串，或定义事务的文件

### 10 push

推送事务上链。

子命令:

    action                      推送单操作事务

    transaction                  推送任意事务（单操作或多操作）

    transactions                 推送多个任意事务

#### 10.1 push action

推送单操作事务。

参数:

    account TEXT                string类型，必填，合约账户

    action TEXT                 string类型，必填，action的json字符串或文件

    data TEXT                   操作的参数

#### 10.2 push transaction

推送任意事务（单操作或多操作）。

参数:

    transaction TEXT            string类型，必填，事务的json字符串或文件

#### 10.3 push transactions

推送多个任意事务。

参数:

    transactions TEXT           string类型，必填，事务的json字符串或文件

### 11 multisig

提案相关操作。

子命令：

    propose                     发起单action提案

    propose\_trx                  发起事务（任意数量action）提案

    review                      提案预览

    approve                     支持提案

    unapprove                   取消对提案支持

    invalidate                    取消对所有的未执行提案的支持

    cancel                       取消提案

    exec                        执行提案

#### 11.1 multisig propose

发起提案（action）。

参数:

    proposal\_name TEXT          string类型，必填，提案名称

    requested\_permissions TEXT    string类型，必填，定义所需权限的json字符串或文件

    trx\_permissions TEXT         string类型，必填，定义事务所需权限的json字符串或文件

    contract TEXT                string类型，必填，合约名

    action TEXT                  string类型，必填，调用的action

    data TEXT                   string类型，必填，定义action的json字符串或文件

    proposer TEXT               string类型，提案发起者

    proposal\_expiration           提案过期时间

#### 11.2 multisig propose\_trx

发起提案（transaction）

参数:

    proposal\_name TEXT          string类型，必填，提案名称

    requested\_permissions TEXT   string类型，必填，定义所需权限的json字符串或文件

    transaction TEXT             string类型，必填，定义事务的json字符串或文件

    proposer TEXT               string类型，提案发起者

#### 11.3 multisig review

提案预览。

参数:

    proposer TEXT               string类型，必填，提案发起者

    proposal\_name TEXT          string类型，必填，提案名称

#### 11.4 multisig approve

提案投票。

参数:

    proposer TEXT               string类型，必填，提案发起者

    proposal\_name TEXT          string类型，必填，提案名称

    permissions TEXT            string类型，必填，定义投票权限的json字符串或文件

    proposal\_hash TEXT           提案哈希（即事务ID）

#### 11.5 multisig unapprove

取消投票。

参数:

    proposer TEXT               string类型，必填，提案发起者

    proposal\_name TEXT          string类型，必填，提案名称

    permissions TEXT             string类型，必填，定义权限的json字符串或文件

#### 11.6 multisig invalidate

撤回对之前所有该账户通过、但未被最终执行的提案的投票。

参数:

    invalidator TEXT            string类型，必填，取消投票的账户名

#### 11.7 multisig cancel

取消提案。

参数:

    proposer TEXT               string类型，必填，提案发起者

    proposal\_name TEXT          string类型，必填，提案名称

    canceler TEXT                string类型，取消投票人

#### 11.8 multisig exec

执行提案。

参数:

    proposer TEXT               string类型，必填，提案发起者

    proposal\_name TEXT          string类型，必填，提案名称

    executer TEXT               string类型，提案执行者

### 12 system

系统合约相关操作。

子命令:

    newaccount                  新建账户

    regproducer                 注册成为生产者

    unregprod                   取消注册生产者

    voteproducer                对生产者投票

    cancelvote                  取消对生产者投票

    listproducers                列出所有生产者

    delegatebw                  抵押带宽

    undelegatebw                赎回抵押资金

    listbw                       列出抵押资金

    bidname                     账户竞拍

    bidnameinfo                 获取竞拍信息

    buyram                      购买RAM

    sellram                     出售RAM

    claimrewards                领取生产者奖励

    canceldelay                 取消延迟交易

#### 12.1 system newaccount

新建账户。

参数:

    creator TEXT                string类型，必填，创建者

    name TEXT                   string类型，必填，将创建的新账户

    OwnerKey TEXT               string类型，必填，新账户的owner公钥

    ActiveKey TEXT              string类型，新账户的activer公钥

#### 12.2 system regproducer

注册成为生产者。

参数:

    account TEXT                string类型，必填，注册成为生产者的账户

    producer\_key TEXT           string类型，生产者公钥

    url TEXT                    生产者信息URL

    ulord\_addr TEXT             ulord地址

#### 12.3 system unregprod

取消注册生产者。

参数:

     account TEXT                string类型，必填，取消注册的账户名

#### 12.4 system voteproducer

对生产者投票。

子命令:

    prods                       对一个或多个生产者投票

    approve                     新增一个生产者到投票队列

    unapprove                   从投票队列中移除一个生产者

**12.4.1  system voteproducer prods**

对一个或多个生产者投票。

参数:

    voter TEXT                  string类型，必填，投票者

    quantity TEXT               string类型，必填，投票金额

    producers TEXT ...            string类型，一个或多个生产者

**12.4.2 system voteproducer approve**

新增一个生产者到投票队列。

参数:

    voter TEXT                  string类型，必填，投票者

    producer TEXT               string类型，必填，新增的生产者

**12.4.3 system voteproducer unapprove**

从投票队列中移除一个生产者。

参数:

      voter TEXT                  string类型，必填，投票者

  producer TEXT               string类型，必填，移除的生产者

#### 12.5 system cancelvote

取消投票。

参数:

    voter TEXT                  string类型，必填，投票者

    quantity TEXT               string类型，必填，取消票数

    producers TEXT ...          string类型，必填，生成者（一个或多个）

#### 12.6 system listproducers

列出所有生产者。

参数:

    无

#### 12.7 system delegatebw

抵押CPU和NET。

参数:

    from TEXT                   string类型，必填，抵押付费账户名

    receiver TEXT               string类型，必填，资源接收者

    stake\_net\_quantity TEXT     string类型，必填，抵押的NET带宽

    stake\_cpu\_quantity TEXT     string类型，必填，抵押的CPU带宽

#### 12.8 system undelegatebw

取消抵押。

参数:

    from TEXT                   string类型，必填，取消抵押的账户名

    receiver TEXT                string类型，必填，接收赎回资金的账户

    unstake\_net\_quantity TEXT     string类型，必填，取消抵押的网络带宽

    unstake\_cpu\_quantity TEXT     string类型，必填，取消抵押的CPU带宽

#### 12.9 system listbw

列出账户抵押金额。

参数:

    account TEXT                string类型，必填，查询的账户名

#### 12.10 system bidname

账户竞拍。

参数:

    bidder TEXT                  string类型，必填，竞拍者

    newname TEXT                string类型，必填，被竞拍账户

    bid TEXT                     string类型，必填，竞拍价格

#### 12.11 system bidnameinfo

列出账户的竞拍信息。

参数:

    newname TEXT                string类型，必填，被竞拍账户名

#### 12.12 system buyram

购买内存。

参数:

    payer TEXT                  string类型，必填，付费账户

    receiver TEXT               string类型，必填，接收账户

    amount TEXT                 string类型，必填，购买RAM的代币数量，或使用--bytes/--kbytes，以字节为单位衡量

#### 12.13 system sellram

出售内存。

参数:

    account TEXT                string类型，必填，接收代币的账户

    bytes UINT                  无符号整型，必填，卖出的RAM数量

#### 12.14 system claimrewards

BP领取奖励。

参数:

    owner TEXT                  string类型，领取奖励的账户名

#### 12.15 system canceldelay

取消延迟交易。

参数:

    canceling\_account TEXT      string类型，必填，原始事务的授权账户

    canceling\_permission TEXT   string类型，必填，原始事务的授权权限

    trx\_id TEXT                 string类型，必填，原始事务的ID

