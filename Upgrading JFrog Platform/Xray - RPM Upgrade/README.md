# Upgrading JFrog Platform
## Xray - RPM Upgrade


1. 准备安装包（先确定安装版本）
    ```
    export xr_new_version=3.54.5
    wget https://releases.jfrog.io/artifactory/jfrog-xray/xray-rpm/${xr_new_version}/jfrog-xray-${xr_new_version}-rpm.tar.gz
    ```
2. 停止Xray服务
    ```
    systemctl start xray
    ```
3. 解压并进入Xray目录
    ```
    tar -xvf jfrog-xray-${xr_new_version}-rpm.tar.gz
    cd jfrog-xray-${xr_new_version}-rpm
    ```
4. 升级并启动Xray
    ```
    rpm -Uvh ./xray/xray.rpm
    systemctl start xray
    ```
5. 查看启动日志
    ```
    # tail -f $JFROG_HOME/xray/var/log/console.log
    ....
    ###############################################################
    ###   All services started successfully in xx.xxx seconds   ###
    ###############################################################
    ....
    ```

    