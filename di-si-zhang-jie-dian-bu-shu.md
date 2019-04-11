# 第四章  节点部署

#### <a name="i2zvng"></a>__一.硬件配置__
需要提供一台公网的机器，机器的硬件配置要求：

CPU:  四核 及以上
内存：32G 及以上
      硬盘：200G 以上
系统：ubuntu 16.04 以上

#### <a name="ynnveu"></a>__二.申请用户名与注册BP__
可以通过以下三种方式获取UOS公链的用户：
* 网页申请
* 发送邮件至官方邮箱申请；
* 添加官方微信群，联系官方系统管理员进行申请；


官方网址：
官方E-mail:
官方微信群：

申请UOS用户需要提供用户名称与两把公匙:
用户名称命名规则：数字 1~5，字母 a~z
密匙对可以在官方网址上通过  密匙生成工具 生成，请通过此工具生成两对密匙 ，将公匙提供给官方，私匙请自行妥善保管；

      申请用户成功后，可以通过上述的三种途径进一步申请注册BP, 这样其它用户可以对您的用户进行投票，投票排名在前21名内可以成为超级节点，超级节点具有生产区块的权利，享受对应的一系列的分红。

#### <a name="gmg9hz"></a>三.下载与安装程序
Github地址:

下载完成进行文件的安装：
```plain
cd uosbin
sudo ./install.sh
```
成功完成安装后，可以全局访问noduos  cluos  kuosd这三个程序。

如果本地不存储区块数据，只是通过命令行cluos工具或者rpc命令进行数据访问，则在这里配置就完成了。
如果需要同步区块数据，或者想当超级节点，则需要运行noduos程序。noduos需要一系列的配置，将在下面展开进行讲解。
#### <a name="caxsgg"></a>四.修改noduos配置文件
为方便配置，假设此台机器的ip如下：
公网ip:    47.92.12.200
私网ip:   172.26.49.90
另外，我们需要使用到两个端口:
9008        //http服务器端口，提供RPC访问
12008       //区块同步端口

首先需要开放这两个端口：
```plain
sudo ufw allow 9008 12008
```

现在，我们开始修改noduos的配置文件。在uosbin目录下，打开配置文件
```plain
vim ./config.ini
```

如果只是做为普通用户节点,设置一下区块同步的ip与端口，rpc访问的ip与端口，以及当前链的名称即可，参照如下配置修改：
```plain
http-server-address = 172.26.49.90:9008
p2p-listen-endpoint = 172.26.49.90:12008

p2p-peer-address = 43.242.156.79:12008
p2p-peer-address = 43.242.156.75:12008
p2p-peer-address = 222.186.36.242:12008
p2p-peer-address = 43.242.156.69:12008
p2p-peer-address = 43.242.156.61:12008

agent-name = "myname"
```

如果您想其它机器可以通过RPC命令访问您机器上的数据，需要配置http-alias选项;如果允许其它机器可以从您的机器上同步区块数据与交易，需要配置p2p-server-address选项，即需要映射一下9008和12008两个端口，参照如下配置修改:
```plain
http-alias = 47.92.12.200:9008
p2p-server-address = 47.92.12.200:12008
```

如果需要保存当前机器所有产生的交易，参照如下配置修改：
```plain
filter-on = *
```

如果需要打开智能合约调试信息开关，参照如下修改：
```plain
contracts-console = true
```

如果想做为生产节点，需要提供生产节点名称与active权限的密匙，假设用户名称与密匙如下：
```plain
uosuosuosuos

//owner key
UOS5NEgNtAESRMWJnaDABckDUBXR8kD1UbSCiBfLjjhQJ4F4cjnw9
5Ka4HjEgjc5gze63nTg8c9roXJ2YXLRPgxWW3W5HjiC7kLq6a8L

//active key
UOS8N1jqXTJ3gMEubeBhxCF77vw6ugk8fp4jFCA5dkmTLxA7Rc9xL
5JxUgLeKaptXYZyhJumBnnihviasfo8qzriHtXN9YCb87L1dprT
```

则增加以下修改：
```plain
producer-name = uosuosuosuos
signature-provider = UOS8N1jqXTJ3gMEubeBhxCF77vw6ugk8fp4jFCA5dkmTLxA7Rc9xL=KEY:5JxUgLeKaptXYZyhJumBnnihviasfo8qzriHtXN9YCb87L1dprT
```

其中uosuosuosuos是在第二步中在官方申请的用户名称，而后面的
UOS8N1jqXTJ3gMEubeBhxCF77vw6ugk8fp4jFCA5dkmTLxA7Rc9xL
5JxUgLeKaptXYZyhJumBnnihviasfo8qzriHtXN9YCb87L1dprT
分别是uosuosuosuos用户active权限的公匙与私匙。

#### <a name="inz7yf"></a>五.开启节点

