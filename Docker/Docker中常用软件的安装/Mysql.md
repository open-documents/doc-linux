
以安装Mysql5.7为例。

1）下载镜像。
```bash
$ docker pull mysql:5.7 
# 下载完成后显示结果:
# 5.7: Pulling from library/mysql
# 70e9ff4420fb (多次安装保持不变): Pull complete                                                             1
# 7ca4383b183f (多次安装保持不变): Pull complete                                                             2
# 3e282e7651b1 (多次安装保持不变): Pull complete                                                             3
# 1ffa0e0ca707 (多次安装保持不变): Pull complete                                                             4 
# 6eb790cf6382 (多次安装保持不变): Pull complete                                                             5 
# 2b7ffc37d8e9 (多次安装保持不变): Pull complete                                                             6 
# 4393c12228b9 (多次安装保持不变): Pull complete                                                             7 
# 389d2c130d52 (多次安装保持不变): Pull complete                                                             8 
# e5df3caef94c (多次安装保持不变): Pull complete                                                             9 
# 5c6aa409290d (多次安装保持不变): Pull complete                                                             10 
# faa350980ea9 (多次安装保持不变): Pull complete                                                             11 
# Digest: sha256:bd873931ef20f30a5a9bf71498ce4e02c88cf48b2e8b782c337076d814deebde(多次安装保持不变)
# Status: Downloaded newer image for mysql:5.7
# docker.io/library/mysql:5.7
```

2）在宿主机创建3个目录/文件：
- `/custom/mysql/etc/xxx.cnf`
- `/custom/mysql/data`
- `/custom/mysql/log`

3）创建容器。
```bash
$ docker run -d -v /custom/mysql/log:/var/log/mysql -v /custom/mysql/data:/var/lib/mysql -v /custom/mysql/etc:/etc/mysql/conf.d/ -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
```





# x Mysql配置文件

```text
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port = 3306
# 设置mysql客户端连接服务端时创建的socket文件名称
socket = /usr/local/services/mysql/var/data/mysql.sock

[mysqld]
# bind-address = 0.0.0.0
# 服务端端口号
# port = 3306
# socket = /usr/local/services/mysql/var/data/mysql.sock
# mysql进程pid文件
# pid-file = /usr/local/services/mysql/var/logs/mysql.pid
# 设置服务端使用的字符集
character-set-server = utf8
# 设置mysql的安装目录
# basedir = /usr/local/services/mysql
# 设置mysql数据库的数据存放目录
datadir = /usr/local/services/mysql/var/data


# skip-external-locking
# skip-name-resolve
# lower_case_table_names = 1
# log-bin-trust-function-creators = 1

max_connections = 6000
max_user_connections = 6000
max_connect_errors = 4000
wait_timeout = 86400
interactive_timeout = 86400
table_open_cache = 512
max_allowed_packet = 32M
sort_buffer_size = 2M
join_buffer_size = 2M
thread_cache_size = 8
thread_concurrency = 8
query_cache_size = 32M

# 设置创建新表时默认的存储引擎
default-storage-engine = InnoDB

# sql_mode="STRICT_ALL_TABLES,NO_AUTO_CREATE_USER"
# server-id = 1

# log-short-format
# log-error = /usr/local/services/mysql/var/logs/mysql.log
# slow_query_log
# long_query_time = 2
# slow_query_log_file = /usr/local/services/mysql/var/logs/mysql-slow.log
# 
# log-bin = /usr/local/services/mysql/var/binlog/mysql-bin
# log_bin_trust_function_creators=1
# binlog_format = MIXED
# expire_logs_days = 10

# INNODB Specific options
# innodb_data_home_dir = /usr/local/services/mysql/var/data
# innodb_log_group_home_dir = /usr/local/services/mysql/var/redolog
# innodb_additional_mem_pool_size = 10M
# innodb_buffer_pool_size = 4G
# innodb_data_file_path = ibdata1:100M:autoextend
# innodb_file_io_threads = 4
# innodb_thread_concurrency = 8
# innodb_flush_log_at_trx_commit = 0
# innodb_flush_method = O_DIRECT
# innodb_log_buffer_size = 128M
# innodb_log_file_size = 256M
# innodb_log_files_in_group = 3
# innodb_max_dirty_pages_pct = 90
# innodb_lock_wait_timeout = 50
# innodb_file_per_table = 1

# MyISAM Specific options
# key_buffer_size = 384M
# read_buffer_size = 4M
# read_rnd_buffer_size = 8M
# myisam_sort_buffer_size = 128M
# myisam_max_sort_file_size = 1G
# myisam_repair_threads = 1
# myisam_recover

[mysqldump]
quick
max_allowed_packet = 16M

[mysql]
default-character-set = utf8
no-auto-rehash
socket = /usr/local/services/mysql/var/data/mysql.sock

[myisamchk]
key_buffer_size = 256M
sort_buffer_size = 256M
read_buffer = 2M
write_buffer = 2M

[mysqlhotcopy]
interactive-timeout
```





