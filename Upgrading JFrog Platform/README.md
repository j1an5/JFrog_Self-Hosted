# Upgrading JFrog Platform

## 前置条件
1. 已安装Artifactory/Xray(单点)
2. Artifactory版本为7.x/Xray版本为3.x
3. 请尽可能使用原安装方式升级，如：安装时使用RPM方式，则升级时也使用RPM

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


### Single Node Upgrade - Artifactory

* [Artifactory - Linux Archive Upgrade](../Upgrading%20JFrog%20Platform/Artifactory%20-%20Linux%20Archive%20Upgrade/README.md)
* [Artifactory - RPM Upgrade](../Upgrading%20JFrog%20Platform/Artifactory%20-%20RPM%20Upgrade/README.md)
* [Artifactory - Docker Compose Upgrade - 待补充]()
* [Artifactory - Helm Upgrade - 待补充]()


### Single Node Upgrade - Xray

* [Xray - Linux Archive Upgrade](../Upgrading%20JFrog%20Platform/Xray%20-%20Linux%20Archive%20Upgrade/README.md)
* [Xray - RPM Upgrade](../Upgrading%20JFrog%20Platform/Xray%20-%20RPM%20Upgrade/README.md)
* [Xray - Docker Compose Upgrade - 待补充]()
* [Xray - Helm Upgrade - 待补充]()
