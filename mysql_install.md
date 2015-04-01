## MySQL安装

### 1 Windows安装MySQL

Windows下安装MySQL有两种方式，一种是下载msi格式的安装文件，另一种是下载zip格式的压缩包。msi的安装比较简单，基本就是一路next到底。因此，这里只讲zip格式的安装。

从[ftp://mirror.switch.ch/mirror/mysql/Downloads/](ftp://mirror.switch.ch/mirror/mysql/Downloads/)下载合适的版本的zip压缩包。

例如，这里下载win32平台的5.6.23版本的MySQL:mysql-5.6.23-win32.zip。

1 将该压缩包解压到安装路径。例如，有的人喜欢讲软件安装在C:\Program Files路径下，但是该路径下安装的软件较多时，会导致系统启动较慢，因此，我就喜欢讲软件安装在D:\Program Files目录下。于是，可以将该压缩包解压到D:\Program Files目录。

2 添加环境变量。计算机->属性->高级系统设置->环境变量。新建MYSQL_HOME，值为软件的路径名，这里就是D:\Program Files\mysql-5.6.23-win32。然后在Path后面添加`;%MYSQL_HOME%\bin`。

3 在软件的根目录下有个my-default.ini文件，它是mysql的配置文件。修改该文件，并另存为my.ini保存在软件根目录中。

```
[mysqld]
loose-default-character-set = utf8
character-set-server = utf8
basedir = D:\Program Files\mysql-5.6.23-win32
datadir = D:\Program Files\mysql-5.6.23-win32\data

[client]  
#设置客户端字符集  
loose-default-character-set = utf8

[WinMySQLadmin]  
Server = D:\Program Files\mysql-5.6.23-win32\bin\mysqld.exe
```

3 开始安装。进入C:\Windows\System32，查找cmd.exe，右键“以管理员身份运行”，然后输入mysqld -install即将mysql安装到windos的服务中。

4 启动服务。net start mysql启动mysql服务，然后就可以执行mysql了。

5 设置密码。经过以上步骤之后，虽然可以访问mysql中的数据库，但是只能看到部分数据库，而且不能添加数据库和表。这时需要为用户设置登录密码。下面的语句设置root用户的密码为YOUR_PASSWD。

```
update mysql.user set password=PASSWORD('YOUR_PASSWD') where User='root'
flush privileges
```

### 2 Ubuntu(Linux)安装MySQL