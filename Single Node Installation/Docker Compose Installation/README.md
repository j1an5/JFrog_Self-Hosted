# Single Node Installation
## Docker Compose Installation

### 前置条件
#### Docker-ce [安装](https://docs.docker.com/compose/install/)
```
yum install -y yum-utils   device-mapper-persistent-data   lvm2
yum-config-manager   --add-repo    https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce  -y
systemctl start docker && systemctl enable docker 
```
#### Docker-compose [安装](https://mirror.tuna.tsinghua.edu.cn/help/docker-ce/)
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 
```
```
# docker-compose --version
docker-compose version 1.29.2, build 5becea4c
```

### Artifactory
1. 下载并解压,进入解压后的目录,本文使用的版本是7.33.9(Extract the contents of the compressed archive)
    ```
    wget https://releases.jfrog.io/artifactory/artifactory-pro/org/artifactory/pro/docker/jfrog-artifactory-pro/7.33.9/jfrog-artifactory-pro-7.33.9-compose.tar.gz
    tar -xvf jfrog-artifactory-pro-7.33.9-compose.tar.gz
    cd artifactory-pro-7.33.9
    ```
2. 执行交互脚本,自动生成配置文件(Run the script to setup folders with required ownership. This is an interactive script.)
    ```
    \# ./config.sh<br>
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
    ```
3. 修改配置(Customize the product configuration (optional) including database, Java Opts, and filestore.)
    ```
    # vim $JFROG_HOME/artifactory/var/etc/system.yaml
    shared:
    ....
      node:
        id: "art1"
        ip: "192.168.xx.xx”
    ....
    ```
4. 启停Artifactory及PostgreSQL(Manage Artifactory using native Docker Compose commands, docker-compose -p rt <action> command.<br>
Run this command from the extracted folder.)<br>
    >启动
    ```
    docker-compose -p rt-postgres -f docker-compose-postgres.yaml up -d
    docker-compose -p rt up -d
    ```
    >停止
    ```
    docker-compose -p rt down
    docker-compose -p rt-postgres -f docker-compose-postgres.yaml down

    ```
5. Check Artifactory Log.
    ```
    docker-compose -p rt logs
    ```
6. 访问Artifactory(Access Artifactory from your browser.)
    > http://SERVER_HOSTNAME:8082

### Xray
1. Extract the contents of the compressed archive and go to the extracted folder.
    ```
    # wget https://releases.jfrog.io/artifactory/jfrog-xray/xray-compose/3.43.1/jfrog-xray-3.43.1-compose.tar.gz
    # tar -xvf jfrog-xray-<version>-<compose|rpm|deb>.tar.gz
    # cd jfrog-xray-<version>-compose
    ```
2. Run the installer script.
    >![Artifactory Join Key 1](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%201.png?raw=true)
    ![Artifactory Join Key 2](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%202.png?raw=true)
    ```
    # ./config.sh
    ```
3. Customize the product configuration
    ```
    # vim $JFROG_HOME/xray/var/etc/system.yaml
    configVersion: 1
        ......
        node:
          id: "xray1"
          ip: "192.168.xx.xx"
        ....
    ```
4. Start and manage the Xray service.
    ```
    # docker-compose -p xray-rabbitmq -f docker-compose-rabbitmq.yaml up -d
    # docker-compose -p xray-postgres -f docker-compose-postgres.yaml up -d
    # docker-compose -p xray up -d
    # docker-compose -p xray ps
    # docker-compose -p xray down
    ```
5. Check Xray Log.
    ```
    # tail -f $JFROG_HOME/xray/var/log/console.log
    ```
6. Access Xray from your browser at: http://\<jfrogUrl>/ui/, go the Dashboard tab in the Application module in the UI.

