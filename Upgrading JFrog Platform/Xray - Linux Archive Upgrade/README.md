# Upgrading JFrog Platform
## Xray - Linux Archive Upgrade
1. 设置环境变量并确定安装版本
    ```
    export xr_new_version=3.54.5
    export JFROG_HOME=/opt/jfrog
    export JF_NEW_VERSION=/opt/jfrog/jfrog-xray-${xr_new_version}-linux
    ```
2. 准备升级安装包
    ```
    cd $JFROG_HOME/
    wget https://releases.jfrog.io/artifactory/jfrog-xray/xray-linux/${xr_new_version}/jfrog-xray-${xr_new_version}-linux.tar.gz
    ```
 
3. 停止Xray服务
    | Daemon Process |
    | ---- |
    | $JFROG_HOME/xray/app/bin/xray.sh stop |

4. 提取Xray压缩包
    ```
    tar -xvf jfrog-xray-${xr_new_version}-linux.tar.gz
    ```

5. 升级Xray
    - 移除旧的app目录
        ```
        rm -rf $JFROG_HOME/xray/app
        ```
    - 拷贝新的app
        ```
        cp -fr $JF_NEW_VERSION/app $JFROG_HOME/xray/
        ```
    - 移除解压的安装包
        ```
        rm -rf $JF_NEW_VERSION
        ```
6. 启动Xray
    | Daemon Process |
    | ---- |
    | $JFROG_HOME/xray/app/bin/xray.sh start |
7. 检查启动日志(Check Xray Log.)
    ```termminal
    # tail -n100 -f $JFROG_HOME/xray/var/log/console.log
    ###############################################################
    ###   All services started successfully in xx.xxx seconds   ###
    ###############################################################
    ```
