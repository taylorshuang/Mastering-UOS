# 节点部署

<a name="ef9cd036"></a>
# 主网节点部署

1. 下载安装文件<br />
wget [ftp://tools.ulord.one/UOS_Mainnet.tar.gz]()

2. 将UOS_Mainnet.tar.gz放到当前用户的根目录下并解压文件<br />
  tar  -zxvf  ~/UOS_Mainnet.tar.gz
 
3. 修改文件执行权限<br />
  chmod +x ~/uos/noduos ~/uos/cluos ~/uos/kuosd ~/uos/install.sh<br />
  ~/uos/uninstall.sh

4. 安装uos程序<br />
sudo ~/uos/install.sh

5. 修改noduos配置文件<br />
配置文件路径：~/uos/cfg/config.ini<br />
(1)修改状态数据库大小，使用free -m 查看当前系统可用内存，最好大于16G可用，如下是把状态数据库配置为30G：<br />
chain-state-db-size-mb = 30480<br />

(2)修改http服务地址，假如您的计算机私网ip为10.186.14.20，启用9008端口，如下配置：<br />
http-server-address = 10.186.14.20:9008<br />

(3)修改端口映射，即映射(2)中的服务端口。假如您的公网ip为114.61.17.245，如下配置：<br />
http-alias = 114.61.17.245:9008<br />

(4)修改p2p服务地址。假如您的计算机私网ip为10.186.14.20，启用12008端口做为数据同步端口，如下设置：<br />
p2p-listen-endpoint = 10.186.14.20:12008<br />

(5)修改p2p服务端口地址映射，即映射(4)中的服务端口。假如您的公网ip为114.61.17.245，如下配置：<br />
p2p-server-address = 114.61.17.245:12008<br />

(6)增加p2p peer地址。这里是指其它机器的服务地址。这里可以指定多个地址,目前主网可用的部分p2p地址如下:<br />

  p2p-peer-address = 114.67.37.75:9660<br />
  p2p-peer-address = 114.67.37.76:9660<br />
  p2p-peer-address = 124.232.147.65:7020<br />
  p2p-peer-address = rpc.uos.iccob.com:12008<br />
  p2p-peer-address = 111.204.209.105:11111<br />
  p2p-peer-address = uos.chainclub.one:12008<br />
  p2p-peer-address = 27.128.165.213:12008

6. 修改节点名称
  agent-name = "myname"<br /><br />
  
7. 打开上面服务器的配置的端口
  sudo ufw allow 9008 12008<br /><br />

8. 运行noduos程序

```
//前台
noduos --genesis-json ~/uos/cfg/genesis.json --config-dir ~/uos/cfg/ --data-dir ~/uosdata

//后台
nohup noduos --genesis-json ~/uos/cfg/genesis.json --config-dir ~/uos/cfg/ --data-dir ~/uosdata &
```

此时节点开始运行，从别的节点同步区块，同步完成后方可接受实时交易。<br />
<br />

<a name="de71f21f"></a>
# UOS生产节点部署及申请
<a name="a4d3b02a"></a>
## 概述
   UOS采用DPOS共识，DPOS类似董事会投票，持币者投出一定数量的节点，代理其进行验证和记账，大幅缩小参与验证和记账节点的数量，可以达到秒级的共识验证。我们称这部分被选出来的节点为生产节点或BP节点。<br />BP节点将为<br />UOS网络提供算力和带宽支持，它们的主要任务就是收集交易信息将其打包进区块，并将区块广播给其他节点，通过验证后再将区块上传至区块链。同时，BP节点还将拥有 UOS 生态的投票权，2/3以上的BP节点投票将使得链上的决策（提案）生效。<br />申请成为BP节点对服务器有较高的要求；同时，需要抵押100000 UOS。<br />当然，成为BP节点可获取奖励，包含出块奖励和投票奖励。同时，为保证链的安全和稳定，BP可以投出作恶或者性能不好的BP节点，一旦BP节点被投出，将扣除其押金和（未领取的）奖励；并将该账户移入黑名单。

<a name="a9f94dcd"></a>
## 部署
1.从网站上下载安装文件（该文档中下载文件存放在当前用户根目录）
<br />wget [ftp://tools.ulord.one/UOS_Testnet.tar.gz]()

2. 解压下载的文件
  tar -zxvf ~/UOS_Testnet.tar.gz

3. 修改文件执行权限
  chmod +x ~/uos/noduos ~/uos/cluos ~/uos/kuosd ~/uos/install.sh ~/uos/uninstall.sh                     
  ~/uos/mongodb/bin/mongod

4. 安装uos程序
  sudo ~/uos/install.sh

5. 修改noduos配置文件
  配置文件路径：~/uos/cfg/config.ini<br />
  (1)修改状态数据库大小，使用free -m 查看当前系统可用内存，最好大于16G可用，下面是把状态数据库配置为20G大小：
  <br />chain-state-db-size-mb = 20480

  (2)修改http服务地址,假如您的计算机私网ip为10.186.14.20，启用9008端口，如下配置：
  <br />http-server-address<br />= 10.186.14.20:9008

  (3)修改端口映射，即映射(2)中的服务端口。假如您的公网ip为114.61.17.245,如下配置：<br />http-alias =114.61.17.245:9008

如果只有一个公网ip地址，此项与http-server-address填写一致，即：<br />http-alias = 10.186.14.20:9008

(4)修改p2p服务地址。假如您的计算机私网ip为10.186.14.20，启用12008端口做为数据同步端口，如下设置：<br />p2p-listen-endpoint = 10.186.14.20:12008

(5)修改p2p服务端口地址映射，即映射(4)中的服务端口。假如您的公网ip为114.61.17.245，如下配置：<br />p2p-server-address =114.61.17.245:12008

如果只有一个公网ip地址，此项与p2p-listen-endpoint填写一致，即：<br />p2p-server-address = 10.186.14.20:12008

(6)增加p2p peer地址。这里是指其它机器的服务地址。如想与公网为114.67.37.2，端口为20581的节点同步数据，如下设置：<br />p2p-peer-address= 114.67.37.2:20581

这里可以指定多个地址：<br />p2p-peer-address = 114.67.37.245:12008<br />p2p-peer-address = 124.232.156.101:12008<br />p2p-peer-address = 124.232.147.65:12008

(7)修改节点名称<br />agent-name= "myname"

(8)设置生产节点名称（假设您的账户名为uosgenesuser）<br />producer-name = uosgenesuser<br />(9)设置签名私钥（您的账户的active公私钥对）<br />signature-provider= UOS6K6EJq8WXrK1nQ92rw7GfuGh9VvBquqSkKw7zLCJEjbZvPNPsE=KEY:5HxmNvuR7fPbRKU87j39B3CoJ5e2zYnuYFLXvukqQt3EARvbnLU

(10)修改加载插件（BP节点加载以下4个插件即可）<br />plugin= uosio::chain_api_plugin<br />plugin= uosio::chain_plugin<br />plugin= uosio::net_plugin<br />plugin= uosio::net_api_plugin

5. 打开上面服务器的配置的端口

sudo ufw allow 9008 12008

6. 运行noduos程序

前台运行命令：<br />noduos --genesis-json ~/uos/cfg/genesis.json --config-dir ~/uos/cfg/ --data-dir ~/uos/data<br />
<br />后台运行命令：<br />nohup noduos --genesis-json ~/uos/cfg/genesis.json --config-dir ~/uos/cfg/ --data-dir ~/uos/data &

<a name="a02c7020"></a>
### 抵押
一个UOS节点若想申请成为生产节点，除了服务器的硬件条件外，还需要抵押100000 UOS。因此，该账户需要抵押足够的UOS。命令如下：<br />cluos system delegatebw uosgenesuser uosgenesuser "50000.0000 UOS" "50000.0000 UOS"

查看抵押是否成功：<br />cluos get account uosgenesuser<br />说明：普通用户抵押UOS获取net或cpu资源也是通过该命令实现，普通用户的cpu抵押可用于对生产节点投票，而net抵押不能用于投票。

<a name="a40f3554"></a>
### 申请
申请的账户抵押UOS之后，发起申请，命令如下（最后一个参数为ulord地址）：<br />cluos system regproducer uosgenesuser UOS6K6EJq8WXrK1nQ92rw7GfuGh9VvBquqSkKw7zLCJEjbZvPNPsE [https://uosgenesuser.com/chain](https://uosgenesuser.com/chain) uPmEkuRuq36TvYHYWcNZYJwdVYWc79Z22w

查看申请是否成功：<br />cluos get table uosio uosio producers -l 100<br />在显示的列表中查看是否已有自己的节点的名称。

<a name="e7f40ca8"></a>
### 投票
所有生产节点中，前21个节点可以生产区块，申请成功后，UOS持有者可以对该账户投票（生产节点可以给自己投票），命令如下：<br />cluos system voteproducer prods testaccount2 "50000.0000 UOS"  uosgenesuser

查看投票是否成功：<br />cluos get account uosgenesuser<br />cluos get table uosio uosio producers<br />说明：选项total_votes除以10000为当前候选节点获得的投票数。

<a name="d783eed7"></a>
### 撤销投票
如果您想撤销投票，请使用以下命令：<br />cluos system cancelvote uosgenesuser "1000000.0000 UOS" uosgenesuser

查看是否成功撤销：<br />cluos get table uosio uosio producers

赎回（投票账户）<br />普通账户撤销投票后，可以赎回抵押的UOS，命令如下：<br />cluos system undelegatebw testaccount2 testaccount2 "50000.0000 UOS" "50000.0000 UOS"

<a name="8d63ef38"></a>
### 暂停
生节点由于硬件维护原因暂时不能出块，申请暂时退出，命令如下：<br />cluos push action uosio unregprod '["uosgenesuser","0"]' -p uosgenesuser[@active ]()

查看暂停是否成功：（成功，则is_active的值置为0）<br />cluos get table uosio uosio producers

<a name="c7db6d4f"></a>
### 恢复
恢复出块命令与申请命令相同，此处不再重复。

查询是否成功恢复：<br />cluos get table uosio uosio producers

<a name="c3992269"></a>
### 退出
如果某生产节点想退出生产者队列，并撤回抵押的资金，则需要uosio授权，该操作需要发起提案来实现，提案命令如下：<br />cluos multisig propose releaseprod '[{"actor":"uosgenesuser","permission":"active"},{"actor":"dragonexsafe","permission":"active"},{"actor":"gm2tqnjygyge","permission":"active"},{"actor":"marsaccount3","permission":"active"},{"actor":"robertocarlo","permission":"active"},{"actor":"uosvegasjack","permission":"active"}]' '[{"actor":"uosgenesuser","permission":"active"}]' uosio unregprod '{"producer":"uosgenesuser","release":1}' -p uosgenesuser

查看是否成功退出：<br />cluos get table uosio uosio producers<br />说明：释放之后，该账户可以重新申请成为生产节点。

<a name="fcef0ed2"></a>
### 赎回（生产节点）
生产节点退出成功后，可赎回抵押的UOS，命令如下：<br />cluos system undelegatebw uosgenesuser uosgenesuser "50000.0000 UOS" "50000.0000 UOS"

如果该生产节点没有退出，则无法赎回抵押的100000<br />UOS，发起赎回命令将报错（超出部分可赎回）：<br />注意：用于投票的UOS需要取消投票之后才能赎回。

<a name="86048b4f"></a>
### 移除
如果生产节点长时间不出块，或者对外提供的访问节点长时间无法访问，uosio有权限删除该节点，且会扣除抵押的全部资金（优先扣除cpu抵押）。该操作需要发起提案来实现，提案命令如下：<br />cluos multisig propose removeprod '[{"actor":"uosgenesuser","permission":"active"},{"actor":"dragonexsafe","permission":"active"},{"actor":"gm2tqnjygyge","permission":"active"},{"actor":"marsaccount3","permission":"active"},{"actor":"robertocarlo","permission":"active"},{"actor":"uosvegasjack","permission":"active"}]' '[{"actor":"dragonexsafe","permission":"active"}]' uosio rmvproducer '{"producer":"uosgenesuser"}' -p dragonexsafe

移除之前，查看账户详情：<br />cluos get account uosgenesuser

移除之后，查看该生产节点状态：<br />cluos get table uosio uosio producers<br />移除之后，查看账户详情：<br />cluos get account uosgenesuser<br />注意：以上是测试链截图，因此仅扣除1000<br />UOS，正式链将会扣除100000 UOS。移除之后，该账户无法再次申请成为生产节点。

<a name="c39a1ae0"></a>
### 奖励
生产节点需要自行领取奖励，每天可领取一次，申领命令如下：<br />cluos system claimrewards testaccount2

已经被移除的生产节点则无法领取奖励。如下图，领取奖励将报错：<br />


<a name="3a15a9e4"></a>
# 测试网节点部署

1. 测试网的节点部署与主链节点部署基本一致， 其安装文件的下载

wget [ftp://tools.ulord.one/UOS_Testnet.tar.gz]()<br />

1. 然后进行解压：

tar -zxvf ~/UOS_Testnet.tar.gz<br /><br />
1. 后面的步骤与主网的一致，直到”增加p2p peer地址”的时候，填写

p2p-peer-address = 114.67.37.2:20581<br />p2p-peer-address= 114.67.37.245:12008<br />程序的启动方法与主网一致。<br />


<a name="c98e06d2"></a>
# 搭建UOS节点私网

<br />为方便我们的测试与开发，我们可以搭建自己的UOS私有网络，可以使用多台机器搭建，也可以使用一台机器搭建，注意一下端口配置即可，下面我们来使用ip为10.186.11.110的一台机器来构建我们的私网。<br />
<br />  注：在此之前，如果没有安装UOS程序，请先按照主网或者测试网节点部署里的说明下载与安装UOS程序。

1. 创建密匙

在命令行输入以下命令可以生成密匙:<br />cluos create key –to-console

下面是我们通过三次cluos命令的调用生成的公私匙对：

```
//uosio
Public key: UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
Private key: 5KWHUBmukJaqccDQ3EGfbR3jpYnMfo4Ys98fGtXY8GEMasyhTRM

//marsaccount3(producer)
//owner
Private key:5Jt9DpJHchaQLjY4rDZxxexu3HSFtobX4xYFWT4VkUGyQbkRQC3
Public key: UOS6tRMkGV71q3HGiTDWQF5WJ1m5XTb5xiwa2ChLVztBwM4GLDNV4
//active
Private key:5JBP26FH3SduMZEyf7mdXYAy4pFqfedpdU4YB7J3DNLxX3TvWmT
Public key:UOS8EAN6hruoe4HHRGFKUVYTM5yhVEoeGcAxutpTvA37QeqTdsPaW

//sign
Private key:5HvWgzntm2oAgaNvq2YJv1bdVCjC6s6Ux6utcqKJS4jfyGb8RGu
Public key:UOS67pXkxe4C3B8hsLJk2oXMkMqWpBxP1413WJG13q8hesyADkroR

//uosvegasjack(producer)
//owner
Private key:5J4XRydJBXqUoxSuPzNJEefgBCnXuqka43QvLqn4fJdamxmU3XS
Public key:UOS7DxdqHvnQ7MRqvA6n8S3Cz2gm6G2YN6vVFLQZoLAgSef7k3A77
//active
Private key:5JeAZLJHssaPGAmfDyVnvMvCywmL9ayJAnJMNeQmdnDjMQcvrMo
Public key:UOS4uGpLZrWVpP42dxC1QVQ4jReDkePPJvJphHkLQKtzWgZs77HyR
//sign
Private key:5K1mqs5ysrfiMY8JKwhRh37NGckoU5YtwWW6YoUSUL37XofKs5V
Public key: UOS6evJJre5M9C9wkzS18gi3FYf4qCMLApTzcJ24e6XhMvusSiBDq

//dragonexsafe(producer)
//owner
Private key:5KhWpmspCepD5W4ooPi1MKM3DXFfC27qMvmQa5Gz9Ub3fnhudCx
Public key:UOS74LnwpTH4Txixcz5Ze7v78FZLwLZJmBdzCJFBPQinQRbdssVny
//active
Private key:5KRcKYJMAYL3VDfGQtrNGBJb268sou2yWy3rVeCgv5xJZ6bUfh6
Public key:UOS87iQtt9sPgf949RC6vdRoqZvHPUg68SVBwLXYb8HTMLwmteqvT
//sign
Private key:5JcNjjvZBPTqjjBnnqfdP3TBn19nMACr5zbN88KTcpCwV8vqyuz
Public key:UOS8gqvsvhd8TBLvFdTxtPbDmcR4MyuLWUmJBtJUkoMt9TjT2Fm1C
```

<br />注：其中marsaccount3、uosvegasjack、dragonexsafe表示三个bp用户，将来用于出块,而其中超级用户uosio的密匙是默认产生的，不要手动产生，直接使用即可。

1. 创建钱包与导入密匙

我们创建一个钱包，并将密匙导入到钱包中：<br />cluos wallet create -n uos --to-console<br />
<br />请把返回的钱包密匙妥善保管，用于将来解锁钱包。下面将密匙导入到钱包中，用来签名我们的交易:

```
cluos wallet import -n uos --private-key  5KWHUBmukJaqccDQ3EGfbR3jpYnMfo4Ys98fGtXY8GEMasyhTRM
cluos wallet import -n uos --private-key  5JBP26FH3SduMZEyf7mdXYAy4pFqfedpdU4YB7J3DNLxX3TvWmT
cluos wallet import -n uos --private-key  5HvWgzntm2oAgaNvq2YJv1bdVCjC6s6Ux6utcqKJS4jfyGb8RGu
cluos wallet import -n uos --private-key 5JeAZLJHssaPGAmfDyVnvMvCywmL9ayJAnJMNeQmdnDjMQcvrMo
cluos wallet import -n uos --private-key 5K1mqs5ysrfiMY8JKwhRh37NGckoU5YtwWW6YoUSUL37XofKs5V
cluos wallet import -n uos --private-key 5KRcKYJMAYL3VDfGQtrNGBJb268sou2yWy3rVeCgv5xJZ6bUfh6
cluos wallet import -n uos --private-key 5JcNjjvZBPTqjjBnnqfdP3TBn19nMACr5zbN88KTcpCwV8vqyuz
```

1. 开启超级用户uosio节点

```
noduos -e -p uosio -d ~/uosdata/uosio --http-server-address 10.186.11.110:6000 --p2p-listen-endpoint 10.186.11.110:7000 --plugin uosio::net_plugin --plugin uosio::net_api_plugin   --plugin uosio::chain_plugin --plugin uosio::chain_api_plugin --p2p-peer-address 47.92.123.209:9008 --contracts-console --delete-all-blocks &
```

命令行各字段的解析如下:<br />-e:   表示当前节点需要出块<br />-p:   生产节点对应的用户名称<br />-d:   区块数据存储路径<br />--http-server-address:http协议访问地址<br />--p2p-listen-endpoint:p2p监听地址，用于同步区块<br />--plugin:  当前程序启动使用的插件名称<br />--p2p-peer-address:p2p对端地址<br />--contracts-console:      智能合约打印开关<br />--delete-all-blocks:       删除当前节点的所有区块，使用之前请慎重考虑

1. 启动三个bp节点

开启三个命令行终端，分别运行如下指令：

```
noduos  -p marsaccount3 -d ~/uosdata/marsaccount3 --http-server-address 10.186.11.110:6001 --p2p-listen-endpoint 10.186.11.110:7001 --plugin uosio::net_plugin --plugin uosio::net_api_plugin --plugin uosio::chain_plugin --plugin uosio::chain_api_plugin   --p2p-peer-address 10.186.11.110:7000 --p2p-peer-address 10.186.11.110:7002 --p2p-peer-address 10.186.11.110:7003 --signature-provider UOS67pXkxe4C3B8hsLJk2oXMkMqWpBxP1413WJG13q8hesyADkroR=KEY:5HvWgzntm2oAgaNvq2YJv1bdVCjC6s6Ux6utcqKJS4jfyGb8RGu  --delete-all-blocks &
noduos  -p uosvegasjack -d ~/uosdata/uosvegasjack --http-server-address 10.186.11.110:6002 --p2p-listen-endpoint 10.186.11.110:7002 --plugin uosio::net_plugin --plugin uosio::net_api_plugin --plugin uosio::chain_plugin --plugin uosio::chain_api_plugin   --p2p-peer-address 10.186.11.110:7000 --p2p-peer-address 10.186.11.110:7001 --p2p-peer-address 10.186.11.110:7003 --signature-provider UOS6evJJre5M9C9wkzS18gi3FYf4qCMLApTzcJ24e6XhMvusSiBDq=KEY:5K1mqs5ysrfiMY8JKwhRh37NGckoU5YtwWW6YoUSUL37XofKs5V  --delete-all-blocks &
noduos  -p dragonexsafe -d ~/uosdata/dragonexsafe --http-server-address 10.186.11.110:6003 --p2p-listen-endpoint 10.186.11.110:7003 --plugin uosio::net_plugin --plugin uosio::net_api_plugin --plugin uosio::chain_plugin --plugin uosio::chain_api_plugin   --p2p-peer-address 10.186.11.110:7000 --p2p-peer-address 10.186.11.110:7001 --p2p-peer-address 10.186.11.110:7002 --signature-provider UOS8gqvsvhd8TBLvFdTxtPbDmcR4MyuLWUmJBtJUkoMt9TjT2Fm1C=KEY:5JcNjjvZBPTqjjBnnqfdP3TBn19nMACr5zbN88KTcpCwV8vqyuz  --delete-all-blocks &
```

<br />运行后，可以看到这三个节点在同步区块，生产区块的为uosio用户。

1. 创建系统用户

```
cluos  -u http://10.186.11.110:6000 create account uosio uosio.bpay UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
cluos  -u http://10.186.11.110:6000 create account uosio uosio.msig  UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
cluos  -u http://10.186.11.110:6000 create account uosio uosio.names UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
cluos  -u http://10.186.11.110:6000 create account uosio uosio.ram   UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
cluos  -u http://10.186.11.110:6000 create account uosio uosio.ramfee UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
cluos  -u http://10.186.11.110:6000 create account uosio uosio.saving  UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
cluos  -u http://10.186.11.110:6000 create account uosio uosio.stake UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
cluos  -u http://10.186.11.110:6000 create account uosio uosio.token  UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
cluos  -u http://10.186.11.110:6000 create account uosio uosio.vpay UOS5qf77wsPHB1gu2o5K7vbcnSzeR4aaCQ3NZ4AEE7SJscnpHR5yA
```

1. 加载智能合约

```
cluos  -u http://10.186.11.110:6000 set contract uosio.token ~/mainnet/uosio.token/
cluos  -u http://10.186.11.110:6000 set contract uosio.msig ~/mainnet/uosio.msig/
cluos  -u http://10.186.11.110:6000 push action uosio.token create '["uosio", "1000000000.0000 UOS"]' -p uosio.token@active
cluos  -u http://10.186.11.110:6000  push action uosio.token issue '["uosio", "1000000000.0000 UOS", "memo"]' -p uosio
cluos  -u http://10.186.11.110:6000 set contract uosio ~/mainnet/uosio.system/ -x 3000
cluos  -u http://10.186.11.110:6000 set contract uosio ~/mainnet/uosio.system/
cluos  -u http://10.186.11.110:6000 push action uosio setpriv '["uosio.msig", 1]' -p uosio
```

1. 创建bp用户

```
cluos  -u http://10.186.11.110:6000 system newaccount --transfer uosio marsaccount3 UOS6tRMkGV71q3HGiTDWQF5WJ1m5XTb5xiwa2ChLVztBwM4GLDNV4 UOS8EAN6hruoe4HHRGFKUVYTM5yhVEoeGcAxutpTvA37QeqTdsPaW   --stake-net "1000000.0000  UOS" --stake-cpu "100000.0000  UOS" --buy-ram "10000.0000  UOS"
cluos  -u http://10.186.11.110:6000 system newaccount --transfer uosio uosvegasjack UOS7DxdqHvnQ7MRqvA6n8S3Cz2gm6G2YN6vVFLQZoLAgSef7k3A77 UOS4uGpLZrWVpP42dxC1QVQ4jReDkePPJvJphHkLQKtzWgZs77HyR --stake-net "2000000.0000  UOS" --stake-cpu "100000.0000  UOS" --buy-ram "10000.0000  UOS"
cluos  -u http://10.186.11.110:6000 system newaccount --transfer uosio dragonexsafe UOS74LnwpTH4Txixcz5Ze7v78FZLwLZJmBdzCJFBPQinQRbdssVny UOS87iQtt9sPgf949RC6vdRoqZvHPUg68SVBwLXYb8HTMLwmteqvT --stake-net "3000000.0000  UOS" --stake-cpu "100000.0000  UOS" --buy-ram "10000.0000  UOS"
```

1. 注册候选节点

```
cluos  -u http://10.186.11.110:6000 system regproducer marsaccount3 UOS67pXkxe4C3B8hsLJk2oXMkMqWpBxP1413WJG13q8hesyADkroR http://marsaccount3.com UOS67pXkxe4C3B8hsLJk2oXMkMqWpBxP1413WJG13q8hesyADkroR
cluos  -u http://10.186.11.110:6000 system regproducer uosvegasjack UOS6evJJre5M9C9wkzS18gi3FYf4qCMLApTzcJ24e6XhMvusSiBDq http://uosvegasjack.com UOS6evJJre5M9C9wkzS18gi3FYf4qCMLApTzcJ24e6XhMvusSiBDq
cluos  -u http://10.186.11.110:6000 system regproducer dragonexsafe UOS8gqvsvhd8TBLvFdTxtPbDmcR4MyuLWUmJBtJUkoMt9TjT2Fm1C http://dragonexsafe.com UOS8gqvsvhd8TBLvFdTxtPbDmcR4MyuLWUmJBtJUkoMt9TjT2Fm1C
```

1. 投票

三个bp分别给自己进行投票

```
cluos  -u http://10.186.11.110:6000 system voteproducer prods marsaccount3 "1000000.0000 UOS" marsaccount3
cluos  -u http://10.186.11.110:6000 system voteproducer prods uosvegasjack "2000000.0000 UOS" uosvegasjack
cluos  -u http://10.186.11.110:6000 system voteproducer prods dragonexsafe "1200000.0000 UOS" dragonexsafe
```

<br />投完票后，可以看到三个bp用户 marsaccount3、uosvegasjack、dragonexsafe开始产生区块，私网搭建成功。

