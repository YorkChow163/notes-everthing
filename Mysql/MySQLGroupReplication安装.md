- [参考1](https://www.jianshu.com/p/dc64fee1c792)
- [参考2](https://www.linuxidc.com/Linux/2017-06/144776.htm)
# 安装
```shell
wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.17-linux-glibc2.5-x86_64.tar.gz 
tar -zxvf mysql-5.7.17-linux-glibc2.5-x86_64.tar.gz -C /opt/app
ln -s /opt/app/mysql /usr/local/mysql
mkdir -p /data/mysql/{mysql_3306,mysql_3307,mysql_3308}/{data,logs,tmp}
chown -R mysql.mysql /data/mysql/
```
# 配置master
```
[mysql]
user=root
password=123456
socket=/usr/local/mysql/mysql1.sock
port=3306

[mysqld]
#misc
user = mysql
basedir = /usr/local/mysql
datadir = /data/mysql/mysql_3306/data
port = 3306
socket = /tmp/mysql3306.sock
event_scheduler = 0
default_storage_engine=innodb

tmpdir=/data/mysql/mysql_3306/tmp
#timeout
interactive_timeout = 43200
wait_timeout = 43200

#character set
character-set-server = utf8

open_files_limit = 65535
max_connections = 100
max_connect_errors = 100000
#
explicit_defaults_for_timestamp
#logs
log-output=file
slow_query_log = 1
slow_query_log_file = slow.log
log-error = error.log
log_error_verbosity=3
pid-file = mysql.pid
long_query_time = 1
#log-slow-admin-statements = 1
#log-queries-not-using-indexes = 1
log-slow-slave-statements = 1

#binlog
binlog_format = row
server-id = 1343306
log-bin = /data/mysql/mysql_3306/logs/mysql-bin
binlog_cache_size = 1M
max_binlog_size = 200M
max_binlog_cache_size = 2G
sync_binlog = 0
expire_logs_days = 10


#group replication
server_id=1013306
gtid_mode=ON
enforce_gtid_consistency=ON
master_info_repository=TABLE
relay_log_info_repository=TABLE
binlog_checksum=NONE
log_slave_updates=ON
binlog_format=ROW

transaction_write_set_extraction=XXHASH64
loose-group_replication_group_name="13855fca-d2ab-11e6-8f37-005056b8286c"
loose-group_replication_start_on_boot=off
#通讯端口配置
loose-group_replication_local_address= "172.16.3.134:3406"
loose-group_replication_group_seeds= "172.16.3.134:3406,172.16.3.134:3407,172.16.3.134:3408"
loose-group_replication_bootstrap_group= off

loose-group_replication_single_primary_mode=off
loose-group_replication_enforce_update_everywhere_checks=on

#relay log
skip_slave_start = 1
max_relay_log_size = 500M
relay_log_purge = 1
relay_log_recovery = 1
#slave-skip-errors=1032,1053,1062

#buffers & cache
table_open_cache = 2048
table_definition_cache = 2048
table_open_cache = 2048
max_heap_table_size = 96M
sort_buffer_size = 2M
join_buffer_size = 2M
thread_cache_size = 256
query_cache_size = 0
query_cache_type = 0
query_cache_limit = 256K
query_cache_min_res_unit = 512
thread_stack = 192K
tmp_table_size = 96M
key_buffer_size = 8M
read_buffer_size = 2M
read_rnd_buffer_size = 16M
bulk_insert_buffer_size = 32M

#myisam
myisam_sort_buffer_size = 128M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1

#innodb
innodb_buffer_pool_size = 500M
innodb_buffer_pool_instances = 1
innodb_data_file_path = ibdata1:100M:autoextend
innodb_flush_log_at_trx_commit = 2
innodb_log_buffer_size = 64M
innodb_log_file_size = 256M
innodb_log_files_in_group = 3
innodb_max_dirty_pages_pct = 90
innodb_file_per_table = 1
innodb_rollback_on_timeout
innodb_status_file = 1
innodb_io_capacity = 2000
transaction_isolation = READ-COMMITTED
innodb_flush_method = O_DIRECT
```
## 启动master
```
/usr/local/mysql/bin/mysqld --defaults-file=/data/mysql/mysql_3306/my3306.cnf --initialize-insecure
/usr/local/mysql/bin/mysqld --defaults-file=/data/mysql/mysql_3306/my3306.cnf --user=mysql &
CREATE USER rpl_user@'%';
GRANT REPLICATION SLAVE ON *.* TO rpl_user@'%' IDENTIFIED BY 'rpl_pass';
flush privileges;
SET SQL_LOG_BIN=1;
CHANGE MASTER TO MASTER_USER='rpl_user', MASTER_PASSWORD='rpl_pass' FOR CHANNEL 'group_replication_recovery'; 
install plugin group_replication soname 'group_replication.so';
SHOW PLUGINS;
```
