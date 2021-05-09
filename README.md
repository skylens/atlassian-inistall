# Atlassian 产品安装教程

## Atlassian 产品列表

+ JIRA Software
+ JIRA Core
+ JIRA Service Desk
+ Confluence
+ Bamboo
+ Bitbucket
+ FishEye
+ Crowd
+ Crucible

## 安装

### 准备

1. Altassian 全系列软件后端数据库支持 MySQL，选择 MySQL 为后端数据库

[https://dev.mysql.com/downloads/repo/yum/](https://dev.mysql.com/downloads/repo/yum/)

```bash
sudo apt install -y mysql-server
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
sudo yum update
sudo yum install -y mysql mysql-server
```

**优化配置**

```
[mysqld]
character-set-server=utf8mb4
collation-server=utf8mb4_bin
default-storage-engine=INNODB
innodb_default_row_format=DYNAMIC
innodb_file_format=Barracuda
max_allowed_packet=256M
innodb_log_file_size=2GB
transaction-isolation=READ-COMMITTED
binlog_format=row
log-bin-trust-function-creators = 1

```

重启服务

```
systemctl restart mysqld
```

创建数据库、用户并赋予权限

优化

```sql
# ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
# 更改密码策略
set global validate_password_length=4;
set global validate_password_policy=0;

SHOW VARIABLES LIKE 'validate_password%';

# 开启 READ-COMMITED
# Your database must use 'READ-COMMITED' as default isolation level.
SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

+ Confluence

```sql
CREATE DATABASE confluence CHARACTER SET utf8 COLLATE utf8mb4_bin;
CREATE USER 'confluence'@'172.16.32.%' IDENTIFIED BY 'confluence';
GRANT ALL PRIVILEGES ON confluence.* TO 'confluence'@'172.16.32.%' IDENTIFIED BY 'confluence' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```


### 下载软件包

软件包有两种，一种是bin结尾的二进制包，一种是tar.gz结尾的压缩包，bin结尾的二进制包安装是交互式的，这里选择bin结尾的二进制包 

bin结尾的二进制包

```bash
wget https://www.atlassian.com/software/confluence/downloads/binary/atlassian-confluence-6.15.3-x64.bin
wget https://www.atlassian.com/software/stash/downloads/binary/atlassian-bitbucket-6.2.0-x64.bin
wget https://www.atlassian.com/software/jira/downloads/binary/atlassian-jira-software-8.1.0-x64.bin
```

tar.gz结尾的压缩包

```bash
wget https://www.atlassian.com/software/confluence/downloads/binary/atlassian-confluence-6.15.3.tar.gz
wget https://www.atlassian.com/software/stash/downloads/binary/atlassian-bitbucket-6.2.0.tar.gz
wget https://www.atlassian.com/software/jira/downloads/binary/atlassian-jira-software-8.1.0.tar.gz
```

## 破解

用于测试，切勿用于商业用途

[Atlassian系列产品及插件激活方法[JIRA8.0]](https://zhile.io/2018/12/20/atlassian-license-crack.html)

```bash
wget -o /opt/atlassian/atlassian-agent.jar https://ftp.skylens.co/atlassian/atlassian-agent.jar
# 设置环境变量

echo '#!/bin/sh
export JAVA_OPTS="-javaagent:/opt/atlassian/atlassian-agent.jar ${JAVA_OPTS}"' >> /etc/profile.d/atlassian-agent.sh
```