使用如下命令开启uos节点，开始同步区块
```plain
cd uosbin

//前台运行
noduos --config-dir ./ --genesis-json ./genesis.json

//后台运行
nohup noduos --config-dir ./ --genesis-json ./genesis.json &
```

如果是前台启动，会有类似如下界面，说明节点启动成功，节点开始同步区块：
```plain
uos@hml:~/uosbin$ ./noduos --config-dir ./ --genesis-json ./genesis.json 
info  2018-11-27T05:58:09.485 thread-0  chain_plugin.cpp:333          plugin_initialize    ] initializing chain plugin
info  2018-11-27T05:58:09.485 thread-0  chain_plugin.cpp:585          plugin_initialize    ] Using genesis state provided in '/home/uos/uosbin/./genesis.json'
warn  2018-11-27T05:58:09.485 thread-0  chain_plugin.cpp:592          plugin_initialize    ] Starting up fresh blockchain with provided genesis state.
info  2018-11-27T05:58:09.496 thread-0  http_plugin.cpp:422           plugin_initialize    ] configured http to listen on 172.26.48.98:9008
info  2018-11-27T05:58:09.496 thread-0  net_plugin.cpp:2901           plugin_initialize    ] Initialize net plugin
info  2018-11-27T05:58:09.496 thread-0  net_plugin.cpp:2927           plugin_initialize    ] host: 0.0.0.0 port: 12008 
info  2018-11-27T05:58:09.496 thread-0  net_plugin.cpp:2998           plugin_initialize    ] my node_id is 55743266ceee7e22ebca1af1410ac241e7b29fbed8ee52856c750184eb6cfbae
warn  2018-11-27T05:58:09.496 thread-0  net_api_plugin.cpp:96         plugin_initialize    ] 
**********SECURITY WARNING**********
*                                  *
* --         Net API            -- *
* - EXPOSED to the LOCAL NETWORK - *
* - USE ONLY ON SECURE NETWORKS! - *
*                                  *
************************************

info  2018-11-27T05:58:09.497 thread-0  main.cpp:110                  main                 ] noduos version 1.3.2-24-gae5e4b8
info  2018-11-27T05:58:09.497 thread-0  main.cpp:111                  main                 ] uosio root is /home/uos/.local/share
info  2018-11-27T05:58:09.497 thread-0  main.cpp:112                  main                 ] noduos using configuration file /home/uos/uosbin/config.ini
info  2018-11-27T05:58:09.497 thread-0  main.cpp:113                  main                 ] noduos data directory is /home/uos/.local/share/uosio/noduos/data
error 2018-11-27T05:58:09.497 thread-0  controller.cpp:1619           startup              ] No head block in fork db, perhaps we need to replay
warn  2018-11-27T05:58:09.497 thread-0  controller.cpp:576            initialize_fork_db   ]  Initializing new blockchain with genesis state                  
info  2018-11-27T05:58:09.533 thread-0  chain_plugin.cpp:707          plugin_startup       ] starting chain in read/write mode
info  2018-11-27T05:58:09.533 thread-0  chain_plugin.cpp:711          plugin_startup       ] Blockchain started; head block is #1, genesis timestamp is 2018-11-25T12:00:00.000
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:486           plugin_startup       ] start listening for http requests
info  2018-11-27T05:58:09.533 thread-0  chain_api_plugin.cpp:77       plugin_startup       ] starting chain_api_plugin
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/abi_bin_to_json
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/abi_json_to_bin
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_abi
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_account
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_block
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_block_header_state
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_code
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_code_hash
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_currency_balance
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_currency_stats
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_info
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_producer_schedule
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_producers
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_raw_abi
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_raw_code_and_abi
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_required_keys
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_scheduled_transactions
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_table_by_scope
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_table_rows
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/get_transaction_id
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/push_block
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/push_transaction
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/chain/push_transactions
info  2018-11-27T05:58:09.533 thread-0  history_api_plugin.cpp:38     plugin_startup       ] starting history_api_plugin
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/history/get_actions
info  2018-11-27T05:58:09.533 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/history/get_controlled_accounts
info  2018-11-27T05:58:09.534 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/history/get_key_accounts
info  2018-11-27T05:58:09.534 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/history/get_transaction
info  2018-11-27T05:58:09.534 thread-0  net_plugin.cpp:3017           plugin_startup       ] starting listener, max clients is 25
warn  2018-11-27T05:58:09.534 thread-0  net_plugin.cpp:708            connection           ] created connection to 114.67.37.2:20581
info  2018-11-27T05:58:09.534 thread-0  net_plugin.cpp:1921           connect              ] host: 114.67.37.2 port: 20581 
info  2018-11-27T05:58:09.534 thread-0  net_api_plugin.cpp:63         plugin_startup       ] starting net_api_plugin
info  2018-11-27T05:58:09.534 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/net/connect
info  2018-11-27T05:58:09.534 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/net/connections
info  2018-11-27T05:58:09.534 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/net/disconnect
info  2018-11-27T05:58:09.534 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/net/status
info  2018-11-27T05:58:09.534 thread-0  producer_plugin.cpp:714       plugin_startup       ] producer plugin:  plugin_startup() begin
info  2018-11-27T05:58:09.534 thread-0  producer_plugin.cpp:736       plugin_startup       ] Launching block production for 1 producers at 2018-11-27T05:58:09.534.
info  2018-11-27T05:58:09.534 thread-0  producer_plugin.cpp:748       plugin_startup       ] producer plugin:  plugin_startup() end
info  2018-11-27T05:58:09.663 thread-0  transaction_context.cp:103    deadline_timer       ] Using 12us deadline timer for checktime: min:4us max:14us mean:5us stddev:3us
debug 2018-11-27T05:58:10.084 thread-0  apply_context.cpp:28          print_debug          ] 
info  2018-11-27T05:58:10.515 thread-0  producer_plugin.cpp:337       on_incoming_block    ] Received block 90262da105ea5fe0... #1000 @ 2018-11-26T08:13:58.000 signed by uosio [trxs: 0, lib: 999, conf: 0, latency: 78252515 ms]
info  2018-11-27T05:58:11.279 thread-0  producer_plugin.cpp:337       on_incoming_block    ] Received block bb70bf4d26df06f9... #2000 @ 2018-11-26T08:30:38.000 signed by uosio [trxs: 0, lib: 1999, conf: 0, latency: 77253279 ms]
info  2018-11-27T05:58:12.045 thread-0  producer_plugin.cpp:337       on_incoming_block    ] Received block 674d1c61f5364fce... #3000 @ 2018-11-26T08:48:10.000 signed by uosio [trxs: 0, lib: 2999, conf: 0, latency: 76202045 ms]
info  2018-11-27T05:58:12.813 thread-0  producer_plugin.cpp:337       on_incoming_block    ] Received block 83620b4c6cbdb2ce... #4000 @ 2018-11-26T09:04:50.000 signed by uosio [trxs: 0, lib: 3999, conf: 0, latency: 75202813 ms]
```

