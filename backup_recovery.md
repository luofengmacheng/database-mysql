## MySQL的备份与恢复

### 1 备份/恢复策略

1 定期做MySQL备份，并考虑系统可以承受的恢复时间。

2 确保MySQL打开log-bin，有了二进制日志，MySQL才可以在必要的时候做完整恢复，或基于时间点的恢复，或基于位置的恢复。注：如何打开二进制日志呢？

Linux:使用--log-bin选项启动服务器。

Windows:修改MySQL的选项文件my.ini，在[mysqld]下设置log-bin的目录即可。如：

```
[mysqld]

# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M

# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
log_bin="D:\Program Files\mysql-5.6.23-win32\data"
```

3 经常做备份恢复测试，确保备份是有效的，并且是可以恢复的。

### 2 冷备份

备份；

(1) 停掉MySQL服务，在操作系统级别备份MySQL的数据文件。

(2) 重启MySQL服务，备份重启以后生成的binlog。

恢复：

(1) 停掉MySQL服务，在操作系统级别恢复MySQL的数据文件。

(2) 重启MySQL服务，使用mysqlbinlog恢复自备份以来的binlog。

注：直接备份data目录下的数据库目录。恢复时，将备份的文件拷贝到data目录下即可。

### 3 逻辑备份

备份：

1 在系统空闲时，使用mysqldump进行备份。

```
mysqldump -uuser_name -p database_name -F > database_name.sql
```

2 备份mysqldump开始以后生成的binlog。

恢复：

1 停掉应用，执行mysql导入备份文件。

```
mysql -uuser_name -p database_name < database_name.sql
```

2 使用mysqlbinlog恢复自mysqldump备份以来的binlog。

```
mysqlbinlog binlog_filename | mysql -u root -p
```

注：使用mysqldump进行备份得到的文件是sql文件，里面包含的是创建表和插入数据的sql语句。而在恢复时，直接将sql语句抛给mysql，让它运行sql语句即可。

### 4 单个表的备份

备份：

方法1：

```
select * into outfile 'backup_filename' fields-terminated-by=',' from table_name;
```

注：该命令将表table_name中的数据保存到文件backup_filename中，因此，它只备份数据。

方法2：

```
mysqldump -uroot -p -T backup_directory database_name table_name --fields-terminated-by=,
```

注：mysqldump会生成两个文件table_name.sql和table_name.txt，其中table_name.sql中包含生成表结构的sql语句，table_name.txt中包含表的数据。--fields-terminated-by是指定域之间的分隔符，如果指定为`,`，则分隔符就是`,`。

恢复：

方法1：

```
load data [local] infile 'backup_filename' into table_name fields-terminated=',';
```

方法2：

```
mysqlimport -uroot -p [--local] database_name backup_filename --fields-terminated=,
```

### 5 小结

备份与恢复涉及到的命令有：

mysqldump 将数据库备份为sql文件，使用-T选项可以将数据库中的表单独保存，每个表分成创建表格式的sql文件和数据的txt文件。

select 将数据库中的某个表的数据保存到txt文件。

mysqlimport 将数据导入到数据库中。

mysqlbinlog 使用二进制日志进行增量恢复。

load 将数据导入到数据库中的某个表中，与mysqlimport类似。

其中mysqldump、select都是完全备份，mysqlbinlog为增量恢复。

### 参考资料：

1 深入浅出MySQL数据库开发、优化与管理维护