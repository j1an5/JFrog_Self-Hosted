# Single Node Installation
## Docker Compose Installation

### Artifactory
1. Extract the contents of the compressed archive (.tar.gz file) and then go to the extracted folder.
    ```
    # wget https://releases.jfrog.io/artifactory/artifactory-pro/org/artifactory/pro/docker/jfrog-artifactory-pro/7.33.9/jfrog-artifactory-pro-7.33.9-compose.tar.gz
    # tar -xvf jfrog-artifactory-pro-<version>-compose.tar.gz
    # cd jfrog-artifactory-pro-<version>
    ```
2. Run the script to setup folders with required ownership. This is an interactive script.
    ```
    # ./config.sh
    ```
3. Customize the product configuration (optional) including database, Java Opts, and filestore.
    ```
    # vim $JFROG_HOME/artifactory/var/etc/system.yaml
    shared:
    ....
      node:
        id: "art1"
        ip: "192.168.xx.xx”
    ....
    ```
4. Manage Artifactory using native Docker Compose commands, docker-compose -p rt <action> command.<br>
Run this command from the extracted folder.
    ```
    # docker-compose -p rt-postgres -f docker-compose-postgres.yaml up -d
    # docker-compose -p rt up -d
    # docker-compose -p rt ps
    # docker-compose -p rt down
    ```
6. Check Artifactory Log.
    ```
    # docker-compose -p rt logs
    ```
7. Access Artifactory from your browser at: http://SERVER_HOSTNAME:8082/ui/. For example, on your local machine: http://localhost:8082/ui/.

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

