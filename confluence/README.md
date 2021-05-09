# README

## Docker 运行 confluence

### 准备安装包

```bash
# confluence tar 包
export confluence_version=7.4.8
wget https://www.atlassian.com/software/confluence/downloads/binary/atlassian-confluence-${confluence_version}.tar.gz
# jdbc jar 包
wget https://downloads.mysql.com/archives/get/p/3/file/mysql-connector-java-5.1.49.tar.gz
tar -xvf mysql-connector-java-5.1.49.tar.gz && mv mysql-connector-java-5.1.49/mysql-connector-java-5.1.49.jar .
# 破解 jar 包
wget https://ftp.skylens.co/atlassian/atlassian-agent.jar
```

### Docker 镜像构建

```bash
# 使用 buildkit 优化
export DOCKER_BUILDKIT=1

export confluence_version=7.4.8
docker build -t atlassian/confluence:${confluence_version} .

docker images | grep atlassian/confluence:${confluence_version}
```

