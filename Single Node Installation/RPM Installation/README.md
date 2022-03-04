# Single Node Installation
## RPM Installation

### Artifactory
1. Install Artifactory as a service on Red Hat compatible Linux distributions, as a root user.
    ```
    # wget https://releases.jfrog.io/artifactory/artifactory-pro-rpms/jfrog-artifactory-pro/jfrog-artifactory-pro-7.33.9.rpm
    # yum install -y jfrog-artifactory-pro-<version>.rpm
    ```
2. Customize the product configuration (optional) including database, Java Opts, and filestore.
    ```
    # vim $JFROG_HOME/artifactory/var/etc/system.yaml
    shared:
    ....
      node:
        id: "art1"
        ip: "192.168.xx.xx”
    ....
    ```
3. Manage Artifactory using the following commands.
    ```
    # systemctl start|stop artifactory
    ```
4. Check Artifactory Log.
    ```
    # tail -f $JFROG_HOME/artifactory/var/log/console.log
    ```
5. Access Artifactory from your browser at: http://SERVER_HOSTNAME:8082/ui/. For example, on your local machine: http://localhost:8082/ui/.

### Xray
1. Set the JFrog Home environment variable
    ```
    # echo "export JFROG_HOME=/opt/jfrog" >> /etc/profile
    # source /etc/profile
    ```
2. Create a JFrog Home directory and move the downloaded installer archive into that directory, for example:
    ```
    # mkdir -p $JFROG_HOME
    # wget https://releases.jfrog.io/artifactory/jfrog-xray/xray-rpm/3.43.1/jfrog-xray-3.43.1-rpm.tar.gz
    # mv jfrog-xray-<version>-rpm.tar.gz $JFROG_HOME/
    # cd $JFROG_HOME/
    ```
3. Extract the contents of the compressed archive, and go to the extracted folder.
    ```
    # tar -xvf jfrog-xray-<version>-rpm.tar.gz
    # cd jfrog-xray-<version>-rpm
    ```
4. Prerequisites
    1. PostgreSQL [install](https://www.jfrog.com/confluence/display/JFROG/Installing+Xray#InstallingXray-InstallingPostgreSQL) & configure
        ```
        # mkdir -p /var/opt/postgres/data
        # rpm -ivh --replacepkgs ./third-party/postgresql/libicu-50.2-4.el7_7.x86_64.rpm (only AWS instance)
        # rpm -ivh --replacepkgs ./third-party/postgresql/postgresql13-libs-13.4-1PGDG.rhel7.x86_64.rpm
        # rpm -ivh --replacepkgs ./third-party/postgresql/postgresql13-13.4-1PGDG.rhel7.x86_64.rpm
        # rpm -ivh --replacepkgs ./third-party/postgresql/postgresql13-server-13.4-1PGDG.rhel7.x86_64.rpm
        # chown -R postgres:postgres /var/opt/postgres
        # echo 'export PGDATA="/var/opt/postgres/data"' >> /etc/profile
        # echo 'export PGSETUP_INITDB_OPTIONS="-D /var/opt/postgres/data"' >> /etc/profile
        # source /etc/profile
        # sed -i "s~^Environment=PGDATA=.*~Environment=PGDATA=/var/opt/postgres/data~" /lib/systemd/system/postgresql-13.service
        # systemctl daemon-reload
        # /usr/pgsql-13/bin/postgresql-13-setup initdb 
        # systemctl start postgresql-13.service 
        # vim /var/lib/pgsql/13/data/pg_hba.conf
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
        # su - postgres //切换用户并执行创建数据库脚本
        $ POSTGRES_PATH=\$(dirname \$(readlink -f \$(which psql))) \$JFROG_HOME/xray/app/third-party/postgresql/createPostgresUsers.sh
        ```
    2. Install RabbitMQ dependencies.
        ```
        # rpm -ivh --replacepkgs ./third-party/rabbitmq/socat-<version>.rpm
        # rpm -ivh --replacepkgs ./third-party/rabbitmq/erlang-<version>.rpm
        ```
    3. Install db-util. You can use the bundled db-utils RPM found under /third-party/misc/.
        ```
        # yum install -y ./third-party/misc/*utils-<version>.rpm
        ```
5. Install Xray. You must run as a root user.
    ```
    # rpm -Uvh --replacepkgs ./xray/xray.rpm
    ```
6. Customize the product configuration
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
7. Start and manage the Xray service.
    ```
    # systemctl start|stop xray.service
    ```
8. Check Xray Log.
    ```
    # tail -f $JFROG_HOME/xray/var/log/console.log
    ```
9. Access Xray from your browser at: http://\<jfrogUrl>/ui/, go the Dashboard tab in the Application module in the UI.

