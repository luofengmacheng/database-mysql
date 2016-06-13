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