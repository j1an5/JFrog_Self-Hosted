# 安装 JFrog Platform
本文将带你了解JFrog的订阅类型,并提供了几种安装方式。

## 步骤

1. 申请试用License的链接 [Start a Free Trial](https://www.jfrogchina.com/start-free/).
2. 确定要安装的产品
3. 查看架构和系统要求 [System Architecture](https://www.jfrog.com/confluence/display/JFROG/System+Architecture) and [System Requirements](https://www.jfrog.com/confluence/display/JFROG/System+Requirements)
4. 准备服务器

### JFrog各产品的安装顺序

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

不同订阅对应的产品
>安装JFrog平台,首先需要安装Artifactory

| Subscription Type | JFrog Products to Install |
| ---- | ---- |
| Artifactory OSS (Free) |JFrog Artifactory Open Source: Maven and Generic Package Manager Repository<br>[OSS安装包](https://jfrog.com/community/open-source/)|
| Artifactory CE (Free) | JFrog Artifactory Conan Edition: Conan C/C++ and Generic Package Manager Repository<br>[CE安装包](https://conan.io/downloads.html) |
| JFrog Container Registry (Free) | JFrog Container Registry (Powered by Artifactory): Docker, Helm and Generic Package Manager Repository<br>[Container安装包](https://jfrog.com/download-jfrog-container-registry/) |
| Pro | JFrog Artifactory: Universal Package Manager Repository |
| Pro X | JFrog Artifactory: Universal Package Manager Repository <br>JFrog Xray: Security and Compliance Scanning |
| Enterprise X | JFrog Artifactory: Universal Package Manager Repository <br>JFrog Xray: Security and Compliance Scanning |
| Enterprise+ | JFrog Artifactory: Universal Package Manager Repository <br>JFrog Insight: Manage DevOps Insights <br>JFrog Xray: Security and Compliance Scanning <br>JFrog Pipelines: CI/CD pipeline orchestration  <br>JFrog Distribution: Global software distribution |

## Artifactory 网络端口
8081和8082是外部的访问端口
其他端口：
| Microservice | Port |
| ---- | ---- |
| Artifactory | 8081 |
| Access | 8040 and 8045 |
| Web | 8070 |
| Replicator | 8048 and 9092 |
| Metadata | 8086 |
| Router | 8082, 8046, 8047, 8049, and 8091 |
| Events | 8061 and 8062 |
| Integration | 8071 and 8072 |
| JFConnect | 8030 |
| Observability | 8036 |
| gRPC | 8037 |

## Xray 网络端口
8082是外部的访问端口
其他端口：
| Microservice | Port |
| ---- | ---- |
| Xray Server | 8000 |
| Analysis | 7000 |
| Indexer | 7002 |
| Persist | 7003 |
| Router | 8082, 8046, 8047, and 8049 |
| RabbitMQ | 4369, 5671, 5672, 15672, and 25672 |
| PostgreSQL (bundled PostgreSQL) | 5432 |
| Observability | 8036 |
| gRPC | 8037 |

## 安装包及认证
>[安装包](https://jfrog.com/download-legacy/)<br>
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
>关闭IpV6 (在Docker compose 的安装方式中,不执行此步骤)
第一种方法是通过 /etc/sysctl.conf 文件对 /proc 进行永久修改。

```shell
sudo vi /etc/sysctl.conf
#禁用整个系统所有接口的IPv6
net.ipv6.conf.all.disable_ipv6 = 1
#禁用某一个指定接口的IPv6(例如：eth0, lo)
net.ipv6.conf.lo.disable_ipv6 = 1
net.ipv6.conf.eth0.disable_ipv6 = 1
```
>使这些更改生效，运行以下命令：

```shell
sudo sysctl -p /etc/sysctl.conf
```


![测试环境准备](./resource/images/%E6%B5%8B%E8%AF%95%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87.png)

## Single Node Installation

### [Linux Archive Installation](./Single%20Node%20Installation/Linux%20Archive%20Installation/README.md)
### [RPM Installation](./Single%20Node%20Installation/RPM%20Installation/README.md)
### [Docker Compose Installation](./Single%20Node%20Installation/Docker%20Compose%20Installation/README.md)
### [Helm Installation](./Single%20Node%20Installation/Helm%20Installation/README.md)

## Single Node Upgrade
> 升级方式和步骤依赖于安装方式，请选择一样的方式进行安装和升级。

### [Linux Archive Upgrade](./Upgrading%20JFrog%20Platform/README.md)
* [Artifactory](./Upgrading%20JFrog%20Platform/Artifactory%20-%20Linux%20Archive%20Upgrade/README.md)
* [Xray](./Upgrading%20JFrog%20Platform/Xray%20-%20Linux%20Archive%20Upgrade/README.md)

### [RPM Upgrade](./Upgrading%20JFrog%20Platform/README.md)
* [Artifactory](../Upgrading%20JFrog%20Platform/Artifactory%20-%20RPM%20Upgrade/README.md)
* [Xray](../Upgrading%20JFrog%20Platform/Xray%20-%20RPM%20Upgrade/README.md)


## 补充
>如果安装过程中遇到问题或启动失败,可以将以下内容发送到我的邮箱-不保证回复(jians0625@gmail.com)
- console.log
- journalctl -xe

## JFrog小助手和公众号
<img src="./resource/images/%E5%85%AC%E4%BC%97%E5%8F%B7%E4%BA%8C%E7%BB%B4%E7%A0%81.jpg" width = "100" height = "100" alt="" align=center />      <img src="./resource/images/%E5%B0%8F%E5%8A%A9%E6%89%8B%E4%BA%8C%E7%BB%B4%E7%A0%81.jpg" width = "100" height = "100" alt="" align=center />

