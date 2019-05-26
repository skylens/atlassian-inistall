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

Atlassian 全系列软件是 Java 开发的，所以需要安装 Java，这里选择是用 openjdk-8，当然也可以使用 OracleJdk

```bash
sudo apt install -y openjdk-8-jdk
sudo yum install -y java-1.8.0-openjdk
```

Altassian 全系列软件后端数据库支持 MySQL，选择 MySQL 为后端数据库

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
sql_mode = NO_AUTO_VALUE_ON_ZERO
transaction-isolation=READ-COMMITTED
binlog_format=row

```

重启服务

```
systemctl restart mysqld
```

创建数据库、用户并赋予权限

```sql
-- 非必需（设置密码策略为简单）
SET GLOBAL validate_password_policy=LOW;
-- 创建本地认证的用户
CREATE USER 'atlassian'@'localhost' IDENTIFIED BY 'atlassian';
-- 创建安装 confluence 所需的数据库
CREATE DATABASE confluence CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
-- 赋予 atlassian 对 confluence 的所有权限
GRANT ALL PRIVILEGES ON confluence.* TO 'atlassian'@'localhost' IDENTIFIED BY 'atlassian' WITH GRANT OPTION;
-- 创建安装 bitbucket 所需的数据库
CREATE DATABASE bitbucket CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
-- 赋予 atlassian 对 bitbucket 的所有权限
GRANT ALL PRIVILEGES ON bitbucket.* TO 'atlassian'@'localhost' IDENTIFIED BY 'atlassian' WITH GRANT OPTION;
-- 创建安装 jira 所需的数据库
CREATE DATABASE jira CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
-- 赋予 atlassian 对 jira 的所有权限
GRANT ALL PRIVILEGES ON jira.* TO 'atlassian'@'localhost' IDENTIFIED BY 'atlassian' WITH GRANT OPTION;
-- 刷新权限
FLUSH PRIVILEGES;
```

MySQL jdbc 下载配置，这个步骤在安装完毕时进行操作，之后需要重启各项服务

```bash
wget https://cdn.mysql.com//Downloads/Connector-J/mysql-connector-java-5.1.47.tar.gz
tar -xvf mysql-connector-java-5.1.47.tar.gz
cp mysql-connector-java-5.1.47/mysql-connector-java-5.1.47.jar /opt/atlassian/confluence/confluence/WEB-INF/lib
cp mysql-connector-java-5.1.47/mysql-connector-java-5.1.47.jar /opt/atlassian/bitbucket/app/WEB-INF/lib/
cp mysql-connector-java-5.1.47/mysql-connector-java-5.1.47.jar /opt/atlassian/jira/atlassian-jira/WEB-INF/lib/
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

### 创建目录

```bash
mkdir -p /opt/atlassian/{confluence,bitbucket,jira,application-data}
mkdir -p /opt/atlassian/application-data/{confluence,bitbucket,jira}
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
