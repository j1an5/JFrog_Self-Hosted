# Single Node Installation
## RPM Installation

### 前置条件

1. 演示环境[准备](https://github.com/j1an5/JFrog_Self-Hosted#%E6%BC%94%E7%A4%BA%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)

### Artifactory
1. 安装Artifactory,本文使用的版本是7.33.9(Install Artifactory as a service on Red Hat compatible Linux distributions, as a root user）
    ```
    export rt_version=7.33.9
    wget https://releases.jfrog.io/artifactory/artifactory-pro-rpms/jfrog-artifactory-pro/jfrog-artifactory-pro-${rt_version}.rpm
    yum install -y jfrog-artifactory-pro-${rt_version}.rpm
    ```
    或安装最新版
    ```
    wget https://releases.jfrog.io/artifactory/artifactory-pro-rpms/artifactory-pro-rpms.repo -O /etc/yum.repos.d/jfrog-artifactory-pro-rpms.repo;
    sudo yum update 
    sudo yum install jfrog-artifactory-pro
    ```
2. 修改配置文件(Customize the product configuration (optional) including database, Java Opts, and filestore.)
    ```
    # vim /opt/jfrog/artifactory/var/etc/system.yaml
    shared:
    ....
      node:
        id: "art1"
        ip: "192.168.xx.xx”
    ....
    ```
3. 启动Artifactory(Manage Artifactory using the following commands.)
    ```
    systemctl start artifactory
    ```
4. 检查日志(Check Artifactory Log.)
    ```
    # tail -f /opt/jfrog/artifactory/var/log/console.log
    ....
    ###############################################################
    ###   All services started successfully in xx.xxx seconds   ###
    ###############################################################
    ....
    ```
5. 访问Artifactory UI 9Access Artifactory from your browser)
    >http://SERVER_HOSTNAME:8082/

### Xray
1. 设置JFrog主目录(Set the JFrog Home environment variable)
    ```
    echo "export JFROG_HOME=/opt/jfrog" >> /etc/profile
    source /etc/profile
    ```
2. 创建主目录，进入主目录并下载安装包，本文使用的版本是3.43.1(Create a JFrog Home directory and move the downloaded installer archive into that directory, for example:)
    ```
    mkdir -p $JFROG_HOME && cd $JFROG_HOME/
    export xr_version=3.43.1
    wget https://releases.jfrog.io/artifactory/jfrog-xray/xray-rpm/${xr_version}/jfrog-xray-${xr_version}-rpm.tar.gz
    ```
3. 提取压缩包,进入目录(Extract the contents of the compressed archive, and go to the extracted folder.)
    ```
    tar -xvf jfrog-xray-${xr_version}-rpm.tar.gz
    cd jfrog-xray-${xr_version}-rpm
    ```
4. 前置条件(Prerequisites)
    1. PostgreSQL [安装](https://www.jfrog.com/confluence/display/JFROG/Installing+Xray#InstallingXray-InstallingPostgreSQL) & configure
        ```
        mkdir -p /var/opt/postgres/data
        rpm -ivh --replacepkgs ./third-party/postgresql/libicu-*.el7_7.x86_64.rpm
        rpm -ivh --replacepkgs ./third-party/postgresql/postgresql13-libs-*.rhel7.x86_64.rpm
        rpm -ivh --replacepkgs ./third-party/postgresql/postgresql13-13.*.rhel7.x86_64.rpm
        rpm -ivh --replacepkgs ./third-party/postgresql/postgresql13-server-13.*-*.rhel7.x86_64.rpm
        chown -R postgres:postgres /var/opt/postgres
        echo 'export PGDATA="/var/opt/postgres/data"' >> /etc/profile
        echo 'export PGSETUP_INITDB_OPTIONS="-D /var/opt/postgres/data"' >> /etc/profile
        source /etc/profile
        sed -i "s~^Environment=PGDATA=.*~Environment=PGDATA=/var/opt/postgres/data~" /lib/systemd/system/postgresql-13.service
        systemctl daemon-reload
        /usr/pgsql-13/bin/postgresql-13-setup initdb 
        systemctl start postgresql-13.service 
        ```
    2. PostgreSQL 配置
        ```
        # vim /var/opt/postgres/data/pg_hba.conf
            # "local" is for Unix domain socket connections only
            local   all             all                                     peer
            # IPv4 local connections:
            host    all             all             127.0.0.1/32            trust
            # IPv6 local connections:
            host    all             all             ::1/128                 trust
            # Allow replication connections from localhost, by a user with the
            # replication privilege.
            local   replication     all                                     peer
            host    replication     all             127.0.0.1/32            md5
            host    replication     all             ::1/128                 md5
        # systemctl restart postgresql-13.service
        ```
    3. 创建Xray数据库及用户
        ```
        # su - postgres //切换用户并执行创建数据库脚本
        $ POSTGRES_PATH=$(dirname $(readlink -f $(which psql))) $JFROG_HOME/jfrog-xray-*-rpm/third-party/postgresql/createPostgresUsers.sh
        ....main] - Waiting for Postgres to get ready using the commands: "/usr/pgsql-13/bin/psql --host=localhost --port=5432 --version" & "/usr/pgsql-13/bin/psql --host=localhost --port=5432 -l"
        ....main] - Postgres is ready. Executing commands
        ....main] - Postgres setup is now complete
        $ exit
        ```
    4. 安装Rabbitmq依赖(Install RabbitMQ dependencies.)
        ```
        rpm -ivh --replacepkgs ./third-party/rabbitmq/socat-*.el7.x86_64.rpm
        rpm -ivh --replacepkgs ./third-party/rabbitmq/erlang-*.el7.x86_64.rpm
        ```
    5. 安装db-util(You can use the bundled db-utils RPM found under /third-party/misc/.)
        ```
        yum install -y ./third-party/misc/libdb-utils-*.el7.x86_64.rpm
        ```
5. 安装Xray(Install Xray. You must run as a root user)
    ```
    rpm -Uvh --replacepkgs ./xray/xray.rpm
    ```
6. 修改配置文件(Customize the product configuration)
    >![Artifactory Join Key 1](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%201.png?raw=true)
    ![Artifactory Join Key 2](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%202.png?raw=true)

    ```
    # vim $JFROG_HOME/xray/var/etc/system.yaml
    configVersion: 1
      shared:
        jfrogUrl: http://192.168.xx.xx:8082/
        security:
          joinKey: "xxxxxxxxxxxxxxxx"
        node:
          id: "xray1"
          ip: "192.168.xx.xx"
        database:
          type: postgresql
          driver: org.postgresql.Driver
          url: "postgres://localhost:5432/xraydb?sslmode=disable"
          username: xray
          password: xray
    ```
7. 启动xray(Start and manage the Xray service.)
    ```
    systemctl start xray
    ```
8. 检查日志(Check Xray Log.)
    ```
    # tail -f $JFROG_HOME/xray/var/log/console.log
    ....
    ###############################################################
    ###   All services started successfully in xx.xxx seconds   ###
    ###############################################################
    ....
    ```
9. 通过Artifactory UI访问Xray(Access Xray from your browser)
    >http://\<jfrogUrl>/

