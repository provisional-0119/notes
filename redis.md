## 版本号规则

如果第二位是奇数，则是非稳定版本，如2.7，2.9，3.1

如果第二位是偶数，则是稳定版本，如2.6，2.8，3.0，3.2

## Redis安装

#### 安装准备

必须是64位虚拟机

需GCC编译环境（因为redis是c语言写的）

#### 安装

1. 传输redis压缩包到/opt目录下，并使用tar 命令解压

2. 使用`make && make install` 命令

​	make命令用于编译Redis源代码，生成可执行文件。

​	这将在当前目录下编译Redis源代码，并生成redis-server和redis-cli等可执行文件。

​	make install命令用于将编译好的可执行文件安装到系统中，默认安装到usr/local/bin中。

​	生成的可执行文件：

```
redis-benchmark：Redis的性能测试工具，用于测试Redis服务器的性能和吞吐量。
redis-check-aof：Redis的AOF文件检查工具，用于检查AOF文件的完整性和正确性。
redis-check-rdb：Redis的RDB文件检查工具，用于检查RDB文件的完整性和正确性。
redis-cli：Redis的命令行客户端工具，用于连接和操作Redis服务器。
redis-sentinel：Redis的哨兵进程，用于监控和管理Redis服务器的高可用性。
redis-server：Redis的服务器进程，用于启动和运行Redis数据库服务。
```

3. 复制并修改redis.conf，以修改后的配置文件启动服务器

   ```
   daemonize no  ->  daemonize yes
   protected-mode yes -> protected-mode no
   bind 127.0.0.1 -::1 -> 注释掉
   设置密码：去掉requirepass 的注释，添加密码，个人当前为123456
   ```

   启动后默认端口为6379

   启动后使用redis-cli连接，可以添加`-a <password>`参数，声明密码，

   ​	或者在执行redis-cli命令后，使用`auth <password>`声明密码。

   ​	如果不是6379端口，需要`-p`声明，例如：`redis-cli -a 123456 -p <port>`

 4. 关闭服务器

    如果在redis-cli状态，直接`shutdown`；

    如果在普通shell状态，则`redis-cli [-a <password>] [-p <ports>] shutdown` 

5. 卸载redis

   在`/usr/local/bin`目录下查找所有redis的可执行文件：`ls -l /usr/local/bin/redis-*`

   删除所有可执行文件：`rm -rf /usr/local/bin/redis-*`

## Redis数据类型

#### 常用key操作

1. `keys <pattern>` : pattern支持通配符，例如：`keys *` ：查询所有key， `keys user*` ：查询所有以user开头的key
2. `exists <key>` ：判断是否存在key
3. `type <key>` ：查看key是什么类型
4. `del <key>` : 删除key
5. `unlink <key>` ：非阻塞删除key，仅仅将key从keyspace元数据中删除，真正的删除在后续的异步操作中进行
6. `ttl <key>` ：查看key还有多久过期，-1代表永不过期，-2代表已过期
7. `expire <key> <second>` ：为key设置过期时间，以秒为单位
8. `move <key> <dbIndex>` : 将key移到<dbIndex>号库  (默认库编号为0-15，由redis.conf中的databases配置，默认值为16)
9. `select <dbIndex>` : 切换库为<dbIndex>号库
10. `dbsize`：查看当前库key数量
11. `flushdb`：清空当前库
12. `flushall` ：清空所有库

注意：命令不区分大小写，但是key区分大小写

​		帮助命令： `help [@<type>|command]` , 例如 `help @string`，`help keys`

