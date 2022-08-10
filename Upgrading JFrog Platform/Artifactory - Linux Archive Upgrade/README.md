# Upgrading JFrog Platform
## Artifactory - Linux Archive Upgrade

1. 设置变量(确定安装版本)
    ```
    export rt_new_version=7.41.7
    export JFROG_HOME=/opt/jfrog
    export JF_NEW_VERSION=/opt/jfrog/artifactory-pro-${rt_new_version}
    ```
2. 准备升级安装包
    ```
    cd $JFROG_HOME/
    wget https://releases.jfrog.io/artifactory/artifactory-pro/org/artifactory/pro/jfrog-artifactory-pro/${rt_new_version}/jfrog-artifactory-pro-${rt_new_version}-linux.tar.gz
    ```

3. 关闭Artifactory
    | Daemon Process |
    | ---- |
    | $JFROG_HOME/artifactory/app/bin/artifactoryctl stop |

4. 提取压缩包
    ```
    tar -xvf jfrog-artifactory-pro-${rt_new_version}-linux.tar.gz
    ```
5. 将app目录升级为新版本
    - 移除旧版本的app目录
        ```
        rm -rf $JFROG_HOME/artifactory/app
        ```
    - 拷贝新版本的app
        ```
        cp -r $JF_NEW_VERSION/app $JFROG_HOME/artifactory/
        ```
    - 删除解压后的安装包
        ```
        rm -rf $JF_NEW_VERSION
        ```
5. 启动服务
    | Daemon Process |
    | ---- |
    | $JFROG_HOME/artifactory/app/bin/artifactoryctl start |
6. 查看日志
    ```
    # tail -f /opt/jfrog/artifactory/var/log/console.log
    ....
    ###############################################################
    ###   All services started successfully in xx.xxx seconds   ###
    ###############################################################
    ....
    ```