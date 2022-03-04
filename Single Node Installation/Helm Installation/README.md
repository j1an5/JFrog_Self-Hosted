# Single Node Installation
## Helm Installation

### Artifactory
1. Add the ChartCenter Helm repository to your Helm client.
    ```
    # helm repo add jfrog-charts https://releases.jfrog.io/artifactory/jfrog-charts
    ```
2. Update the repository.

    ```
    # helm repo update
    ```
3. Install the chart with the release name artifactory and with master key and join key.
    ```
    # kubectl create namespace <your namespace>
    # helm upgrade --install artifactory jfrog-charts/artifactory \
        --namespace <your namespace> \
        --set postgresql.enabled=false \
        --set nginx.service.type="NodePort"  \
        --version 107.33.9
    ```
4. To access the logs, find the name of the pod using this command.
    ```
    # kubectl --namespace <your namespace> get pods
    # kubectl --namespace <your namespace> logs -f <name of the pod>
    ```
5. Connect to Artifactory.
    ```
    # export NODE_PORT=$(kubectl get --namespace <your namespace> -o jsonpath="{.spec.ports[0].nodePort}" services artifactory-artifactory-nginx)
    # export NODE_IP=$(kubectl get nodes --namespace artifactory -o jsonpath={.items[0].status.addresses[0].address}")
    # echo http://$NODE_IP:$NODE_PORT/
    ```


### Xray
1. Add the ChartCenter Helm repository to your Helm client.
    ```
    # helm repo add jfrog-charts https://releases.jfrog.io/artifactory/jfrog-charts
    ```
2. Update the repository.

    ```
    # helm repo update
    ```
3. Initiate installation by providing a join key and JFrog url as a parameter to the Xray chart installation.
    >![Artifactory Join Key 1](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%201.png?raw=true)
    ![Artifactory Join Key 2](https://github.com/j1an5/JFrog_Self-Hosted/blob/main/resource/images/Artifactory%20Join%20Key%202.png?raw=true)
    ```
    # kubectl create namespace <your namespace>
    # helm upgrade --install xray jfrog/xray \
        --namespace <your namespace> \
        --set postgresql.persistence.size=150Gi \
        --set xray.joinKey=<RT_JOIN_KEY> \
        --set xray.jfrogUrl=<RT_BASE_URL> \
        --version 103.43.1
    ```
4. Check the status of your deployed Helm release.
    ```
    # helm status xray
    ```
5. Access Xray from your browser at: http://<jfrogUrl>/ui/: go to the Xray Security & Compliance tab in the Administration module in the UI