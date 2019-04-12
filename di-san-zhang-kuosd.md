# 第三章 kuosd

## 概述

### UOS钱包与UOS账号

UOS钱包，是专门负责保管钥匙的软件，就像你为了安全起见，把家里的保险柜钥匙放在一个专用的钱包里一样。

UOS账号，代表你在UOS网络里的身份，相当于你在某个银行的卡号，你的朋友要转钱给你，就只认这个卡号。你在UOS网络上转币、抵押、领糖果等，都用这个账号进行。

UOS账号与UOS钱包的关系是，UOS账号进行任何操作，都需要UOS钱包里存放的钥匙来授权，不然这个账号就干不了活。

UOS账号名，由12位字符组成，包含数字1-5和小写字母a-z，这是不需要花钱购买的。但是，你想要简短一点的、个性化的名字，比如“money”，就需要花UOS购买了。

### Owner Key和Active Key

UOS的账户，通常有两种权限的密钥，即：Owner Key、Active Key。

Owner Key和Active Key之间的关系，可以看作是领导和下属的关系。平时，都是员工Active Key先生来处理日常事务，比如转账、投票等，而领导Owner Key很少露面。只有当Active Key搞不定时，他才会出来解决问题，比如，Active Key忘记了自己的密钥，领导Owner Key就可以帮他重新设置。

专业一点的解释是：Owner Key的权限表示所有权，它不但具有Active Key的所有权限，还有重置Owner Key、Active Key等权限，要注意离线妥善保存。

### kuosd介绍

kuosd 是一个轻客户端钱包，负责管理钱包，以便在广播到网络之前保护密钥和签署交易，在本地计算机上运行并在本地存储您的公私钥。

首先运行一次kuosd，然后停掉kuosd，就会自动建立目录和文件。

找到文件并打开~/uosio-wallet/config.ini。

将第一行的服务地址设置为：http-server-address = 127.0.0.1:1677

这时配置的钱包的端口是1677 ，以后用命令行连接钱包时使用的url配置为 --wallet-url http://127.0.0.1:1677

## 钱包常用RPC

启动kuosd

可以直接输入命令kuosd即可启动kuosd。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
kuosd
```
{% endcode-tabs-item %}
{% endcode-tabs %}

停止kuosd

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
pkill  kuosd
```
{% endcode-tabs-item %}
{% endcode-tabs %}

使用cluos创建钱包

下面我们使用命令行进行操作，需要使用到cluos，它是一个命令行工具，可以与noduos公开的REST API进行交互，前提是noduos需要配置uosio :: chain\_api\_plugin插件。

### **创建钱包**

钱包是授权私钥的存储库，与区块链交互所必需的。通过如下命令创建钱包，可以通过-n参数指定创建的钱包名称。如果不指定钱包名称，则会创建一个名为default的默认钱包。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet create  --to-console
```
{% endcode-tabs-item %}
{% endcode-tabs %}

创建指定名称的钱包

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos  wallet create --to-console -n  ttwallet
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回如下：（注意备份钱包秘钥）

```bash
Creating wallet: ttwallet
Save password to use in the future to unlock this wallet.
Without password imported keys will not be retrievable.
"PW5J6e5Qg4S9C31z15KZf3KQUbmugY9H4fRgBPNGzaub7NNBCwrEd"
```

成功后将会生成钱包的解锁密码，钱包15分钟不用将会自动锁定，注意一定要保存最后一行的密码，需要使用它解锁这个钱包。

### **解锁钱包**

可以通过两种方式解锁钱包，推荐使用第二种方式解锁。

直接在命令行上添加密码参数，可以通过-n参数指定钱包名称

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet unlock -n ttwallet --password PW5J6e5Qg4S9C31z15KZf3KQUbmugY9H4fRgBPNGzaub7NNBCwrEd
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在交互模式下解锁

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos wallet unlock -n ttwallet
```
{% endcode-tabs-item %}
{% endcode-tabs %}

接着提示password:，然后输入密码即可

```bash
password:
```

两种方式解锁成功后都会输出Unlocked: default。

