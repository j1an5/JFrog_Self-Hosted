# Installing the JFrog Platform
This section will help you get started with the JFrog Platform installation initial steps and planning.

## Before You Begin
In this section, you will find information to get you started with installing or upgrading whether you are a new user or an existing user to the JFrog Platform. Depending on your subscription type and organization's needs, you can install the complete set of JFrog products or start with JFrog Artifactory as the base. To learn about the JFrog supported subscriptions, see the JFrog offering.

**To get started with a new installation**, you'll need to know the following:

1. Your subscription type. If you don’t have an existing subscription, [Start a Free Trial](https://www.jfrogchina.com/start-free/).
2. Your selected JFrog products to install.
3. Review the System Architecture and System Requirements
4. Plan your new installation.

### Installing JFrog Products by Subscription Type
To use JFrog products, you'll need to first install JFrog Artifactory. Once installed, you can continue to install additional products according to your subscription type and organization needs. 

>The new major versions of the JFrog Platform products are not backward compatible with previous major versions.

JFrog products should be installed in the following order:

1. [Install Artifactory](https://www.jfrog.com/confluence/display/JFROG/Installing+Artifactory)
2. Install Insight (Enterprise/Enterprise+ only)
3. Apply your JFrog License depending on your license type, you can:
    * either deploy a license manually to Artifactory (using the onboarding wizard on initial startup)
    * or use a license bucket after connecting Insight
4. Install Artifactory Edge node(s) (optional)
    * The installation process is identical to installing any other Artifactory instance.
5. [Install Xray](https://www.jfrog.com/confluence/display/JFROG/Installing+Xray)
    * Trial License: upload it to your JPD as a separate file. Read more about the JFrog Xray trial.
6. Install Distribution
    * Configure Distribution
7. Install Pipelines

Below are the products you have access to for each subscription. 
>Products in purple are mandatory and products in black are optional depending on your organization needs.

| Subscription Type | JFrog Products to Install |
| ---- | ---- |
| Artifactory OSS (Free) |JFrog Artifactory Open Source: Maven and Generic Package Manager Repository |
| Artifactory CE (Free) | JFrog Artifactory Conan Edition: Conan C/C++ and Generic Package Manager Repository |
| JFrog Container Registry (Free) | JFrog Container Registry (Powered by Artifactory): Docker, Helm and Generic Package Manager Repository |
| Pro | JFrog Artifactory: Universal Package Manager Repository |
| Pro X | JFrog Artifactory: Universal Package Manager Repository <br>JFrog Xray: Security and Compliance Scanning |
| Enterprise X | JFrog Artifactory: Universal Package Manager Repository <br>JFrog Xray: Security and Compliance Scanning |
| Enterprise+ | JFrog Artifactory: Universal Package Manager Repository <br>JFrog Insight: Manage DevOps Insights <br>JFrog Xray: Security and Compliance Scanning <br>JFrog Pipelines: CI/CD pipeline orchestration  <br>JFrog Distribution: Global software distribution |




## Single Node Installation
### Linux Archive Installation

#### Artifactory
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

#### Xray
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
        # rpm -ivh --replacepkgs $JFROG_HOME/xray/app/third-party/rabbitmq/socat-\<version>.rpm<br>
        # rpm -ivh --replacepkgs $JFROG_HOME/xray/app/third-party/rabbitmq/erlang-\<version>.rpm
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
          driver: org.postgresql.Driver<br>
          url: "postgres://localhost:5432/xraydb?sslmode=disable"
          username: xray
          password: xxxxxxxxxxxx
    ```
6. Start and manage the Xray service as the user who extracted the tar.<br>
    As a process
    | Daemon Process |
    | ---- |
    | $JFROG_HOME/xray/app/bin/xray.sh start |
7. Check Xray Log.
    ```
    # tail -f $JFROG_HOME/xray/var/log/console.log
    ```
8. Access Xray from your browser at: http://<jfrogUrl>/ui/, go the Dashboard tab in the Application module in the UI.







