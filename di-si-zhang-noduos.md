# 第四章 noduos

## **概述**

UOSIO附带了许多程序。您将使用的主要部分以及此处涉及的部分是：noduos、cluos、kuosd。

noduos（node + uos） - 核心UOSIO 节点守护程序，可以配置插件来运行节点。如：生产区块，专用API节点和本地开发等。

cluos （cli + uos） - 命令行界面，用于与区块链交互并管理钱包。

kuosd （key + uos） - 将UOSIO密钥安全存储在钱包中的组件。

### **运行noduos**

Noduos可以从命令行运行。行为noduos取决于使用的插件和每个插件的配置选项。Noduos本身有几个选项，这些选项允许您设置存储区块链数据的数据目录，并指向插件的配置文件和日志记录。

例如：

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
noduos -e -p uosio --plugin uosio::producer_plugin --plugin uosio::chain_api_plugin --plugin uosio::http_plugin --plugin uosio::state_history_plugin --data-dir /Users/mydir/uosio/data --config-dir /Users/mydir/uosio/config --access-control-allow-origin='*' --contracts-console --http-validate-host=false --state-history-dir /shpdata --trace-history --chain-state-history --verbose-http-errors --filter-on='*' --disable-replay-opts >> noduos.log 2>&1 &
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### **Noduos配置**

Noduos可以使用命令行界面（CLI）选项或配置文件进行配置config.ini。可以通过运行找到所有CLI选项$ noduos --help。

每个CLI选项都映射到一个config.ini的设置，例如--plugin uosio::chain\_api\_plugin可以通过添加plugin = uosio::chain\_api\_plugin来设置。

config.ini执行时可以使用自定义文件$ noduos –config-dir path/to/config.ini。

默认情况下，config.ini 可以在以下位置找到：

~/.local/share/uosio/noduos/config

### **Noduos选项**

运行$ **noduos –help**，输出如下所示，实际输出将包括插件选项，但为清楚起见已排除这些选项。

```text
Application Options:
Application Config Options:
  --plugin arg                          Plugin(s) to enable, may be specified
                                        multiple times
Application Command Line Options:
  -h [ --help ]                         Print this help message and exit.
  -v [ --version ]                      Print version information.
  --print-default-config                Print default configuration template
  -d [ --data-dir ] arg                 Directory containing program runtime
                                        data
  --config-dir arg                      Directory containing configuration
                                        files such as config.ini
  -c [ --config ] arg (=config.ini)     Configuration file name relative to
                                        config-dir
  -l [ --logconf ] arg (=logging.json)  Logging configuration file name/path
                                        for library users
Application Options:
Application Config Options:
  --plugin arg                          Plugin(s) to enable, may be specified
                                        multiple times
Application Command Line Options:
  -h [ --help ]                         Print this help message and exit.
  -v [ --version ]                      Print version information.
  --print-default-config                Print default configuration template
  -d [ --data-dir ] arg                 Directory containing program runtime
                                        data
  --config-dir arg                      Directory containing configuration
                                        files such as config.ini
  -c [ --config ] arg (=config.ini)     Configuration file name relative to
                                        config-dir
  -l [ --logconf ] arg (=logging.json)  Logging configuration file name/path
                                        for library users
```

插件选项

有关每个插件的详细信息，请参阅插件的文档：

```text
bnet_plugin
chain_plugin
history_plugin
http_client_plugin
http_plugin
login_plugin
mongo_db_plugin
net_plugin
producer_plugin
state_history_plugin
txn_test_gen_plugin
```

## noduos常用API

### chain

#### **get\_info**

返回包含区块链的各种详细信息的对象。

https：//rpc1.uosio.org:8250/v1/chain/get\_info

