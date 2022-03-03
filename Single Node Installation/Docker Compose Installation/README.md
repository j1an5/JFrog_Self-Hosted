# Single Node Installation
## Linux Archive Installation

### Artifactory
1. Set the JFrog Home environment variable
    ```
    # echo "export JFROG_HOME=/opt/jfrog" >> /etc/profile
    # source /etc/profile
    ```
2. Create a JFrog Home directory and move the downloaded installer archive into that directory, for example:
    ```
    # mkdir -p $JFROG_HOME
    # mv jfrog-artifactory-pro-<version>-linux.tar.gz $JFROG_HOME/
    # cd $JFROG_HOME/
    ```
3. Extract the contents of the compressed archive and move it into the artifactory directory.
    ```
    # tar -xvf jfrog-artifactory-pro-<version>-linux.tar.gz
    # mv artifactory-pro-<version> artifactory
    ```
4. Customize the product configuration (optional) including database, Java Opts, and filestore.
    ```
    # vim $JFROG_HOME/artifactory/var/etc/system.yaml
    shared:
    ....
      node:
        id: "art1"
        ip: "192.168.xx.xx”
    ....
    ```
5. Run Artifactory as a foreground, background process, or as a service.
    | Daemon Process |
    | ---- |
    | $JFROG_HOME/artifactory/app/bin/artifactoryctl start |
6. Check Artifactory Log.
    ```
    # tail -f $JFROG_HOME/artifactory/var/log/console.log
    ```
7. Access Artifactory from your browser at: http://SERVER_HOSTNAME:8082/ui/. For example, on your local machine: http://localhost:8082/ui/.

### Xray
1. Set the JFrog Home environment variable
    ```
    # echo "export JFROG_HOME=/opt/jfrog" >> /etc/profile
    # source /etc/profile
    ```
2. Create a JFrog Home directory and move the downloaded installer archive into that directory, for example:
    ```
    # mkdir -p $JFROG_HOME
    # mv jfrog-xray-<version>-linux.tar.gz $JFROG_HOME/
    # cd $JFROG_HOME/
    ```
3. Extract the contents of the compressed archive and move it into xray directory.
    ```
    # tar -xvf jfrog-xray-<version>-linux.tar.gz
    # mv jfrog-xray-<version>-linux xray
    ```
4. Prerequisites
    1. PostgreSQL [install](https://www.postgresql.org/download/linux/redhat/) & configure
        ```
        # yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
        # yum install -y postgresql12-server
        # /usr/pgsql-12/bin/postgresql-12-setup initdb
        # systemctl enable postgresql-12 && systemctl start postgresql-12
        # vim /var/lib/pgsql/12/data/pg_hba.conf
        "local" is for Unix domain socket connections only
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
        # su - postgres //切换用户并执行创建数据库脚本
        $ POSTGRES_PATH=\$(dirname \$(readlink -f \$(which psql))) \$JFROG_HOME/xray/app/third-party/postgresql/createPostgresUsers.sh
        ```
    2. Erlang - Packaged as RPM (or DEB) within the archive.
        ```
        # rpm -ivh --replacepkgs $JFROG_HOME/xray/app/third-party/rabbitmq/socat-<version>.rpm
        # rpm -ivh --replacepkgs $JFROG_HOME/xray/app/third-party/rabbitmq/erlang-<version>.rpm
        ```
    3. Db-Utils - Packaged as RPM (or DEB) within the archive.
        ```
        # yum install -y $JFROG_HOME/xray/app/third-party/misc/*utils-<version>.rpm
        ```
5. Customize the product configuration
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
          password: xxxxxxxxxxxx
    ```
6. Start and manage the Xray service as the user who extracted the tar.(As a process)
    | Daemon Process |
    | ---- |
    | $JFROG_HOME/xray/app/bin/xray.sh start |
7. Check Xray Log.
    ```
    # tail -f $JFROG_HOME/xray/var/log/console.log
    ```
8. Access Xray from your browser at: http://\<jfrogUrl>/ui/, go the Dashboard tab in the Application module in the UI.

