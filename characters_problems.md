## MySQL中的编码问题

### 1 变量中的字符集配置的含义

* character_set_client        客户端请求数据使用的字符集
* character_set_connection    客户端/服务器连接的字符集
* character_set_results       返回给客户端的结果集的字符集
* character_set_database      数据库使用的字符集，如果没有切换到数据库，则该配置应该与character_set_server一致
* character_set_server        数据库服务器的默认字符集

* character_set_filesystem    把os上文件名转化成次字符集，默认binary不做任何转换
* character_set_system        系统字符集，总是utf8，不能更改，用于数据库对象的名字，也用于存储在目录表中的函数的名字

### 2 MySQL执行SQL过程中字符集的转换

1 Server收到请求后，character_set_client => character_set_connection
2 Server内部操作，character_set_connection => 表创建的字符集
3 生成结果集，表创建的字符集 => character_set_connection => character_set_results

而执行`SET NAMES utf8`则将`character_set_client`、`character_set_connection`、`character_set_results`都设置为utf8。

### 3 配置文件中编码的设置

#### 3.1 配置文件的查找顺序

`mysql --help | grep my.cnf`可以用于查看配置文件的查找顺序，mysql会从前到后查找这些配置文件。

#### 3.2 配置文件的分组

MySQL的配置文件分成很多组，在这些组中，[client]是比较特殊的一种，该配置会被所有的MySQL客户端应用读取，也就是说，该配置会被除了mysqld服务器程序之外的所有客户端程序读取，而其它的组都是给对应的程序使用的，例如[mysql]是给mysql程序读取的，[mysqld]是给mysqld程序使用的。

#### 3.3 编码设置

在my.cnf配置文件中可以设置client、mysqld、mysql的默认编码为utf8：

```
default-character-set=utf8
```

### 4 小结

* 尽量保持character_set_client、character_set_connection、character_set_results、character_set_database一致，都是utf8
* 在程序中连接数据库时，一定要指定编码(语言的库中一般都会提供设置编码的功能)
* 如果实在程序中不方便设置编码，可以对数据库添加`init_connect`配置，在此配置中加上`set names utf8`
* 大范围的编码转小范围的编码是可以的，小范围的编码转大范围的编码可能会丢失字符的含义