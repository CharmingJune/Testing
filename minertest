# 矿工0.2.5接入教程

    一个`miner`对应多个`storagenode`。

- 启动`miner`和`storagenode`

> 1. 下载安装包并解压
> 
> 2. 配置`lambdacli`
> 
> 3. 添加矿工账户
> 
> 4. 创建`miner`
> 
> 5. 配置和启动`storagenode`
> 
> 6. 买单/卖单/续期操作

- 文件上传和查看

> 1. 配置
> 
> 2. 文件操作
> 
> 3. 查看订单和矿工状态

- 文件分享

> - 分享文件
> 
> - 接收分享文件
> 
> - 接收并下载分享文件
> 
> - 下载分享文件

- 挖矿

- 排错

## 一、启动`miner`和`storagenode`

1. ### 下载安装包并解押

> 1. 创建并进入目录
> 
> ```shell
> mkdir -p ~/LambdaIM && cd ~/LambdaIM
> ```
> 
> 2. 下载安装包
> 
> ```shell
> wget https://github.com/LambdaIM/launch/releases/download/Storage0.2.5/lambda-storage-0.2.5-testnet.tar.gz
> # 如果下载缓慢可使用下面的链接
> wget http://download.lambdastorage.com/lambda-storage/0.2.5/lambda-storage-0.2.5-testnet.tar.gz
> ```
> 
> 3. 解压安装包
> 
> ```shell
> tar zxvf lambda-storage-0.2.5-testnet.tar.gz
> ```
> 
> 4. 进入解压后的目录
> 
> ```shell
> cd lambda-storage-0.2.5-testnet
> ```

2. ### 配置`lambdacli`

> ```shell
> ./lambdacli config node tcp://[nodeip]:26657
> ```
> 
> `[nodeip]`为自己质押的验证节点`公网IP`。
> 
> 可选Lambda官方验证节点IP如下:
> 
> ```textile
> 47.93.196.236
> 47.94.129.97
> 39.105.148.217
> 182.92.66.63
> ```
> 
> ```shell
> ./lambdacli config chain-id lambda-chain-test4.8
> ./lambdacli config trust-node true
> ./lambdacli config dht-gateway-address [kad.external_address]
> ```
> 
> `[kad.external_address] `为验证节点配置`lambda.toml`中的`kad.external_address`，可填写自己质押的验证节点`dht地址`。
> 
> 可选Lambda官方`dht-gateway-address`如下:
> 
> ```textile
> 47.93.196.236:13000
> 47.94.129.97:13000
> 39.105.148.217:13000
> 182.92.66.63:13000
> ```

3. ### 添加矿工账户

    将命令行中的`[your-account-name]`替换成您自定义的矿工账户名称，需要设置您的账户密码，不用加中括号。

    矿工子账户用来提交挖矿声明和挖矿证明，每笔交易需要一定的手续费，需要保证矿工子账户余额大于`1000LAMB`。

    提示：也可以使用钱包进行添加矿工账户、导入/导出矿工子账户、转账、质押等操作。

- #### 方式一：新建矿工账户及子账户（新用户）

> ```shell
> ./lambdacli keys add [your-account-name] --generate-miner
> ```

- #### 方式二：导入矿工账户及子账户（老用户）

> 如果是钱包创建的账户导入，则通过钱包创建账户时候的助记词进行导入：
> 
> ```shell
> ./lambdacli keys add [your-account-name] --recover 
> ```
> 
> 输入命令后按照提示输入密码和助记词即导入账户成功。
> 
> 然后生成矿工子账户：
> 
> ```shell
> ./lambdacli keys create-miner [your-account-name]
> ```
> 
> 输入命令后按照提示输入助记词即可。

    添加好矿工账户及子账户以后，在当前目录下会生成名为`[your-account-name]_miner_key.json`的账户信息文件，请勿手动修改。

4. ### 创建`miner`
- #### 质押资产到节点

