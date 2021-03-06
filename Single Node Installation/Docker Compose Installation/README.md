# Single Node Installation
## Docker Compose Installation

### 前置条件

1. 演示环境[准备](https://github.com/j1an5/JFrog_Self-Hosted#%E6%BC%94%E7%A4%BA%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)
2. Docker-ce [安装](https://docs.docker.com/compose/install/)
    ```
    yum install -y yum-utils   device-mapper-persistent-data   lvm2
    yum-config-manager   --add-repo    https://download.docker.com/linux/centos/docker-ce.repo
    yum install docker-ce  -y
    systemctl start docker && systemctl enable docker 
    ```
    ```
    # docker --version
    Docker version 20.10.12, build e91ed57
    ```
3. Docker-compose [安装](https://mirror.tuna.tsinghua.edu.cn/help/docker-ce/)
    ```
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 
    ```
    ```
    # docker-compose --version
    docker-compose version 1.29.2, build 5becea4c
    ```
4. 文件句柄数设置[File Handle Allocation Limit](https://www.jfrog.com/confluence/display/JFROG/System+Requirements#SystemRequirements-Xray)
    ```
    # vi /etc/security/limits.conf
    root hard nofile 100000
    root soft nofile 100000
    # reboot
    ```


### Artifactory
1. 下载并解压,进入解压后的目录,本文使用的版本是7.33.9(Extract the contents of the compressed archive)
    ```
    wget https://releases.jfrog.io/artifactory/artifactory-pro/org/artifactory/pro/docker/jfrog-artifactory-pro/7.33.9/jfrog-artifactory-pro-7.33.9-compose.tar.gz
    tar -xvf jfrog-artifactory-pro-7.33.9-compose.tar.gz
    cd artifactory-pro-7.33.9
    ```
2. 执行交互脚本,自动生成配置文件(Run the script to setup folders with required ownership. This is an interactive script.)
    >\# ./config.sh<br>
    <br>
    Beginning JFrog Artifactory setupM<br>
    <br>
    Validating System requirements<br>
    <br>
    Installation Directory (Default: /opt/jfrog/artifactory): <font color=#FF0000 >/opt/jfrog/artifactory</font><br>
    <br>
    Running system diagnostics checks, logs can be found at [/root/artifactory-pro-7.33.9/systemDiagnostics.log]<br>
    <br>
    Triggering migration script. This will migrate if needed and may take some time.<br>
    <br>
    Migration logs will be available at [/root/artifactory-pro-7.33.9/bin/migration.log]. The file will be archived at [/opt/jfrog/artifactory/var/log] after installation<br>
    <br>
    For IPv6 address, enclose value within square brackets as follows : [<ipv6_address>]<br>
    Please specify the IP address of this machine (Default: fe80::825:e011:a715:9470%enp0s8): <font color=#0000FF >192.168.56.110</font><br>
    <br>
    Are you adding an additional node to an existing product cluster? [y/N]: n<br>
    <br>
    The installer can install a PostgreSQL database, or you can connect to an existing compatible PostgreSQL database<br>
    (https://service.jfrog.org/installer/System+Requirements#SystemRequirements-RequirementsMatrix)<br>
    If you are upgrading from an existing installation, select N if you have externalized PostgreSQL, select Y if not.<br>
    Do you want to install PostgreSQL? [Y/n]: n<br>
    <br>
    Provide the type of your external database that you want to connect to.<br>
    Note : If you choose derby, it will be considered as an internal database and no further details will be asked<br>
    Enter database type, supported values [ postgresql mssql mariadb mysql oracle derby ]: derby<br>
    <br>
    Database type set as [derby]. Skipping database related prompts<br>
    <br>
    Creating third party directories (if necessary)<br>
    <br>
    Docker setup complete<br>
    <br>
    Installation directory: [/opt/jfrog/artifactory] contains data and configurations.<br>
    <br>
    Use docker-compose commands to start the application. Once the application has started, it can be accessed at [http://fe80::825:e011:a715:9470%enp0s8:8082]<br>
    <br>
    Examples:<br>
    cd /root/artifactory-pro-7.33.9<br>
    <br>
    <br>
    <br>
    start:               docker-compose -p rt up -d<br>
    stop:                docker-compose -p rt down<br>
    <br>
    NOTE: The compose file uses several environment variables from the .env file. Remember to run from within the [/root/artifactory-pro-7.33.9] folder<br>
    <br>
    Done<br>
    
3. 修改配置(Customize the product configuration (optional) including database, Java Opts, and filestore.)
    ```
    # vi /opt/jfrog/artifactory/var/etc/system.yaml
    shared:
    ....
      node:
        id: "art1"
        ip: "192.168.xx.xx”
    ....
    ```
4. 启动、停止Artifactory(Manage Artifactory using native Docker Compose commands, docker-compose -p rt <action> command.<br>
Run this command from the extracted folder.)<br>
    ```
    docker-compose -p rt up -d
    docker-compose -p rt down
    ```
5. Check Artifactory Log.
    ```
    # docker-compose -p rt logs -f
    ....
    artifactory    | ###############################################################
    artifactory    | ###   All services started successfully in xx.xxx seconds   ###
    artifactory    | ###############################################################
    ...
    ```
6. 访问Artifactory(Access Artifactory from your browser.)
    > http://SERVER_HOSTNAME:8082

### Xray
1. 下载安装包,解压并进入目录,本文使用的版本是3.43.1(Extract the contents of the compressed archive and go to the extracted folder.)
    ```
    wget https://releases.jfrog.io/artifactory/jfrog-xray/xray-compose/3.43.1/jfrog-xray-3.43.1-compose.tar.gz
    tar -xvf jfrog-xray-3.43.1-compose.tar.gz
    cd jfrog-xray-3.43.1-compose
    ```
2. 执行./config.sh生成配置文件(Run the installer script.)
    >![Artifactory Join Key 1](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%201.png?raw=true)
    ![Artifactory Join Key 2](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%202.png?raw=true)
    ```
    # ./config.sh

    Beginning JFrog Xray setup


    Validating System requirements

    If you are not performing an upgrade, you can ignore the following question and press y
    Have you disconnected Artifactory Xray pairings, except one prior to performing this upgrade (Refer http://service.jfrog.org/wiki/Xray+and+Artifactory+One+to+One+Pairing for more details) ? [y/N]: y

    Installation Directory (Default: /root/.jfrog/xray): /opt/jfrog/xray

    Running system diagnostics checks, logs can be found at [/root/jfrog-xray-3.43.1-compose/systemDiagnostics.log]

    Triggering migration script. This will migrate if needed and may take some time.

    Migration logs will be available at [/root/jfrog-xray-3.43.1-compose/bin/migration.log]. The file will be archived at [/opt/jfrog/xray/var/log] after installation

    The JFrog URL allows Xray to connect to a JFrog Platform Instance.
    (You can copy the JFrog URL from Admin > Security > Settings)
    JFrog URL: http://192.168.56.110:8082/
    [WARN ] Error while initializing File resolver : Config file does not exists : var/etc/system.yaml
    Attempt to connect JFrogURL succeeded

    The Join key is the secret key used to establish trust between services in the JFrog Platform.
    (You can copy the Join Key from Admin > Security > Settings)
    Join Key: 


    For IPv6 address, enclose value within square brackets as follows : [<ipv6_address>]
    Please specify the IP address of this machine (Default: fe80::825:e011:a715:9470%enp0s8): 192.168.56.120

    Are you adding an additional node to an existing product cluster? [y/N]: n

    The installer can install a PostgreSQL database, or you can connect to an existing compatible PostgreSQL database
    (https://service.jfrog.org/installer/System+Requirements#SystemRequirements-RequirementsMatrix)
    If you are upgrading from an existing installation, select N if you have externalized PostgreSQL, select Y if not.
    Do you want to install PostgreSQL? [Y/n]: y

    To setup PostgreSQL, please enter a password
    database password: 

    confirm database password: 

    Creating third party directories (if necessary)

    Attempting to seed PostgreSQL. This may take some time.

    Successfully seeded PostgreSQL

    Docker setup complete

    Installation directory: [/opt/jfrog/xray] contains data and configurations.

    Use docker-compose commands to start the application. Once the application has started, it can be accessed at [http://192.168.56.110:8082/]

    Examples:
    cd /root/jfrog-xray-3.43.1-compose


    Rabbitmq is a dependent service which needs to be started once after install. This needs to be running before start of xray services.

    start rabbitmq:      docker-compose -p xray-rabbitmq -f docker-compose-rabbitmq.yaml up -d
    start postgresql:    docker-compose -p xray-postgres -f docker-compose-postgres.yaml up -d
    stop  postgresql:    docker-compose -p xray-postgres -f docker-compose-postgres.yaml down
    start:               docker-compose -p xray up -d
    stop:                docker-compose -p xray down

    NOTE: The compose file uses several environment variables from the .env file. Remember to run from within the [/root/jfrog-xray-3.43.1-compose] folder

    Done
    ```
3. 修改配置(Customize the product configuration)
    ```
    # vi /opt/jfrog/xray/var/etc/system.yaml
    configVersion: 1
        ......
        node:
          id: "xray1"
          ip: "192.168.xx.xx"
        ....
    ```
4. 启动Postgresql、Rabbitmq及Xray(Start and manage the Xray service.)
    ```
    docker-compose -p xray-rabbitmq -f docker-compose-rabbitmq.yaml up -d
    docker-compose -p xray-postgres -f docker-compose-postgres.yaml up -d
    docker-compose -p xray up -d
    ```
5. 查看日志(Check Xray Log.)
    ```
    # docker-compose -p xray logs -f
    ....
    router_1         | ###############################################################
    router_1         | ###   All services started successfully in 32.332 seconds   ###
    router_1         | ###############################################################
    ....
    ```
6. 访问Xray(Access Xray from your browser)
    >http://jfrogUrl