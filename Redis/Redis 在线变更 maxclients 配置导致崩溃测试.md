# Redis 在线变更 maxclients 配置导致崩溃测试

## 一 现象

> Redis 在线更改 maxclients 的现象 **不能重现**。

## 二 测试版本

* 2.8.6
* 2.8.19
* 2.8.24（2.8 大版本最新版）
* 3.0.0（3.0 版本）
* 3.2.7（最新稳定版）

## 三 测试流程

* maxclients 预设配置 1000，在线更改到 2000、3000、10000 均正常
* 怀疑没有数据量测不出效果，于是批量插入 300 万 Key，占用内存 300M，在线更改正常
* 再次怀疑没有连接同样测试不出效果，于是用 pconnect 模拟 1000 个连接，在线更改正常

## 四 参考

* [redis crash under the add maxclients](https://github.com/antirez/redis/issues/3412)
* [CONFIG SET maxclients](https://github.com/antirez/redis/issues/464)

## 五 结论

在线变更 maxclients 配置 **有可能导致崩溃**，但不清楚触发条件。**线上务必谨慎使用。**

## 六 附件

### 6.1 测试脚本

**批量插入数据**

bulk_insert.sh

``` bash
#!/bin/bash
for ((i=0;i<3000000;i++))
do
echo -en "helloworld" | /usr/local/redis/bin/2.8.6/redis-cli -h 127.0.0.1 -p 6379 -x set name$i >> redis.log
done
```

**PHP 模拟并发**

link_redis.php

``` php
<?php
    set_time_limit (0);
    $redis = new redis();
    $redis->pconnect('127.0.0.1', 6379);
    sleep(100);
?>
```

max_redis.php

``` php
<?php
	set_time_limit (0);

	for($i=1;$i<=1050;$i++){
    	exec("nohup php /data/tmp/link_redis.php > /dev/null &");
	}
?>
```

### 6.2 配置文件

``` bash
daemonize yes

pidfile /var/run/redis6379.pid

port 6379

bind 127.0.0.1 10.4.4.233

timeout 300

tcp-keepalive 0

loglevel notice

logfile stdout
logfile /data/redis/6379/log/redis6379.log

databases 16

#save 900 1
#save 300 10
#save 60 10000

maxmemory 1073741824
maxmemory-policy volatile-lru
stop-writes-on-bgsave-error yes

rdbcompression yes

rdbchecksum yes

dbfilename dump.rdb

dir /data/redis/6379/data

slave-serve-stale-data yes

slave-read-only yes

repl-disable-tcp-nodelay no

slave-priority 100

appendonly no

appendfsync everysec

no-appendfsync-on-rewrite no

auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

lua-time-limit 5000

slowlog-log-slower-than 10000

slowlog-max-len 128

hash-max-ziplist-entries 512
hash-max-ziplist-value 64

list-max-ziplist-entries 512
list-max-ziplist-value 64

set-max-intset-entries 512

zset-max-ziplist-entries 128
zset-max-ziplist-value 64

activerehashing yes

client-output-buffer-limit normal 0 0 0
client-output-buffer-limit slave 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60

hz 10

maxclients 1000

aof-rewrite-incremental-fsync yes
rename-command FLUSHDB ""
rename-command FLUSHALL ""
```

Enjoy!

© Robin Wen. 2017/02/07