注意：直接在命令行上使用您的密码并将其记录到您的bash历史记录中通常是不安全的，因此最好在交互模式下解锁。出于安全考虑，最好在不使用钱包时使用命令cluos wallet lock将钱包锁定。

### **查看钱包列表**

使用命令cluos wallet lock查看钱包列表，在列表中解锁后的钱包将在钱包名后面有\*标记。

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos  wallet  list
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回如下：

```bash
Wallets:
[
  "ttwallet *"
]
```

### **导入密钥到钱包**

上述步骤中启动的私有区块链是使用默认初始密钥创建的，必须将其加载到钱包中。

使用命令cluos wallet import --private-key \*\*\*\*\*\*。可以通过-n参数指定钱包名称

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
 cluos wallet import -n ttwallet  --private-key 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3 –n ttwallet
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回提示导入成功：

```bash
imported private key for: UOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
```

注意：此时导入的私钥并不是创建钱包时生成的密码。

### **在钱包内部创建密钥**

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos  wallet  create_key  -n  ttwallet
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### **使用钱包，比如在钱包里面创建账户**

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
cluos  -u http://10.186.11.227:9008 create account uosio uosio.bpay  UOS8NaMwb1nSJJfy2KvBHwUJ27qWqeT65hP7MpApvZNezzTQ1dFxE
```
{% endcode-tabs-item %}
{% endcode-tabs %}

注意 [http://10.186.11.227:9008](http://10.186.11.227:9008) 为你自己noduos 启动时配置的节点。

## kuosd常用api

### create

> 创建具有给定名称的新钱包。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/create
>
> 参数：
>
>     string类型

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/create --data '"default"'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### open

> 打开给定名称的现有钱包。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/open
>
> 参数：
>
>     string类型

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/open --data '"default"'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### lock

> 锁定给定名称的现有钱包。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/lock
>
> 参数：
>
>     string类型

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/lock --data '"default"'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### lock\_all

> 锁定所有现有钱包。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/lock\_all
>
> 参数：
>
>     无

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/lock_all
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### unlock

> 使用给定的名称和密码解锁钱包。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/unlock
>
> 参数：
>
>     string类型，钱包名
>
>     string类型，钱包秘钥

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/unlock --data '["default","PW5HuyuuNftajiTPPvABkXZmRG2AyrFpx3W3cgMmeBmZYXK6Q5KEQ"]'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### import\_key

> 将私钥导入给定名称的钱包。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/import\_key
>
> 参数：
>
>     string类型，钱包名
>
>     string类型，私钥

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/import_key --data '["default","5J5T3cfraaYYnHTRpKdEwu23SwFLiyNykAXK4FZnMxvQRE5eqah"]'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### list\_wallets

> 列出所有钱包。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/list\_wallets 
>
> 参数：
>
>     无

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/list_wallets
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_public\_keys

> 列出所有钱包中的所有公钥。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/get\_public\_keys
>
> 参数：
>
>     无

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/get_public_keys
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### set\_timeout

> 设置钱包自动锁定超时（以秒为单位）。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/set\_timeout
>
> 参数：
>
>     int64类型

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/set_timeout --data 2000
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### sign\_transaction

> 签署事务。
>
> URL：https://rpc1.uosio.org:8250/v1/wallet/sign\_transaction
>
> 参数：
>
>     txn : string类型，事务
>
>     signatures : string类型，签名
>
>     id : string类型，链的ID

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/wallet/sign_transaction --data '[{"ref_block_num":919376,"ref_block_prefix":1884381553,"expiration":"2018-12-28T04:38:28.000","actions":[{"account":"uosio","name":"delegatebw","authorization":[{"actor":"tobetioadmax","permission":"active"}],"data":"d08d4c86baac0ecd20f2d4142193b1ca40420f000000000004554f530000000040420f000000000004554f530000000000"}],"signatures":[]},["UOS7kczHUPM1R4WPgzazYXSm9LPbKygngeFYxqQjVMQB9TojXdJqL"],"24c2ffcaeef8f222cf658e7d66dd6df9b25da829c006bff00e4c33c3d02df1c5"]'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

