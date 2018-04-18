*下载解压编译测试*
```
$ wget http://download.redis.io/releases/redis-4.0.2.tar.gz
$ tar xzf redis-4.0.2.tar.gz
$ cd redis-4.0.2
$ make
$ make test
```
*启动Redis*
```
$ src/redis-server

```
*客户端/命令行连接Redis*
```
$ src/redis-cli -h host

```
*连接后-->查看当前配置的密码*
```
$ config get requirepass

```
*连接后-->配置Redis的连接密码*
```
$ config set requirepass yourpasssword
```
*连接前-->在配置文件中配置Redis的连接密码; 在redis.conf中增加配置:*
```
requirepass yourpassword

```
*重启Redis*    **注意: 此时启动时要指定刚才的配置文件redis.conf，命令如下:**
```
$ src/redis-server redis.conf   #此时再连接后就发现需要密码
```


*快照基本配置*
```
#900秒内有1次被写入
save 900 1 
#300秒内有10次被写入  
save 300 10 
#60秒内有10000次被写入 
save 60 10000  

#在保存快照出错时是否让Redis正常的进行写操作
stop-writes-on-bgsave-error yes  
#Compress string objects using LZF when dump .rdb databases是否在导出.rdb数据库文件时采用LZF压缩字符串和对象 | no : 节省cpu,但是对象过大;  yes:压缩体积变小,但耗费CPU
rdbcompression yes  

#从版本RDB版本5开始，一个CRC64的校验就被放在了文件末尾。    
#这会让格式更加耐攻击，但是当存储或者加载rbd文件的时候会有一个10%左右的性能下降，    
#所以，为了达到性能的最大化，你可以关掉这个配置项。  
rdbchecksum    yes

#导出的快照文件的名称/The filename where to dump the DB
dbfilename     dump6666.rdb
#导出的快照文件的存储路径/The working directory， the Append Only File 也会存在这个路径下，这里必须是一个路径而不是一个文件
dir            /work/redis6666
```

*复制的基本配置* [主从环境下会使用该配置,集群还不清楚]
```
#作为从库,连接主库的ip和端口
slaveof <masterip> <masterport>

#在从库中配置主库密码,如果主库设置了密码，那么从库就要配置该项，否则连接会被拒绝
masterauth <master-password>

#在使用主从时，如果从库和主库失去了连接或者复制正在进行中，yes选项表示:从库依旧可以提供服务，只是返回的可能是过期的数据或者null | no 选项表示:从库不提供服务，抛出一个错误SYNC with master in progress"给所有的请求but to INFO and SLAVEOF
slave-serve-stale-data yes

#Since Redis 2.6 by default slaves are read-only
slave-read-only yes

#复制的两种策略:disk or socket.WARNING: DISKLESS REPLICATION IS EXPERIMENTAL CURRENTLY
repl-diskless-sync no

#使用DISKLESS时用到
repl-diskless-sync-delay 5

#从库向主库发送心跳的周期,单位秒
repl-ping-slave-period 10

#复制的超时时间
repl-timeout 60

```