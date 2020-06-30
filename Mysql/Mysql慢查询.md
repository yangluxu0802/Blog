### Mysql慢查询

```mysql
#显示数据库状态
SHOW STATUS;
#显示慢查询是否开启
SHOW VARIABLES LIKE 'slow_query_log';
#显示慢查询日志存放地址
SHOW VARIABLES LIKE 'slow_query_log_file';
#设置慢查询日志存放的位置
SET GLOBAL slow_query_log_file = '/usr/local/mysql/data/slow.log';
#显示慢查询记录阈值
SHOW VARIABLES LIKE 'long_query_time';
#设置慢查询记录阈值
SET GLOBAL long_query_time = 1;
#显示正在运行的线程
SHOW PROCESSLIST;
```