区块的数据保存在以下位置：
```plain
~/.local/share/uosio/noduos/data
```

上面是正常启动节点所使用的命令，如果需要停掉节点：
* 前台运行：使用 Ctrl + C  即可
* 后台运行：  使用   killall noduos  

再次开启节点的时候，如果上一次是使用 Ctrl + C 关掉的程序，使用如下命令开启：
```plain
//前台运行
noduos --config-dir ./ 

//后台运行
nohup noduos --config-dir ./ &
```
如果是使用killall命令或者其它异常情况(机器断电等)下停止的程序，则需要使用以下方式重新开启：
```plain
cd uosbin

//前台运行
noduos --config-dir ./ --hard-replay-blockchain

//后台运行
nohup noduos --config-dir ./ --hard-replay-blockchain &
```

如果想删除以前所有的历史区块，从第一个区块开始同步数据，可以使用参数 --delete-all-blocks：
```plain
cd uosbin

//前台运行
noduos --config-dir ./ --genesis-json ./genesis.json --delete-all-blocks

//后台运行
nohup noduos --config-dir ./ --genesis-json ./genesis.json --delete-all-blocks &
```

#### <a name="kt0lux"></a>六.钱包的使用
1.开启钱包服务
首先在命令行运行一次  kuosd ,显示如下：
```plain
uos@hml:~$ kuosd
info  2018-11-27T06:16:19.270 thread-0  wallet_plugin.cpp:42          plugin_initialize    ] initializing wallet plugin
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/kuosd/stop
info  2018-11-27T06:16:19.270 thread-0  wallet_api_plugin.cpp:73      plugin_startup       ] starting wallet_api_plugin
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/create
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/create_key
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/get_public_keys
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/import_key
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/list_keys
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/list_wallets
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/lock
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/lock_all
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/open
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/remove_key
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/set_timeout
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/sign_digest
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/sign_transaction
info  2018-11-27T06:16:19.270 thread-0  http_plugin.cpp:554           add_handler          ] add api url: /v1/wallet/unlock

```
使用ctrl +c关掉，kuosd在当前用户的目录下创建了一个文件夹uosio-wallet，修改里面的配置文件：
```plain
cd uosio-wallet
vim config.ini
```
打开以后，修改钱包的ip与端口：
```plain
http-server-address = 127.0.0.1:8900
unlock-timeout = 90000
```
其中unlock-timeout默认为900,即钱包15分钟自动锁定不能使用，用户根据自己的需要设定时间，设置好后使用以下命令将钱包程序放到后台运行：
```plain
nohup kuosd &
```