参数：

    无

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_info
get_block
```
{% endcode-tabs-item %}
{% endcode-tabs %}

返回一个对象，其中包含有关区块链上特定块的各种详细信息。

https：//rpc1.uosio.org:8250/v1/chain/get\_block

参数：

    block\_num\_or\_id : string类型，区块高度或区块ID

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250 /v1/chain/get_block --data '{"block_num_or_id":"10000"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_account**

返回一个对象，其中包含有关区块链上特定帐户的各种详细信息。

https：//rpc1.uosio.org:8250/v1/chain/get\_account

参数：

    account\_name ：string类型，账户名

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_account --data '{"account_name":"uosio"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_abi**

返回某个账户下的智能合约abi信息。

https：//rpc1.uosio.org:8250/v1/chain/get\_abi

参数：

    account\_name ：string类型，账户名

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_abi --data '{"account_name":"uosio"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_code**

返回一个对象，其中包含区块链上特定智能合约的各种详细信息。

https：//rpc1.uosio.org:8250/v1/chain/get\_code

参数：

    account\_name ：string类型，账户名

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_code --data '{"account_name":"uosio","code_as_wasm":true}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_raw\_code\_and\_abi**

返回一个对象，其中包含区块链上特定智能合约的各种详细信息及abi。

https：//rpc1.uosio.org:8250/v1/chain/get\_raw\_code\_and\_abi

参数：

    account\_name ：string类型，账户名

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_raw_code_and_abi --data '{"account_name":"uosio"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_table\_rows**

返回一个对象，该对象为指定表中的行。

https：//rpc1.uosio.org:8250/v1/chain/get\_table\_rows

参数：

    scope : string类型，必填，帐户名称

    code: string类型，必填，智能合约名称

    table : string类型，必填，表名称

    json : boolean类型，默认true

    lower\_bound : string类型，可选，下限

    upper\_bound : string类型，可选，上限

    limit : int32类型，可选，限制返回数据条数，默认10

    index\_position : string类型，要使用的索引序号，例如，主键索引为1或primary，次级索引为2或secondary，默认值：1

    key\_type : string类型，索引键类型，例如i64

    encode\_type : string类型，编码类型，dec或hex,默认值：dec

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_table_rows --data '{"scope":"uosio.token","code":"uosio.token","table":"accounts","json":"false","lower_bound":0,"upper_bound":100,"limit":10}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_currency\_balance**

获取指定账户下货币余额。

https：//rpc1.uosio.org:8250/v1/chain/get\_currency\_balance

参数：

    code : string类型，合约名称

    account\_name：string类型，所要查询的账户

    symbol ：string类型，可选，要查询的货币符号（不写则列出所有）

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_currency_balance --data '{"code":"uosio.token","account":"usera","smybol": "SYS"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_currency\_stats**

返回指定货币的状态信息。

https：//rpc1.uosio.org:8250/v1/chain/get\_currency\_stats

参数：

    code : string参数，合约名称

    symbol : string类型，统计数据的货币符号

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_currency_stats --data '{"code":"uosio.token","symbol":"SYS"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_producers**

返回生产节点信息。

https：//rpc1.uosio.org:8250/v1/chain/get\_producers

参数：

    limit : uint32类型，要检索的生产者总数

    lower\_bound : string类型，下限

    json : boolean类型，以JSON格式返回结果

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_producers --data '{"limit":20,"lower_bound":"usera","json":"false"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_required\_kes**

返回签名一个事务所需的公钥清单。

curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/ get\_required\_keys

参数：

    transaction : json类型，事务

    available\_keys : string数组，公钥集合

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/get_required_keys --data '{"transaction":{"ref_block_num":"8280640","ref_block_prefix": "2197313158","expiration": "2019-04-09T09:06:43.500","actions":[{"account":"eosio","name":"delegatebw","authorization": [{"actor":"testaccount1","permission":"active"}],"data": "10f2d4142193b1ca10f2d4142193b1ca102700000000000004535953000000001027000000000000045359530000000000"}],"signatures": [],"authorizations": []},"available_keys":["EOS72shafjy5hHqaESGRCA7XQRtdkz2aos58wqKJZ7S4uqVc1VvQ7","EOS8TnAwmHfdZvWYgbCBGYmLscwASkba9fALELpQSugqh9HUfHHkP","EOS5PVr4cGoXfaciZLpWLie4ozdy8fnoHVT57Ea3XxjpebZ9DLVyJ"]}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **abi\_json\_to\_bin**

将json格式内容转换成十六进制字符串

https：//rpc1.uosio.org:8250/v1/chain/abi\_json\_to\_bin

参数：

    code : string类型，合约名称

    action : string类型，action名称

    args : json类型，action参数

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/abi_json_to_bin --data '{"code":"uosio","action":"delegatebw","args": {"from":"tobetioadmax","receiver":"testaccount2","stake_net_quantity":"100.0000 UOS","stake_cpu_quantity":"100.0000 UOS","transfer":0}}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **abi\_bin\_to\_json**

将十六进制字符串转换为json格式。

https：//rpc1.uosio.org:8250/v1/chain/abi\_bin\_to\_json

参数：

    code : string类型，合约名称

    action : string类型，action名称

    binargs : string类型，action的十六进制格式

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/abi_bin_to_json --data '{"code":"uosio","action":"delegatebw","binargs":"d08d4c86baac0ecd20f2d4142193b1ca40420f000000000004554f530000000040420f000000000004554f530000000000"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**push\_transaction**

向链上推送事务。

https：//rpc1.uosio.org:8250/v1/chain/push\_transaction

参数：

    compression : string类型，是否压缩

    transaction : json类型，事务

    signatures : string类型，签名集合

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/chain/push_transaction --data '{"compression":"none","transaction":{"expiration":"2018-12-28T04:38:28","ref_block_num":1872,"ref_block_prefix":1884381553,"context_free_actions":[],"actions":[{"account":"uosio","name":"delegatebw","authorization":[{"actor":"tobetioadmax","permission":"active"}],"data":"d08d4c86baac0ecd20f2d4142193b1ca40420f000000000004554f530000000040420f000000000004554f530000000000"}],"transaction_extensions":[]},"signatures":["SIG_K1_KZxLe83ERHU1QLBJeSuii4dZtpYdNnp3HTrN6CgnWnU7R54f3eQNoX1EM19ekY7Nk2isVR47Qbi1yro5EaeE7kzB53c8L5"]}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### history

#### **get\_actions**

返回action详情。

https：//rpc1.uosio.org:8250/v1/history/get\_actions

参数：

    pos ： int32类型

    offset ： int32类型

    account\_name ： string类型,账户名

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/history/get_actions --data '{"pos":0,"offset":100,"account_name":"uosio"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_transaction**

返回事务详情。

https：//rpc1.uosio.org:8250/v1/history/get\_transaction

参数：

    id ： string类型，事务ID

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/history/get_transaction --data '{"id":"c7e8abec0d7c12622d59bcb11b8df0e08173f1e40ad0dfd741805f3aee320402"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_key\_accounts**

返回给定某个公钥下的所有账户。

https：//rpc1.uosio.org:8250/v1/history/get\_key\_accounts

参数：

    public\_key : string类型，公钥

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/history/get_key_accounts --data '{"public_key":"UOS8VrEtzikkqQBokT5ECirQQZ4Qpkh1p31spGSgTCHX95T86tU7Y"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_controlled\_accounts**

返回控制的账户。

https：//rpc1.uosio.org:8250/v1/history/get\_controlled\_accounts

参数:

    controlling\_account : string类型，账户名

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/history/get_controlled_accounts --data '{"controlling_account":"uosio"}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### net

#### **connect**

连接某个节点。

https：//rpc1.uosio.org:8250/v1/net/connect

参数：

    string类型

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/net/connect --data '"https://rpc3.uosio.org:8080"'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **disconnect**

断开与某个节点的连接。

https：//rpc1.uosio.org:8250/v1/net/disconnect

参数：

    string类型

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/net/disconnect --data '"https://rpc3.uosio.org:8080"'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **status**

返回与某个节点的连接状态。

https：//rpc1.uosio.org:8250/v1/net/status

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/net/status --data '"https://rpc3.uosio.org:8080"'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **connections**

返回所有的连接节点的信息。

https：//rpc1.uosio.org:8250/v1/net/status

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/net/connections
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### producer

#### **pause**

暂停。

https：//rpc1.uosio.org:8250/v1/producer/pause

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/producer/pause
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **resume**

重新提交。

https：//rpc1.uosio.org:8250/v1/producer/resume

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/producer/resume
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**paused**

返回暂停的生产节点。

https：//rpc1.uosio.org:8250/v1/producer/paused

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/producer/paused
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_runtime\_options**

获取运行时选项。

https：//rpc1.uosio.org:8250/v1/producer/get\_runtime\_options

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/producer/get_runtime_options
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **get\_greylist**

返回生产节点灰名单

https：//rpc1.uosio.org:8250/v1/producer/get\_greylist

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/producer/get_greylist
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **add\_greylist\_accounts**

将账户添加到灰名单。

https：//rpc1.uosio.org:8250/v1/producer/add\_greylist\_accounts

参数:

     account\_name：string数组类型，帐户名

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/producer/add_greylist_accounts --data '{"accounts":["usera"]}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **remove\_greylist\_accounts**

将账户从灰名单移除。

https：//rpc1.uosio.org:8250/v1/producer/remove\_greylist\_accounts

参数：

    account\_name：string数组类型，帐户名

```text
curl --request POST --url https://rpc1.uosio.org:8250/v1/producer/remove_greylist_accounts --data '{"accounts":["usera"]}'
```

#### **get\_whitelist\_blacklist**

返回生产节点白名单和黑名单。

https：//rpc1.uosio.org:8250/v1/producer/get\_whitelist\_blacklist

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/producer/get_whitelist_blacklist
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### **set\_whitelist\_blacklist**

设置生产节点白名单和黑名单。

https：//rpc1.uosio.org:8250/v1/producer/get\_whitelist\_blacklist

参数：

    actor\_whitelist：string类型，账户名

    actor\_blacklist：string类型，账户名

    contract\_whitelist：string类型，合约名称

    contract\_blacklist：string类型，合约名称

    action\_blacklist：string类型，action名称

    key\_blacklist：string类型，公钥

{% code-tabs %}
{% code-tabs-item title="shell" %}
```bash
curl --request POST --url https://rpc1.uosio.org:8250/v1/producer/get_whitelist_blacklist --data '{"actor_whitelist":["usera"],"actor_blacklist":[],"contract_whitelist":[],"contract_blacklist":[],"action_blacklist":[],"key_blacklist":[]}'
```
{% endcode-tabs-item %}
{% endcode-tabs %}

