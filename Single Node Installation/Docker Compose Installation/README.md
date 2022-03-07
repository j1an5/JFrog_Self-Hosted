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
1. 下载并解压,进入解压后的目录(Extract the contents of the compressed archive)
    ```
    wget https://releases.jfrog.io/artifactory/artifactory-pro/org/artifactory/pro/docker/jfrog-artifactory-pro/7.33.9/jfrog-artifactory-pro-7.33.9-compose.tar.gz
    tar -xvf jfrog-artifactory-pro-<version>-compose.tar.gz
    cd jfrog-artifactory-pro-<version>
    ```
2. 执行交互脚本,自动生成配置文件(Run the script to setup folders with required ownership. This is an interactive script.)
    ```
    ./config.sh
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

