## MySQL安装

### 1 Windows安装MySQL

Windows下安装MySQL有两种方式，一种是下载msi格式的安装文件，另一种是下载zip格式的压缩包。msi的安装比较简单，基本就是一路next到底。因此，这里只讲zip格式的安装。

[ftp://mirror.switch.ch/mirror/mysql/Downloads/](ftp://mirror.switch.ch/mirror/mysql/Downloads/)

从上面的FTP下载合适的版本的zip压缩包。例如，这里下载win32平台的5.6.23版本的MySQL:mysql-5.6.23-win32.zip。

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
update mysql.user set password=PASSWORD('YOUR_PASSWD') where user='root';
flush privileges;
```

### 2 Ubuntu(Linux)安装MySQL

1 同样的，从上述的FTP下载合适的版本的tar.gz压缩包。例如，这里下载的是5.6.22版本的MySQL：mysql-5.6.22-linux-glibc2.5-i686.tar.gz。

2 将压缩包解压：

```
tar -zxvf mysql-5.6.22-linux-glibc2.5-i686.tar.gz
```

然后将解压后的包拷贝到安装目录，这里是/usr/local/目录，并重命名为mysql。

3 当然，也可以下载通用版本：例如，mysql-5.6.22.tar.gz。

将它解压到安装目录下，然后进行编译：

执行BUILD目录下的autorun.sh，就会在mysql的根目录下生成configure文件，接下来就是安装三部曲了。

```
./configure
make
sudo make install
```

4 添加mysql用户组和用户

```
sudo groupadd mysql
sudo useradd -r -g mysql mysql
```

5 修改安装目录的所有者

cd /usr/local/mysql

chown -R root:mysql .　//把当前目录中所有文件的所有者所有者设为root，所属组为mysql

chown -R mysql:mysql data

6 创建系统数据库的表

```
sudo scripts/mysql_install_db --user=mysql
```

7 启动与关闭mysql

```
// 启动mysql
sudo ./bin/mysqld_safe --user=mysql &

// 关闭mysql
sudo mysqladmin -u root -p shutdown
```

8 设置root密码。mysql -uroot登录mysql，设置密码的方式与windows一样。

9 将mysql的bin目录加入到PATH中。编辑当前用户的.bashrc文件，在文件尾部添加：

```
export PATH=$PATH:/usr/local/mysql/bin
```

然后使之生效：

```
source ./.bashrc
```

10 设置mysql服务自启动。
