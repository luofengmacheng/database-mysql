## 其它问题集锦

### 1 Windows下重置密码

* 关闭mysql：net stop mysql
* 进入mysql的bin目录，以安全模式启动mysql：mysqld --skip-grant-tables
* 重新打开一个窗口，登录mysql：mysql
* 重置密码：

```
use mysql;
update user set Password=password('123456') where User='root';
flush privileges;
```
* 在任务管理器中关闭所有mysql的进程，然后启动mysql：net start mysql

### 2 1093: Table 'sInstances' is specified twice, both as a target for 'UPDATE' and as a separate source for data

执行以下SQL报错：

```
UPDATE sInstances SET ip_region = (SELECT ip_region FROM sInstances WHERE ip = '10.209.24.19' AND ip_region != 0 LIMIT 1) WHERE ip = '10.209.24.19' AND ip_region = 0;
```

此时可以用JOIN解决：

```
UPDATE sInstances A LEFT JOIN sInstances B ON A.ip = B.ip SET A.ip_region = B.ip_region WHERE A.ip = '10.209.24.19' AND A.ip_region = 0 AND B.ip_region !=0;
```