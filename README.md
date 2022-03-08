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

## Download
### [历史版本下载链接](https://jfrog.com/download-legacy/)
>默认账户和密码(Default credential for Artifactory:)<br>
user: admin<br>
password: password
## 演示环境准备
Cenotos7.x虚拟机  2台 [(VirtualBox提供)](https://github.com/alexwang66/Guestbook-microservices-k8s/blob/master/Virtualbox安装虚拟机配置双网卡.md)
>虚机的推荐配置如下,所有磁盘空间都分给"/"目录<br>
>* 2c4g100G for Artifactory<br>
>* 4c8g200G for Xray<br>
>* 8c16g300G for K8s<br>

>服务器时区和时间配置
```shell
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
yum -y install ntpdate net-tools
/usr/sbin/ntpdate ntp1.aliyun.com
```
>设置定时任务:同步服务器时间
```shell
echo "*/10 * * * * /usr/sbin/ntpdate ntp1.aliyun.com" >> /var/spool/cron/root
```
>关闭防火墙 (在Docker compose 的安装方式中,不执行此步骤)
```shell
systemctl disable firewalld && systemctl stop firewalld
```
![测试环境准备](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/%E6%B5%8B%E8%AF%95%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87.png)

## Single Node Installation

### [Linux Archive Installation](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/Single%20Node%20Installation/Linux%20Archive%20Installation/README.md)
### [RPM Installation](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/Single%20Node%20Installation/RPM%20Installation/README.md)
### [Docker Compose Installation](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/Single%20Node%20Installation/Docker%20Compose%20Installation/README.md)
### [Helm Installation](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/Single%20Node%20Installation/Helm%20Installation/README.md)


## JFrog小助手和公众号
<img src="https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/%E5%85%AC%E4%BC%97%E5%8F%B7%E4%BA%8C%E7%BB%B4%E7%A0%81.jpg" width = "100" height = "100" alt="" align=center />      <img src="https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/%E5%B0%8F%E5%8A%A9%E6%89%8B%E4%BA%8C%E7%BB%B4%E7%A0%81.jpg" width = "100" height = "100" alt="" align=center />

