# Single Node Installation
## Helm Installation

### 前置条件
1. 演示环境[准备](https://github.com/j1an5/JFrog_Self-Hosted#%E6%BC%94%E7%A4%BA%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)
2. Docker-ce [安装](https://docs.docker.com/compose/install/)
    ```
    yum install -y yum-utils device-mapper-persistent-data lvm2
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install docker-ce -y
    systemctl start docker && systemctl enable docker 
    ```
    ```
    # docker --version
    Docker version 20.10.12, build e91ed57
    ```
3. Kubectl [安装-本文使用的是阿里镜像源](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management)
    ```
    cat <<EOF > /etc/yum.repos.d/kubernetes.repo
    [kubernetes]
    name=Kubernetes
    baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
    enabled=1
    gpgcheck=1
    repo_gpgcheck=1
    gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
    EOF
    setenforce 0
    yum install -y kubectl
    ```
    ```
    # kubectl version
    Client Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.4", GitCommit:"e6c093d87ea4cbb530a7b2ae91e54c0842d8308a", GitTreeState:"clean", BuildDate:"2022-02-16T12:38:05Z", GoVersion:"go1.17.7", Compiler:"gc", Platform:"linux/amd64"}
    The connection to the server localhost:8080 was refused - did you specify the right host or port?
    ```
4. Helm [安装](https://helm.sh/docs/intro/install/)
    ```
    wget https://get.helm.sh/helm-v3.8.0-linux-amd64.tar.gz
    tar -xf helm-v3.8.0-linux-amd64.tar.gz
    cp -rp linux-amd64/helm /usr/local/bin/helm
    ```
    ```
    # helm version
    version.BuildInfo{Version:"v3.8.0", GitCommit:"d14138609b01886f544b2025f5000351c9eb092e", GitTreeState:"clean", GoVersion:"go1.17.5"}
    ```
5. K8s [本文使用rancher安装k8s]()
    >1.运行Rancher
    ```
    docker pull rancher/rancher:latest
    docker run -d --restart=unless-stopped \
    -p 80:80 -p 443:443 \
    --privileged \
    rancher/rancher:latest
    ```
    >2.安装K8s机群<br>
    Clusters -> Create -> Custom -> Cluster Name -> Next -> "etcd + Control Plane + Worker"<br>
    Copy -> End.<br>
    ![K8s create 1](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/K8s%20create%201.png)
    ![K8s create 2](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/K8s%20create%202.png)
    ![K8s create 3](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/K8s%20create%203.png)
    ![K8s create 4](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/K8s%20create%204.png)
    ![K8s create 5](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/K8s%20create%205.png)
 
    >3.配置kubectl config<br>
    Download KubeConfig -> 保存到 ~/.kube/config中<br>
    ![k8s config file](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/K8s%20config%20file.png)
    ```
    # kubectl config view
    apiVersion: v1
    clusters:
    - cluster:
        certificate-authority-data: DATA+OMITTED
        server: https://192.168.xx.xxx/k8s/clusters/c-l82z7
      name: k8s
    - cluster:
        certificate-authority-data: DATA+OMITTED
        server: https://10.0.x.xx:6443
      name: k8s-bogon
    contexts:
    - context:
        cluster: k8s
        user: k8s
      name: k8s
    - context:
        cluster: k8s-bogon
        user: k8s
      name: k8s-bogon
    current-context: k8s
    kind: Config
    preferences: {}
    users:
    - name: k8s
      user:
        token: REDACTED
    ```
6. 安装Longhorn (作为K8s默认存储) [安装参考](https://longhorn.io/docs/1.2.3/deploy/install/install-with-kubectl/)
    ```
    yum install iscsi-initiator-utils wget -y
    wget https://raw.githubusercontent.com/longhorn/longhorn/v1.2.3/deploy/longhorn.yaml
    kubectl apply -f ./longhorn.yaml
    kubectl patch storageclass longhorn -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
    ```
    ```
    # kubectl get pods \
    --namespace longhorn-system 
    NAME                                        READY   STATUS    RESTARTS   AGE
    csi-attacher-5f46994f7-5bhq2                1/1     Running   0          2m39s
    csi-attacher-5f46994f7-fkdgd                1/1     Running   0          2m39s
    csi-attacher-5f46994f7-vcnrl                1/1     Running   0          2m39s
    csi-provisioner-6ccbfbf86f-bwbsh            1/1     Running   0          2m39s
    csi-provisioner-6ccbfbf86f-hpffg            1/1     Running   0          2m39s
    csi-provisioner-6ccbfbf86f-lhksv            1/1     Running   0          2m39s
    csi-resizer-6dd8bd4c97-5kg57                1/1     Running   0          2m39s
    csi-resizer-6dd8bd4c97-s8wlj                1/1     Running   0          2m39s
    csi-resizer-6dd8bd4c97-zndld                1/1     Running   0          2m39s
    csi-snapshotter-86f65d8bc-8xwcg             1/1     Running   0          2m38s
    csi-snapshotter-86f65d8bc-9vqdv             1/1     Running   0          2m38s
    csi-snapshotter-86f65d8bc-xzfdm             1/1     Running   0          2m38s
    engine-image-ei-fa2dfbf0-hdjtq              1/1     Running   0          2m48s
    instance-manager-e-a39aaa4c                 1/1     Running   0          2m48s
    instance-manager-r-d695cbcb                 1/1     Running   0          2m47s
    longhorn-csi-plugin-5qqpw                   2/2     Running   0          2m38s
    longhorn-driver-deployer-784546d78d-pgx9c   1/1     Running   0          3m4s
    longhorn-manager-fs6wj                      1/1     Running   0          3m5s
    longhorn-ui-9fdb94f9-5nlrq                  1/1     Running   0          3m4s
    ```

### Artifactory
1. 设置Helm仓库(Add the ChartCenter Helm repository to your Helm client.)
    ```
    helm repo add jfrog-charts https://releases.jfrog.io/artifactory/jfrog-charts
    helm repo update
    ```
2. 安装Artifactory,本文使用的版本是107.33.9-对应RPM的7.33.9版本(Install the chart with the release name artifactory)
    ```
    kubectl create namespace artifactory
    helm upgrade --install artifactory jfrog-charts/artifactory \
        --namespace artifactory \
        --set postgresql.enabled=false \
        --set nginx.service.type="NodePort"  \
        --version 107.33.9
    ```
3. 查看日志(To access the logs, find the name of the pod using this command.)
    ```
    kubectl --namespace artifactory get pods
    kubectl --namespace artifactory describe pod <name of the pod>
    kubectl --namespace artifactory logs -f <name of the pod>
    ```
4. 访问Artifactory(Connect to Artifactory.)
    ```
    export NODE_PORT=$(kubectl get --namespace artifactory -o jsonpath="{.spec.ports[0].nodePort}" services artifactory-artifactory-nginx)
    export NODE_IP=$(kubectl get nodes --namespace artifactory -o jsonpath="{.items[0].status.addresses[0].address}")
    echo http://$NODE_IP:$NODE_PORT/
    ```
    > http://NODE_IP:NODE_PORT


### Xray
1. 安装xray,本文使用的版本是103.43.1,对应RPM的3.43.1版本(Initiate installation by providing a join key and JFrog url as a parameter to the Xray chart installation.)<br>
    * 参数设置[参考](https://github.com/jfrog/charts/blob/master/stable/xray/values.yaml)
    >![Artifactory Join Key 1](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%201.png?raw=true)
    ![Artifactory Join Key 2](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%202.png?raw=true)
    ```
    kubectl create namespace xray
    helm upgrade --install xray jfrog-charts/xray \
        --namespace xray \
        --set common.persistence.size=50Gi \
        --set postgresql.persistence.size=150Gi \
        --set rabbitmq.persistence.size=20Gi \
        --set xray.joinKey=<RT_JOIN_KEY> \
        --set xray.jfrogUrl=<RT_BASE_URL> \
        --version 103.43.1
    ```
4. 检查状态及日志(Check the status of your deployed Helm release.)
    ```
    helm status xray
    ```
5. 访问Xray(Access Xray from your browser)
    >http://jfrogUrl


### 常见问题
1. Xray启动:<br>
    >报错:<br>
    Warning  FailedAttachVolume  15s (x7 over 51s)  attachdetach-controller  AttachVolume.Attach failed for volume "pvc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2" : rpc error: code = Aborted desc = volume pvc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 is not ready for workloads<br>
    原因:<br>
    pvc无法正常提供服务,如-<br>
    pvc2需要的空间为150Gi,实际磁盘只有73G剩余,无法满足Xray所需,处理方案:
    ```
    # kubectl --namespace xray get pvc
    NAME                     STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
    data-volume-xray-0       Bound    pvc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1   50Gi       RWO            longhorn       88s
    data-xray-postgresql-0   Bound    pvc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2   150Gi      RWO            longhorn       88s
    data-xray-rabbitmq-0     Bound    pvc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx3   20Gi       RWO            longhorn       88s
    # df -hT /
    Filesystem              Type  Size  Used Avail Use% Mounted on
    /dev/mapper/centos-root xfs   200G  123G   73G  62% /
    ```
    >方案一: 为pvc提供足够的空间,磁盘扩容(或更换为磁盘大的虚机,或增加新的k8s节点)<br>
    方案二: 清理pvc后,设置postgresql.persistence.size=50Gi,可以启动成功,但是后续使用Xray扫描功能受影响,清理操作如下:
    ```
    helm --namespace xray delete xray
    kubectl --namespace xray delete pvc data-volume-xray-0 data-xray-postgresql-0 data-xray-rabbitmq-0
    ```