> ```shell
> ./lambdacli tx staking delegate [validator-address] [amount-of-utbb] --from [your-account-name] --broadcast-mode block -y
> ```
> 
> `[validator-address] `为验证节点地址，可使用自己质押的验证节点地址或 Lambda官方验证节点地址，节点地址可通过[浏览器](http://testbrowser.lambda.im/#/)查找或询问节点；
> 
> `[amount-of-utbb]` 为质押到验证节点的`utbb`数量，不得少于`1TBB（1TBB=1000000utbb）`；
> 
> `[your-account-name]` 是您的矿工账户名称。
> 
> 可选Lambda官方节点地址：
> 
> ```textile
> lambdavaloper1prrcl9674j4aqrgrzmys5e28lkcxmntxuvjpcl
> lambdavaloper1thj5fv8d0dsh3aealhpxm9mvgxjfh87suwuj2h
> lambdavaloper1a83p8s9gs5hua09xn5ktmahepst3vzg9u2l20d
> lambdavaloper1r340rrv9fs95gqy5087e2mtz82vvwrglt6amx3
> ```
> 
> 例如质押20TBB到节点lambdavaloper1prrcl9674j4aqrgrzmys5e28lkcxmntxuvjpcl：
> 
> ```shell
> ./lambdacli tx staking delegate lambdavaloper1prrcl9674j4aqrgrzmys5e28lkcxmntxuvjpcl 20000000utbb --from myaccount --broadcast-mode block -y
> ```

- #### 初始化矿工及配置

> 1. 初始化矿工
> 
> > ```shell
> > ./minernode init
> > ```
> > 
> > 会生成矿工配置文件`~/.lambda_miner/config/config.toml`。
> 
> 2. 修改配置文件
> 
> > ```shell
> > vim ~/.lambda_miner/config/config.toml
> > ```
> > 
> > 配置文件说明如下：
> > 
> > ```toml
> > [build]
> > version = "0.2.5"
> > commit = "030c696bc6829cfafb3d240d66058b16b41aa460"
> > mode = "release"
> > 
> > # 服务需要监听的地址
> > # 以本机内网IP为 192.168.10.10，端口映射的外网IP为 200.200.200.100 为例
> > [server]
> > # 对外提供服务的地址，推荐配置为内网地址做端口映射到外网IP
> > address = "192.168.10.10:13000"
> > # 对内提供服务的地址，主要是给StorageNode使用，推荐配置为内网地址
> > private_address = "192.168.10.10:13001"
> > 
> > [kad]
> > # DHT接入节点地址，存储网络提供，可填写多个，以 47.94.129.97:13000 为例
> > # 可填写自己质押的验证节点配置lambda.toml中的 kad.external_address
> > # 可选官方dht地址：39.105.148.217:13000/47.94.129.97:13000/47.93.196.236:13000/182.92.66.63:13000
> > bootstrap_addr = ["47.94.129.97:13000"]
> > # this should listen at Public IP
> > ## 对外暴露的提供服务的地址
> > external_address = "200.200.200.100:13000"
> > 
> > [log]
> > level = "info"
> > output_file = "stdout"
> > 
> > [miner]
> > # ensure_level=0会多占用磁盘1/6空间，ensure_level=1会多占用1/3空间
> > ensure_level = "0"
> > #root access key，不能为空
> > root_secret_seed = "aaa"
> > 
> > [db]
> > # db config
> > lru_cache = "0"
> > keep_log_file_num = "16"
> > write_buffer_size = "268435456"
> > recycle_log_file_num = "0"
> > target_file_size_base = "268435456"
> > max_write_buffer_number = "25"
> > max_bytes_for_level_base = "4294967296"
> > level_0_stop_writes_trigger = "24"
> > target_file_size_multiplier = "1"
> > max_background_compactions = "2"
> > max_bytes_for_level_multiplier = "2"
> > level_0_slowdown_writes_trigger = "17"
> > level_0_file_num_compaction_trigger = "8"
> > level_compaction_dynamic_level_bytes = "0"
> > compaction_algorithm = "0"
> > rate_bytes_per_sec = "67108864"
> > data_backup_path = ""
> > ```

- #### 查看矿工子账户地址

> 将添加账户时生成的`[your-account-name]_miner_key.json`账户信息文件重命名为`default_miner_key.json`并移动到`~/.lambda_miner/config/`目录：
> 
> ```shell
> mv [your-account-name]_miner_key.json ~/.lambda_miner/config/default_miner_key.json
> ```
> 
> 查询矿工子账户地址：
> 
> ```shell
> ./minernode show-address
> ```
> 
> 返回如下结果：
> 
> ```shell
> # 矿工账户地址
> Miner Address: lambda1lhhgvyaepf92mtx5zj49fseexr3g3njlz4jmgt (lambdamineroper1lhhgvyaepf92mtx5zj49fseexr3g3njlk67uak)
> # 矿工子账户地址
> Mining Address: lambda10m4xmmvwat9a53rf47pjjpn3tecdk64urd5qt9
> ```

- #### 给子账户转账

> ```shell
> ./lambdacli tx send [miningAddr] 1000000000ulamb --from [your-account-name] --broadcast-mode block -y
> ```
> 
> `[miningAddr] `为查询到的矿工子账户地址`（1LAMB=1000000ulamb）`。

- #### 创建矿工

> 查询矿工配置信息：
> 
> ```shell
> ./minernode info
> ```
> 
> 返回结果：
> 
> ```shell
> version: 0.2.5
>                 dht id: G4xW3UHMfFnTmaRMZUJ7PKcfvr9YTTFyekcsRxKDZZD9
> server.private_address: 172.17.159.130:15001
>         server.address: 0.0.0.0:26654
>   kad.external_address: 39.106.153.62:26654
>     kad.bootstrap_addr: [39.106.153.62:26650 172.17.159.130:26652]
>       Ensure-level = 0: 1/6 of disk-space would be used for data-replicating
> ```
> 
> 创建矿工：
> 
> ```shell
> ./lambdacli tx market create-miner --dht-id [dht-id] --mining-address [miningAddr] --from [miner-name] --broadcast-mode block -y
> ```
> 
> `[dht-id]`为查询到的`dht id`值；
> 
> `[miner-name] `是您的矿工账户名称；
> 
> `[miningAddr] `为矿工子账户地址。

- #### 启动矿工服务

> ```shell
> ./minernode run --query-interval 5 --daemonize --log.file [log_file_path]
> ```
> 
> `[log_file_path] `为矿工日志的完整路径。
> 
> 如果`[your-account-name]_miner_key.json`文件没有切换到`~/.lambda_miner/config/default_miner_key.json`，则加上`--key-file`参数启动：
> 
> ```shell
> ./minernode run --query-interval 5 --daemonize --log.file [log_file_path] --key-file [filepath/your-account-name]_miner_key.json
> ```

- #### 查看矿工服务进程

> ```shell
> ./minernode run --status
> ```
> 
> 返回如下结果则进程在运行中：
> 
> ```shell
> minernode.pid is running, pid is xxx
> ```

- #### 停止矿工服务

> ```shell
> ./minernode run --stop
> ```
> 
> 返回如下结果则停止矿工服务成功：
> 
> ```shell
> stop daemon process from minernode.pid:xxx successfully
> ```

5. ### 配置和启动`storagenode`
- #### 初始化`storagenode`及配置

> 1. 初始化`storagenode`
> 
> > ```shell
> > ./storagenode init
> > ```
> > 
> > 会生成配置文件`~/.lambda_storage/config/config.toml`。
> 
> 2. 修改配置文件
> 
> > ```shell
> > vim ~/.lambda_storage/config/config.toml
> > ```
> > 
> > 配置文件说明如下：
> > 
> > ```toml
> > [build]
> > version = "0.2.5"
> > commit = "030c696bc6829cfafb3d240d66058b16b41aa460"
> > mode = "release"
> > 
> > # 服务需要监听的地址
> > # 以本机内网IP为 192.168.10.20，端口映射的外网IP为 200.200.200.200 为例
> > [server]
> > # 对外提供服务的地址，推荐配置为内网地址做端口映射到外网IP
> > address = "192.168.10.20:14000"
> > # 对内提供服务的地址，主要是给StorageNode使用，推荐配置为内网地址
> > private_address = "192.168.10.20:14001"
> > 
> > [kad]
> > # address you want kad to connect with
> > # DHT接入节点地址，可以是自己质押的验证节点或minernode配置的kad.external_address，这里以 47.94.129.97:13000 为例
> > # 可选官方dht地址：39.105.148.217:13000/47.94.129.97:13000/47.93.196.236:13000/182.92.66.63:13000
> > bootstrap_addr = ["47.94.129.97:13000"]
> > # time you would wait to connect dht seed node
> > db_path = "/root/.lambda_storage/kademlia"
> > # this should listen at Public IP
> > external_address = "200.200.200.200:14000"
> > 
> > [log]
> > level = "info"
> > output_file = "stdout"
> > 
> > [storage]
> > ## 用于生成apikey的种子内容，不能为空
> > root_secret_seed = "fasdf"
> > ## 存储节点的名字，需要在矿池内部唯一，即连接同一矿工的多个storagenode的storage_name不能重复
> > storage_name = "machine1"
> > ## minernode对内提供服务的地址，即它的server.private_address
> > miner_address = "192.168.10.10:13001"
> > ## 存储路径，可填写多个以逗号隔开
> > data_dir = ["/root/.lambda_storage"]
> > ```

- #### 启动`storagenode`

> ```shell
> ./storagenode run --daemonize --log.file [log_file_path]
> ```
> 
> `--daemonize`指定以后台方式启动；
> 
> `--log.file [log_file_path]`指定`storagenode`运行日志路径，不添加参数则无日志输出。
> 
> 可添加`--log.level debug`参数，日志开启`debug`可查看更详细日志输出，不添加此参数则默认输出`INFO`级别日志。

- #### 查看`storagenode`进程

> ```shell
> ./storagenode run --status
> ```
> 
> 返回如下结果则进程正常运行：
> 
> ```shell
> storagenode.pid is running, pid is xxx
> ```

- #### 停止`storagenode`进程

> ```shell
> ./storagenode run --stop
> ```
> 
> 返回如下结果则进程停止成功：
> 
> ```shell
> stop daemon process from storagenode.pid:xxx successfully
> ```

6. ### 买单/卖单操作
- #### 卖单操作

> 在创建卖单命令中加上`--normal`参数（赔付比率为0.5）的是普通卖单，价格只能等于`5000000ulamb（1LAMB=1000000ulamb）`；不加`--normal`参数（赔付比率等于1）的为优质卖单，优质卖单可指定大于等于`5000000ulamb`的任意价格；
> 
> 设置需要卖出的空间大小`size`；
> 
> 最小购买空间`min-size`不能小于`1GB`；
> 
> 最小购买时长`min-buy-duration`不能小于`1month`；
> 
> 最大购买时长`max-buy-duration`不能大于`60month`。
> 
> 注意：测试网中尽量挂优质卖单（不加`--normal`参数），这样创建买单时才能指定卖单ID匹配到自己矿工的卖单。
> 
> 一个矿工可创建多笔卖单，卖单总空间不能大于质押`TBB`数量，例如：一个矿工质押了`1000000utbb`（即1TBB），创建卖单总空间不能超过`1TB`（即1000GB）。
> 
> 1. 创建普通卖单
> 
> ```shell
> ./lambdacli tx market create-sellorder --price 5000000ulamb  \
> --normal \
> --size [size]GB \
> --market-name LambdaMarket \
> --min-size [min-size]GB \
> --min-buy-duration [min-buy-duration]month \
> --max-buy-duration [max-buy-duration]month \
> --from [miner-name] --broadcast-mode block -y
> ```
> 
> 2. 创建优质卖单
> 
> ```shell
> ./lambdacli tx market create-sellorder --price [sellorder-price]  \
> --size [size]GB \
> --market-name LambdaMarket \
> --min-size [min-size]GB \
> --min-buy-duration [min-buy-duration]month \
> --max-buy-duration [max-buy-duration]month \
> --from [miner-name] --broadcast-mode block -y
> ```
> 
> 3. 查询卖单
> 
> ```shell
> # 查询账户地址
> ./lambdacli keys show [miner-name] --address
> # 查询卖单
> ./lambdacli query market miner-sellorders [address] [page] [limit]
> ```
> 
> 举例：
> 
> ```shell
> # 查看账户名为miner2的地址
> ./lambdacli keys show miner2 --address
> # 返回结果
> lambda1k6rxrmly7hz0ewh7gth2dj48mv3xs9yz8ffauw
> # 查询订单
> ./lambdacli query market miner-sellorders lambda1k6rxrmly7hz0ewh7gth2dj48mv3xs9yz8ffauw 1 10
> # 返回结果
> SellOrder
>   OrderId:            54F82FBD979BE150C8B3246D82DDF60F043129EE  //卖单ID，取消卖单或创建优质买单时需要用到此ID
>   Address:            lambdamineroper1k6rxrmly7hz0ewh7gth2dj48mv3xs9yznx96fn
>   Price:              5000000
>   Rate:               1.000000000000000000  //rate等于1，则该卖单为优质卖单
>   Amount:
>   SellSize:           400
>   UnUseSize:          0
>   Status:             1
>   CreateTime:         2019-11-04 12:02:24.379880755 +0000 UTC
>   CancelTimeDuration: 1h0m0s
>   MarketAddress:      lambdamarketoper1thj5fv8d0dsh3aealhpxm9mvgxjfh87srk3887
>   MachineName:        machine1
>   MinBuySize:         1
>   MinDuration:        720h0m0s
>   MaxDuration:        43200h0m0s
> ```
> 
> 4. 取消卖单
> 
> ```shell
> ./lambdacli tx market cancel-order [orderId] --from [miner-name] --broadcast-mode block -y
> ```
> 
> `[orderId]`为查询订单信息时返回的`OrderId`值；
> 
> `[miner-name]`为矿工账户名称。

- #### 买单操作

> 矿工不能买自己的卖单，只能换其他账户来挂买单。
> 
> 创建优质买单需要指定对应优质卖单`SellOrderID`。
> 
> `account-name`为当前账户的名称；
> `duration`为购买时长；
> `size`为需要购买的空间，不小于对应卖单指定的最小购买空间。
> 
> 1. 创建普通买单
> 
> ```shell
> ./lambdacli tx market create-buyorder --from [account-name] \
>  --duration [buy-duration]month --market-name LambdaMarket \
>  --size [size]GB --broadcast-mode block -y
> ```
> 
> 2. 创建优质买单
> 
>     `[orderId] `可指定1个或多个优质卖单ID，指定多个卖单ID时以逗号分隔。
> 
> ```shell
> ./lambdacli tx market create-buyorder --sellorder-id [orderId] \
> --from [your-account-name] --duration [buy-duration]month \
> --market-name LambdaMarket --size [size]GB --broadcast-mode block -y
> ```
> 
> 3. 查询匹配订单
> 
> ```shell
> # 查询账户地址
> ./lambdacli keys show [account-name] --address
> # 查看匹配订单
> ./lambdacli query market matchorders [address] [page] [limit]
> ```
> 
> 举例：
> 
> ```shell
> # 查看账户名为buyaccount的地址
> ./lambdacli keys show buyaccount --address
> # 返回结果
> lambda1thj5fv8d0dsh3aealhpxm9mvgxjfh87s224esr
> # 查看订单信息
> ./lambdacli query market matchorders lambda1thj5fv8d0dsh3aealhpxm9mvgxjfh87s224esr 1 10
> # 返回结果
> MatchOrder
>   OrderId:               05F09566BA4397BC9EB378EC202676D3FFCAF660   //匹配订单ID，上传文件时需要用到该ID
>   AskAddress:            lambdamineroper1k6rxrmly7hz0ewh7gth2dj48mv3xs9yznx96fn
>   BuyAddress:            lambda1thj5fv8d0dsh3aealhpxm9mvgxjfh87s224esr
>   SellOrderId:           58941CFFEEA859AED51172F0F9DF3E77293D2E12
>   BuyOrderId:            F3B5BDE351253E1D47DA7CEB24C0E4BAB5BDA808
>   Price:                 5000000
>   Size:                  20
>   CreateTime:            2019-11-01 13:20:58.296399278 +0000 UTC //匹配订单开始时间
>   EndTime:               2019-12-01 13:20:58.296399278 +0000 UTC //匹配订单结束时间
>   CancelTimeDuration:    1h0m0s
>   WithDrawTime:          2019-11-01 13:20:58.296399278 +0000 UTC
>   Status:                0
>   MarketAddress:         lambdamarketoper1thj5fv8d0dsh3aealhpxm9mvgxjfh87srk3887
>   MachineName:           machine1
>   UserPay:               100000000ulamb
>   MinerPay:              100000000ulamb
>   PubKey:                PubKeyEd25519{5AD2B4ECA8C33922A8228A5217900E5725BF3013639118D1002B6A413971F9DC}
>   PeerId:                bdd4da2a3021d30e8f215dba64716cec02cdb8a7
>   DhtId:                 5i6fXKQJoktPVmt9PAfZ18RN7DG6tghQN7SK7A3Bq4Rc
> ```

- #### 匹配订单续期

> `链0.4.8 - 存储0.2.5`版本 新增匹配订单续期功能。
> 
> - 匹配订单未到期的，购买了空间的账户可使用`lambdacli tx market order-renewal`命令续期。
> 
> - 匹配订单已过期的，不能再进行续期。
> 
> - 同一匹配订单可多次续期；
> 
> - 续期后的匹配订单总时长（即结束时间减开始时间），不能超过60个月（1个月=30天）。
> 
> 续期成功后，可进入[浏览器](http://testbrowser.lambda.im/#/)搜索匹配订单ID，查看`匹配订单详情页`中结束时间是否延期了对应时长。或使用上面查询匹配订单命令`lambdacli query market matchorders`查看返回结果中的匹配订单结束时间（即`EndTime`）是否延期了对应时长。
> 
> `[orderId]` 需要进行续期的匹配订单ID；
> `[duration]` 订单续期时长，单位为月。如设为3month，为续期3个月。
> 
> ```shell
> ./lambdacli tx market order-renewal [orderId] [duration] --from [account]
> ```
> 
> 举例：
> 
> ```shell
> # 账户buyaccount给自己购买的匹配订单0D3FAE471BFC92CED2AB7806E6AC648973357CAF 续期2个月
> ./lambdacli tx market order-renewal 0D3FAE471BFC92CED2AB7806E6AC648973357CAF 2month --from buyaccount --broadcast-mode block -y
> # 返回结果
> Response:
>   Height: 63
>   TxHash: 144EE614E02E1F4C347BEC08785A74E7F01411BEB6735FC668D25C23E078FEFD
>   Raw Log: [{"msg_index":"0","success":true,"log":""}]
>   Logs: [{"msg_index":0,"success":true,"log":""}]
>   GasWanted: 200000
>   GasUsed: 42848
>   Tags:
>     - action = orderRenewal
>     - address = lambda1jlh7644ghjjt72quxhraxt7aegj79pdr7unczs
> ```
> 
> 订单续期后，重新`storagecli token sync`后存储获取订单最新日期，通过`storagecli order list`可查看订单到期时间。
> 
> 举例：
> 
> ```shell
> # 获取订单信息
> ./storagecli token sync buy
> Password to sign with 'buy':
> sync orders begin, This may take some time...
> http://182.92.242.59:13659/market/user/matchorders/lambda1ejuhsxthm7kpjz63eczlg28prrfje9vd22ma3x
> Order                                              Total                Used
> 19FF9732F7E9069C689216173D3842612EDF02CC           6.0 GiB              101 MiB
> 293B8613B1E26A79F6554472645FACB809F4BAE8           30 GiB               7.9 GiB
> 29E301D07BDF6D22CE5813760F8F857326842C20           7.0 GiB              200 MiB
> 5A8E65C1C04177234DC8E7B7DFBCE98CC31134AC           1.0 GiB              677 MiB
> EC349DC8803BEE33B21E487A2481EB94CFC1F8DD           5.0 GiB              627 MiB
> sync orders finish
> # 查看订单到期时间
> ./storagecli order list buy
> Password to sign with 'buy':
> 
> OrderId                                  |Expire                  |Used/Total      |ProviderStatus
> 19FF9732F7E9069C689216173D3842612EDF02CC |2020-09-10 17:36:26 CST |100 MiB/6.0 GiB |Avaialable
> 293B8613B1E26A79F6554472645FACB809F4BAE8 |2020-09-24 18:23:49 CST |7.9 GiB/30 GiB  |Avaialable
> 29E301D07BDF6D22CE5813760F8F857326842C20 |2020-07-12 17:37:06 CST |200 MiB/7.0 GiB |Avaialable
> 5A8E65C1C04177234DC8E7B7DFBCE98CC31134AC |2020-09-29 09:48:16 CST |50 MiB/1.0 GiB  |Avaialable
> EC349DC8803BEE33B21E487A2481EB94CFC1F8DD |2020-08-11 17:35:00 CST |150 MiB/5.0 GiB |Avaialable
> Total: 5
> Current time: 2020-04-14 17:58:49 CST
> ```

## 二、文件上传和查看

1. ### 配置

> 1. 初始化`storagecli`
> 
> > ```shell
> > ./storagecli init
> > ```
> > 
> > 会生成配置文件`~/.lambda_storagecli/config/user.toml`。
> > 
> > 注意：如果已经备份了旧版`storagecli`的配置文件，覆盖`~/.lambda_storagecli/config/user.toml`即可。
> 
> 2. 修改配置文件
> 
> > ```shell
> > vim ~/.lambda_storagecli/config/user.toml
> > ```
> > 
> > 配置文件说明如下：
> > 
> > ```toml
> > [broker]
> > # dht_gateway_addr为验证节点的dht服务 IP和端口；
> > # 可以是自己质押的验证节点配置的kad.external_address，这里以 47.94.129.97:13000 为例
> > # 可选官方dht地址：39.105.148.217:13000/47.94.129.97:13000/47.93.196.236:13000/182.92.66.63:13000
> > dht_gateway_addr = "39.105.148.217:13000" 
> > # validator_addr为验证节点IP和端口，可以是自己质押的验证节点rest-server服务指定的laddr地址
> > # 可选官方地址：39.105.148.217:13659/47.94.129.97:13659/47.93.196.236:13659
> > validator_addr = "39.105.148.217:13659"   
> > 
> > [gateway]
> > # local listen address
> > address = "0.0.0.0:9002"
> > # for login web UI
> > access_key = "accesskey"
> > secret_key = "secretkey"
> > ```
> 
> 3. 同步订单token
> 
> > ```shell
> > ./storagecli token sync [account-name]
> > ```
> > 
> > `[account-name]`为买单账户名称。

2. ### 文件操作

    文本/图片/视频/音频/可执行文件/压缩包文件可正常上传。上传源文件路径为绝对路径。

    `orderId`为匹配单`orderID`；

    `account-name`为发起买单账户名称；

    `bucket-name` 可设置为长度不小于3且不大于64位的英文大小写，一个订单下可以有多个bucket。

- #### 上传文件

> 1. 创建`bucket`
> 
> ```shell
> LAMBDA_ORDER_ID=[orderId] ./storagecli mb [account-name] lamb://[bucket-name]/
> ```
> 
> 2. 上传文件
> 
> ```shell
> LAMBDA_ORDER_ID=[orderId] ./storagecli cp [account-name] [srcPath] lamb://[bucket-name]/ 
> ```

- #### 查看上传文件列表

> ```shell
> LAMBDA_ORDER_ID=[orderId] ./storagecli ls lamb://[bucket-name]/ 
> ```

3. ### 查看订单和矿工状态

> 用法：
> 
> ```shell
> ./storagecli order list [account-name]
> ```
> 
> 举例：
> 
> ```shell
> ./storagecli order list myaccount 
> 
> OrderId                                  |Expire           |Used/Total      |ProviderStatus
> 81B7663F9D5F40A37F4875FC1B95E2C5E1CD7FEA |2020-04-24 09:00 |100 GiB/100 GiB |Maintaining
> EF667304E33C6AAB9D56F04DF878FD93A5153B6D |2020-04-24 09:00 |100 GiB/100 GiB |Invalid
> Total: 2
> ```
> 
> ProviderStatus为矿工状态，Avaialable为正常状态，Maintaining矿工正在维护，Invalid 矿工失效。

## 三、文件分享

    一个账户可以通过分享命令给另一个存储账户进行分享文件。接收者可以在分享者分享的期限内下载文件。

- ### 分享文件

> `[remote path]`为分享文件的文件地址，可以分享整个文件夹，不加具体文件即可；
> 
> `[duration]`分享文件的期限，Y:年 M:月 d:天 h:小时 m:分 s:秒（8M7h6m, 代表: 8个月 + 7小时 + 6分）。
> 
> 用法：
> 
> ```shell
> LAMBDA_ORDER_ID=[orderId] ./storagecli token share ACCOUNT [remote path] [duration] --download  [flags]
> ```
> 
> 举例：
> 
> ```shell
> LAMBDA_ORDER_ID=5A8E65C1C04177234DC8E7B7DFBCE98CC31134AC ./storagecli token share buy lamb://test/file50_e477d42cadc445049507f215142be187  1M2h3m4s  --download
> Password to sign with 'buy':
> create share token with these properties:
> share duration: 722h3m4s
> share path: lamb://test/file50_e477d42cadc445049507f215142be187
> share type: [download]
> please wait a few seconds
> got share token secret:
> 2b7nqoMMBrKzEUrvz44yFWHUk3xRpBYtvy7seKpwjVdJz2iAnhBpJMiXghhkrXLqPD
> ```
> 
> 执行命令后会输出`got share token secret`，接受者用来接收文件。

- ### 接收分享文件

> `ACCOUN`为接收账户。
> 
> 注意：` --secret`是必填的flag。
> 
> 用法：
> 
> ```shell
> LAMBDA_ORDER_ID=[orderId] ./storagecli token restore ACCOUNT   [flags] 
> ```
> 
> 举例：
> 
> ```shell
> LAMBDA_ORDER_ID=2E5A78E1564E7D220C327B1EC4F7087AD7CF2708  ./storagecli token restore teshare --secret 3gyjicaEhiNa8i8pighP6gbnVZLxAkqfBCQUgv9SAmQLu7453zgvyb48BzMcvouUUw  
> http://182.92.242.59:13659/market/user/matchorders/lambda1ejuhsxthm7kpjz63eczlg28prrfje9vd22ma3x
> file download keys nums 1
> ```

- ### 接收并下载分享文件

    接收并下载分享文件，可以省略接收文件这一步骤。

> `ACCOUNT`为接收账户；
> 
> `[srcPath]`为下载文件地址；
> 
> `[dstPath]`为下载后放文件地址。
> 
> 注意：`--secret`是必填的flag。
> 
> 用法：
> 
> ```shell
> LAMBDA_ORDER_ID=[orderId] ./storagecli cp ACCOUNT [srcPath] [dstPath] [flags]
> ```
> 
> 举例：
> 
> ```shell
> LAMBDA_ORDER_ID=2E5A78E1564E7D220C327B1EC4F7087AD7CF2708 ./storagecli cp  teshare lamb://test/testfiles.tar.gz_414c7b9aa8154c268220d93a8b8a131f  /root/qwe/ --secret 3gyjicaEhiNa8i8pighP6gbnVZLxAkqfBCQUgv9SAmQLu7453zgvyb48BzMcvouUUw  
> http://182.92.242.59:13659/market/user/matchorders/lambda1ejuhsxthm7kpjz63eczlg28prrfje9vd22ma3x
> file download keys nums 1
> duplicate token.
> found only one candicate
> downloading testfiles.tar.gz_414c7b9aa8154c268220d93a8b8a131f
> 492347394 / 492347392 [---------------------------------------------------------------------------------------------------------------------------------------------------] 100.00% 12099253 p/s
> download done 410289495
> ```

- ### 下载分享文件

    下载分享文件需要先接收分享文件，再下载分享文件。

> `ACCOUNT`为接收账户；
> `[srcPath]`为下载文件地址。
> 
> 用法：
> 
> ```shell
> LAMBDA_ORDER_ID=[orderId] ./storagecli cp ACCOUNT lamb://[bucket-name]/  [srcPath] [flags]
> ```
> 
> 举例：
> 
> ```shell
> LAMBDA_ORDER_ID=92F1918765F3654EE1E4F98BD64B96CB4DD4C0BC  ./storagecli cp  teshare lamb://test/upload.tar.gz_3484a737e325439b80ef79cb1297d3a2  /root/qwe/ 
> found only one candicate
> downloading upload.tar.gz_3484a737e325439b80ef79cb1297d3a2
> 1400395776 / 1400395752 [-------------------------------------------------------------------------------------------------------------------------------------------------] 100.00% 11287511 p/s
> download done 1166996480
> ```

## 四、挖矿

- ### 矿工挖矿

> 矿工每接受文件`8GB`文件会对应生成一个声明，所有声明有效期为1个月，每个出块周期（大约每6s出一个块）会由共识网络发起挑战，挑战声明成功并提交挖矿证明的矿工就会得到一笔收益。
> 
> 1. 有有效声明的矿工有挖矿权利；
> 
> 2. 矿工已存文件每满8G可生成1个声明；
> 
> 3. 单个矿工声明越多，该矿工被挑选到的概率越大。

- ### 挖矿收益

> 矿工收益 = 单个区块增发量 * （43% * 95% / 挖矿的矿工数量） + 单个区块增发量 * （43% * 5% / 挖矿的矿工数量 ）* 矿工质押量在节点质押量的占比 * 75%
> 
> 比如单个区块增发量为100LAMB，全网共1个节点该节点质押666TBB，全网共10个矿工分别质押1TBB且分别有10个声明。则单个矿工单个区块得到的挖矿奖励为：
> 
> ```textile
> 4.085LAMB= 100LAMB * 43% * 95% / 10 + 100LAMB * 43% * 5% / 10 * 1/676 * 75%
> ```

## 五、排错

    当启动`miner`、`storagenode`、或上传文件时有报错，请使用以下命令进行排查。

- ### 测试`miner`服务

> 命令：
> 
> ```shell
> ./minernode info --test
> ```
> 
> 返回结果均为`success`即正常：
> 
> ```shell
> version: 0.2.5
>                 dht id: G4xW3UHMfFnTmaRMZUJ7PKcfvr9YTTFyekcsRxKDZZD9
> server.private_address: 172.17.159.130:15001   successful
>         server.address: 0.0.0.0:26654    successful
>   kad.external_address: 39.106.153.62:26654    successful
>     kad.bootstrap_addr: [39.106.153.62:26650 172.17.159.130:26652]    successful successful
>       Ensure-level = 0: 1/6 of disk-space would be used for data-replicating
> ```

- ### 测试`storagenode`服务

> 命令：
> 
> ```shell
> ./storagenode info network --test
> ```
> 
> 返回结果均为`success`即正常：
> 
> ```shell
> version: 0.2.5
>                 dht id: 3mta4YEgHB43RHYE83aWBouvFNNCtSc832siEwmcTUsZ
>   storage.storage_name: sn1
>  storage.miner_address: 172.17.159.130:15001   successful
> server.private_address: 172.17.159.130:16001   successful
>         server.address: 0.0.0.0:26660    successful
>   kad.external_address: 39.106.153.62:26660    successful
>     kad.bootstrap_addr: [172.17.159.130:26650 172.17.159.130:26652]     successful successful
> ```

- ### 查看存储节点磁盘空间

> 命令：
> 
> ```shell
> ./storagenode info disk
> ```
> 
> 返回结果：
> 
> ```shell
> version:  0.2.5
>   storage.storage_name:  sn1
>       storage.data_dir:  [/lambda/data/xvdd/store /lambda/data/xvde/store /lambda/data/xvdc/中文test/store /lambda/.1lambda_storage/store]
> 
> Disk                           |Total  |Used    |Free    |Order                                    |Reserved |Occupied
> /lambda/data/xvdd/store        |18 GiB |8.9 GiB |8.8 GiB |753E54547CC66DB840E6C717C98492640B6E5CF8 |3.0 GiB  |540 MiB
>                                |       |        |        |E15F0CCA09A8F92E401E322638CA777BC9EA24B8 |3.0 GiB  |1.9 GiB
> /lambda/data/xvde/store        |18 GiB |592 MiB |17 GiB  |D3280F0343112CC35B864CFFEE96DE3D2F39F3C7 |12 GiB   |579 MiB
> /lambda/data/xvdc/中文test/store |18 GiB |13 GiB  |4.6 GiB |DBE8C6D465D1701E71A7CBDF35E9F602A9CE55AE |6.0 GiB  |3.8 GiB
> ```
> 
> Reserved为订单预留的磁盘空间，Occupied为当前订单实际占用磁盘空间。
