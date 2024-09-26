mongodb是一种非关系型数据库，非常方便存储和作为中间件

本实验将mongod下载到`/usr/local`下，此处一般需要**赋权**才可以，出问题注意检查`sudo chmod`没有

## 1.获取mongod 和 mongosh两个主要的文件
> Note：从mongod6开始，图像化的界面与mongod服务端拆分，需要单独下载sh版本在linux上使用，或者使用pymongod进行处理
>

```shell
# mongod可执行文件下载
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu2204-8.0.0.tgz
# mongosh可执行文件下载
wget https://downloads.mongodb.com/compass/mongosh-2.3.1-linux-x64.tgz

# 解压授权
sudo tar -zxvf <your mongod path>.gz.tar
sudo tar -zxvf <your mongosh path>.gz.tar

sudo chmod -R 777  <your mongod path>
sudo chmod -R 777  <your mongosh path>
```

## 2.设置环境以及mongod配置
### 2.1编辑用户环境或者系统环境
```shell
# Part1：mongod的配置
sudo vim ~/.bashrc 
sudo vim /etc/profile

# 加入相关环境变量
export MONGODB_HOME=/home/ubuntu/mongodb
export PATH=$MONGODB_HOME/bin:$PATH

# 刷新变量
source ~/.bashrc
source /etc/profile

#Part2：mongosh的配置
# 在mongod配置并启动后可以开启mongosh，会自动连接到本地的mongod服务，需要将二进制文件和链接库拿到/usr/bin中
sudo cp <your mongosh path>/bin/mongosh /usr/bin
sudo cp <your mongosh path>/bin/mongosh_crypt_v1.so /usr/lib
```

### 2.2 mongod的conf编辑
```nginx
systemLog:
 destination: file
 path: /home/ubuntu/mongodb/logs/mongodb.log #存放日志的文件
 logAppend: true  # 设置为追加模式
 quiet: true
 verbosity: 0     # 设置的日志级别，默认4个, 0 1 2 3, 0仅记录重要的信息和错误
storage:
 dbPath: /home/ubuntu/mongodb/data/  # 存放数据库的文件位置
net:
 bindIp: 0.0.0.0 # 绑定的IP
 port: 27017  	 # 默认启动进程的端口
 maxIncomingConnections: 5000
processManagement:
 fork: true # 挂一个子进程在后台运行
#security:  # 安全配置
 # authorization: enabled # 开启后用户必须获取授权才能访问数据库
```

### 2.3 测试启动mongod服务并配置全局守护进程
+ 简单测试，使用命令后会返回`successful`信息

`<your-mongod-path>/bin/mongod -f <your-mongod-path>/bin/mongod.conf`

+ 配置守护进程
    - 默认从存放符号链接的目录`/etc/systemd/system/`读取文件，源文件存储在`/usr/lib/systemd/system/`中
    - `systemctl enable`用于在两个目录之间建立链接联系
    - `service`文件一般定义三个单元`[Unit]`、`[Service]`、`[Install]`

```shell
[Unit]
Description:描述，
After：在network.target,auditd.service启动后才启动
ConditionPathExists: 执行条件
[Service]
EnvironmentFile:变量所在文件
ExecStart: 执行启动脚本
Restart: fail时重启
[Install]
Alias:服务别名
WangtedBy: 多用户模式下需要的
```

+ mongodb配置文件参考

```shell
[Unit]
Description=mongodb
After=network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
ExecStart=<your-mongod-path>/bin/mongod --config <your-mongod-path>/bin/mongod.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=<your-mongod-path>/bin/mongod --config <your-mongod-path>/bin/mongod.conf --shutdown
PrivateTmp=true
[Install]
WantedBy=multi-user.target
```

    - 重新加载配置
        * `systemctl daemon-reload`
        * `systemctl start mongodb`
        * `systemctl status mongodb`
        * `systemctl enable mongodb`



## 3.Mongodb的角色信息
+ 数据库用户角色：`read`、`readWrite`
+ 数据库管理角色：`dbAdmin`、`dbOwner`、`userAdmin`
+ 集群管理角色：`clusterAdmin`、`clusterManager`、`clusterMonitor`、`hostManager`
+ 备份恢复角色：`backup`、`restore`
+ 所有数据库角色：`readAnyDatabase`、`readWriteAnyDatabase`、`userAdminAnyDatabase`、`dbAdminAnyDatabase`
+ 超级用户角色：`root`

**NOTE：在创建角色的时候可以指定给不同的权限**

## 4.常用的命令选项
+ `mongdb`
+ `reference`： [https://www.mongodb.com/zh-cn/docs/mongodb-shell/run-commands/](https://www.mongodb.com/zh-cn/docs/mongodb-shell/run-commands/)



