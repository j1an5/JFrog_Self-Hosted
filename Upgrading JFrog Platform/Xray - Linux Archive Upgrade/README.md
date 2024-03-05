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
    - 升级需要注意依赖包的版本，如果版本不匹配可能会报错；此外，Xray 3.70.0版本及更高版本依赖 RabbitMQ3.11.x及更高版本。且升级到 Rabbit MQ 3.11.x 之前，需要启用功能标志。
    - upgrade to the latest 3.8.x, 3.9.x or 3.10.x first
    - enable the feature flags
    - upgrade to RabbitMQ 3.11.0+
        ```
        sudo -H -u xray bash -c 'export RABBITMQ_FEATURE_FLAGS="drop_unroutable_metric,empty_basic_get_metric,implicit_default_bindings,maintenance_mode_status,quorum_queue,stream_queue,user_limits,virtual_host_metadata"' 
        sudo -H -u xray bash -c '/opt/jfrog/xray/app/third-party/rabbitmq/sbin/rabbitmqctl enable_feature_flag all'
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
