## 一、MySQL安装

### 1. MySQL登录方式

> 第一种登录方式（TCP/IP）

```
语法：mysql -h IP地址 -u 用户 -p 密码 -P 端口
```

> 第二种登录方式（利用mysql.sock登录）

```
语法：mysql -S mysql.sock路径
```

### 2. MySQL 管理命令

> 启动mysql 

```shell
service mysql start
systemctl start mysqld.service	#两条命令都行
```

> 查看启动状态

```shell
service mysql status
systemctl status mysqld			#两条命令都行
```

> 停止 mysql 服务

```shell
service mysqld stop
```

> 设置开机自启

```shell
systemctl enable mysqld.service      # 允许开机自启
systemctl disable mysqld.service     # 禁止开机自启
```

> 修改 MySQL 密码

```shell
alter user 'root'@'localhost' identified by '新密码';
```

### 3. 安装步骤

> 安装需要用到6个rpm包，官网可以下载

```
mysql-community-common-8.0.29-1.el7.x86_64.rpm
mysql-community-client-plugins-8.0.29-1.el7.x86_64.rpm
mysql-community-libs-8.0.29-1.el7.x86_64.rpm
mysql-community-client-8.0.29-1.el7.x86_64.rpm
mysql-community-icu-data-files-8.0.29-1.el7.x86_64.rpm
mysql-community-server-8.0.29-1.el7.x86_64.rpm
```

> 卸载已安装的 MySQL

```shell
rpm -qa | grep -i mysql		 # -i 忽略字符大小写的差别
rpm -e --nodeps + 查询到的名字  #--nodeps不检查依赖强制删除
```

> 修改 /tmp 目录权限

```shell
chown -R 777 /tmp
# 因为 mysql 安装过程中，会通过 mysql 用户在 /tmp 目录下新建 tmp_db 文件
# 所以需要给 /tmp 目录较大的权限
```

> 检查依赖是否存在

```shell
rpm -qa | grep libaio
rpm -qa | grep  net-tools
```

> 安装 openssl-devel 插件

```shell
yum install openssl-devel
# mysql 里面有些 rpm 的安装依赖于该插件
```

> 安装rpm包

```shell
rpm -ivh mysql-community-common-8.0.28-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-plugins-8.0.28-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-8.0.28-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-8.0.28-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-8.0.28-1.el7.x86_64.rpm
rpm -ivh mysql-community-icu-data-files-8.0.28-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-8.0.28-1.el7.x86_64.rpm
# 安装完就可以启动服务了
```

> rpm 安装 MySQL 会自动生成一个随机密码

```shell
cat /var/log/mysqld.log
# 可在 /var/log/mysqld.log 这个文件中查找该密码
```

### 4、卸载步骤

> 卸载 MySQL 前需要先停止 MySQL

```shell
systemctl stop mysqld
```

> 卸载安装包

```shell
rpm -qa | grep -i mysql			#查询 MySQL 的安装文件

# 卸载查询出来的所有的 MySQL 安装包
rpm -e --nodeps mysql-community-common-8.0.28-1.el7.x86_64.rpm
rpm -e --nodeps mysql-community-client-plugins-8.0.28-1.el7.x86_64.rpm
rpm -e --nodeps mysql-community-libs-8.0.28-1.el7.x86_64.rpm
rpm -e --nodeps mysql-community-client-8.0.28-1.el7.x86_64.rpm
rpm -e --nodeps mysql-community-server-8.0.28-1.el7.x86_64.rpm
rpm -e --nodeps mysql-community-icu-data-files-8.0.28-1.el7.x86_64.rpm
rpm -e --nodeps mysql-community-server-8.0.28-1.el7.x86_64.rpm
```

> 删除MySQL的数据存放目录

```shell
rm -rf /var/lib/mysql/
```

> 删除MySQL的配置文件备份

```shell
rm -rf /etc/my.cnf.rpmsave
```

> 删除日志文件

```shell
rm -rf /var/log/mysqld.log
```

### 5、远程登录设置

