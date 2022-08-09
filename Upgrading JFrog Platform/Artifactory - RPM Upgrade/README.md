# Upgrading JFrog Platform
## Artifactory - RPM Upgrade


1. 准备升级安装包(确定安装版本)
    ```
    export rt_new_version=7.41.7
    wget https://releases.jfrog.io/artifactory/artifactory-pro-rpms/jfrog-artifactory-pro/jfrog-artifactory-pro-${rt_new_version}.rpm
    ```

2. 关闭Artifactory
    ```
    systemctl stop artifactory
    ```
3. 使用root用户升级Artifactory
    ```
    rpm -Uvh jfrog-artifactory-pro-${rt_new_version}.rpm
    ```
4. 启动服务
    ```
    systemctl start artifactory
    ```
5. 查看日志
    ```
    # tail -f /opt/jfrog/artifactory/var/log/console.log
    ....
    ###############################################################
    ###   All services started successfully in xx.xxx seconds   ###
    ###############################################################
    ....
    ```