**方法一(改表法)**

> 更改 “mysql” 数据库里的 “user” 表里的 “host” 项，从”localhost”改称”%”，然后重启mysql服务

```mysql
mysql> show databases;
mysql> use mysql;
mysql> select * from user;
+-----------+------------------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------------------+--------------------------+----------------------------+---------------+-------------+-----------------+----------------------+-----------------------+------------------------------------------------------------------------+------------------+-----------------------+-------------------+----------------+------------------+----------------+------------------------+---------------------+--------------------------+-----------------+
| Host      | User             | Select_priv | Insert_priv | Update_priv | Delete_priv | Create_priv | Drop_priv | Reload_priv | Shutdown_priv | Process_priv | File_priv | Grant_priv | References_priv | Index_priv | Alter_priv | Show_db_priv | Super_priv | Create_tmp_table_priv | Lock_tables_priv | Execute_priv | Repl_slave_priv | Repl_client_priv | Create_view_priv | Show_view_priv | Create_routine_priv | Alter_routine_priv | Create_user_priv | Event_priv | Trigger_priv | Create_tablespace_priv | ssl_type | ssl_cipher             | x509_issuer              | x509_subject               | max_questions | max_updates | max_connections | max_user_connections | plugin                | authentication_string                                                  | password_expired | password_last_changed | password_lifetime | account_locked | Create_role_priv | Drop_role_priv | Password_reuse_history | Password_reuse_time | Password_require_current | User_attributes |
+-----------+------------------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------------------+--------------------------+----------------------------+---------------+-------------+-----------------+----------------------+-----------------------+------------------------------------------------------------------------+------------------+-----------------------+-------------------+----------------+------------------+----------------+------------------------+---------------------+--------------------------+-----------------+
| localhost | mysql.infoschema | Y           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          | 0x                     | 0x                       | 0x                         |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | N                | 2022-07-16 19:14:03   |              NULL | Y              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | mysql.session    | N           | N           | N           | N           | N           | N         | N           | Y             | N            | N         | N          | N               | N          | N          | N            | Y          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          | 0x                     | 0x                       | 0x                         |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | N                | 2022-07-16 19:14:03   |              NULL | Y              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | mysql.sys        | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          | 0x                     | 0x                       | 0x                         |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | N                | 2022-07-16 19:14:03   |              NULL | Y              | N                | N              |                   NULL |                NULL | NULL                     | NULL            |
| localhost | root             | Y           | Y           | Y           | Y           | Y           | Y         | Y           | Y             | Y            | Y         | Y          | Y               | Y          | Y          | Y            | Y          | Y                     | Y                | Y            | Y               | Y                | Y                | Y              | Y                   | Y                  | Y                | Y          | Y            | Y                      |          | 0x                     | 0x                       | 0x                         |             0 |           0 |               0 |                    0 | caching_sha2_password | $A$005$~U LM`cdmA~HBV":3RJtPPuo7w6B001VbkSCpL/B.Hb7INeBd63h7z1fgv0 | N                | 2022-07-16 19:21:21   |              NULL | N              | Y                | Y              |                   NULL |                NULL | NULL                     | NULL            |
+-----------+------------------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------------------+--------------------------+----------------------------+---------------+-------------+-----------------+----------------------+-----------------------+------------------------------------------------------------------------+------------------+-----------------------+-------------------+----------------+------------------+----------------+------------------------+---------------------+--------------------------+-----------------+
4 rows in set (0.00 sec)

mysql> update user set Host="%" where User="root";
```

**方法二(授权法)**

> 允许账户myuser使用密码1234从任何主机连接到mysql服务器

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY '1234' WITH GRANT OPTION;
-- 这里%表示允许所有IP地址访问。可以改为特定IP
```

> 允许账户myuser从ip为192.168.1.3的主机连接到mysql服务器，并使用12345作为密码

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'192.168.1.3' IDENTIFIED BY '12345' WITH GRANT OPTION;
```

> 最后，让设置生效

```mysql
FLUSH PRIVILEGES;
```





