# Kubernetes-cluster-monitoring-using-prometheus-and-grafana


- This repo walks through the process of setting up a Kubernetes cluster on AWS and configuring Prometheus and Grafana for monitoring performance metrics. Prometheus and Grafana are widely used open-source tools that help Site Reliability Engineers (SREs) measure site reliability.

- The repo provides step-by-step instructions for installing command-line tools such as AWS CLI, eksctl, kubectl, and Helm Chart. Once the environment is set up, the post explains how to install and configure Prometheus and Grafana, including setting up custom dashboards. With this guide, readers can easily set up a reliable monitoring system for their Kubernetes cluster on AWS.

![dia](https://user-images.githubusercontent.com/110182832/235635545-444919cf-f828-4088-82fd-239f340700a9.PNG)


## In order to accomplish the task, we will follow these steps:

1) Install AWS CLI and eksctl
2) Setup AWS Credentials
3) Install kubectl
4) Install Helm chart
5) Create Kubernetes cluster in AWS using eksctl
6) Install Kubernetes Metrics Server
7) Install Prometheus
8) Install grafana
9) Import Grafana dashboard from Grafana Labs

----------

## 1) Install AWS CLI and eksctl:

- Here, I've used Linux x86(64-bit) operating system.

- Enter the following command to download the AWS CLI installation package:
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```
- Enter the following command to unzip the package:
```
unzip awscliv2.zip
```
- Enter the following command to run the installation script:
```
sudo ./aws/install
```

- To check the installation version, enter the following command:
```
aws --version
```

## Install eksctl:
- Enter the following command to download the eksctl package:
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
```

- Enter the following command to move the eksctl binary to the appropriate directory:
```
sudo mv /tmp/eksctl /usr/local/bin
```

- To verify the installation, enter the following command:
```
eksctl version
```

## 2) Setup AWS Credentials

- To proceed with setting up the Kubernetes cluster, the next step is to set up the AWS credentials, which will enable the usage of eksctl and kubectl command-line tools for managing the cluster. Here is a step-by-step guide for fetching the AWS credentials:
- Log in to your AWS account.
- Go to Security Credentials.
- Click on Access Keys.
- Create a new Access Key.
- Open the terminal/command prompt.
- Run the command "aws configure".
- Enter the AWS Access Key ID and AWS secret access key when prompted.

```
AWS Access Key ID [None]: <ENTER_YOU_ACCESS_KEY>
AWS Secret Access Key [None]: <ENTER_ACCESS_KEY>
Default region name [None]: <ENTER_THE_REGION>
Default output format [None]: json
```

## 3) Install kubectl
- The next tool which we are going to install is the kubectl for managing the Kubernetes cluster.
- Run the following command to download the kubectl binary file:
```
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
```
- Change the permission of the downloaded binary file to make it executable using the following command:
```
chmod +x ./kubectl 
```
- Move the kubectl binary file to /usr/local/bin directory with the following command:
```
sudo mv ./kubectl /usr/local/bin
```

- Verify the kubectl installation by running the following command:
```
kubectl version
```
![kubectl version](https://user-images.githubusercontent.com/110182832/235617647-b8dd4712-6d51-4bae-a67e-1aa83966da58.PNG)


## 4) Install Helm chart
- Run the following command to download the Helm installation script:
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
```
- Make the script executable by running:
```
chmod 700 get_helm.sh
```

- Run the script to install Helm:
```
./get_helm.sh
```

- Verify helm installation by running "helm" command


![helm installation checking](https://user-images.githubusercontent.com/110182832/235619696-a0c567c7-22a0-4fe5-a8fa-95f67d77bc91.PNG)


## 5) Create Kubernetes cluster in AWS using eksctl
- Type the following command to create a new Kubernetes cluster:
```
eksctl create cluster --name test-cluster-1 --version 1.22 --region eu-north-1 --nodegroup-name worker-nodes --node-type t2.large --nodes 2 --nodes-min 2 --nodes-max 3
```
- This command creates a Kubernetes cluster with the following specifications:

- Cluster name: test-cluster-1
- Kubernetes version: 1.22
- AWS region: eu-central-1
- Node group name: worker-nodes
- Node instance type: t2.large
- Number of nodes: 2
- Minimum number of nodes: 2
- Maximum number of nodes: 3
You can adjust these values based on your requirements.

- Wait for the cluster to be created. This may take a few minutes.
- You can monitor the progress of your cluster creation by typing the following command:
```
eksctl get cluster --name test-cluster-1
```
- This command will show you the status of your cluster. Wait until the STATUS field shows ACTIVE.
- You can check your cluster in AWS console,
- go to your AWS Account, search for eks 

- ![eks cluster](https://user-images.githubusercontent.com/110182832/235621824-c91cea3a-87aa-45be-a8e3-8ba1aaa626aa.PNG)


## 6) Install Kubernetes Metrics Server

- To enable Prometheus to collect the performance metrics of Kubernetes, we need to install the Kubernetes Metrics server on the Kubernetes cluster. The Metrics server is responsible for collecting resource metrics from kubelets and making them available to the Kubernetes API server through the Metrics API. These metrics are used by the Horizontal Pod Autoscaler and Vertical Pod Autoscaler.

- Use the following installation script to install kubernetes metrics server
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
- Verify the Kubernetes metric server installation
```
kubectl get pods
```
OR 
```
 kubectl get pods -n kube-system
 ```
 
## 7) Install Prometheus

- Now, install prometheus using helm chart
- Add Prometheus helm chart repository
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts 
```
- Update the helm chart repository
```
helm repo update
```
- Create prometheus namespace
```
kubectl create namespace prometheus
```
- Install the prometheus
```
helm install prometheus prometheus-community/prometheus \
    --namespace prometheus \
    --set alertmanager.persistentVolume.storageClass="gp2" \
    --set server.persistentVolume.storageClass="gp2" 
```

![Inkedgrafana](https://github.com/ArpitB95/Kubernetes-cluster-monitoring-using-prometheus-and-grafana/assets/110182832/e50f47bc-7037-4367-9158-9c0457c8d102)




## 8) Install grafana
- The same way install grafana using helm 
- Add the Grafana helm chart repository
```
helm repo add grafana https://grafana.github.io/helm-charts 
```
- Update the helm chart repository
```
helm repo update 
```
- To enable Grafana to access the Kubernetes metrics, we need to create a Prometheus data source. To do this, we will create a YAML file named prometheus-datasource.yaml and add the following data source configuration to it:

```
datasources:
  datasources.yaml:
    apiVersion: 1
    datasources:
    - name: Prometheus
      type: prometheus
      url: http://prometheus-server.prometheus.svc.cluster.local
      access: proxy
      isDefault: true
```
- This configuration specifies the name of the data source as Prometheus and the type as Prometheus. It also includes the URL of the Prometheus server and specifies that access should be done through a proxy. Finally, it sets this data source as the default.

- Create a namespace grafana
```
kubectl create namespace grafana
```
- Install Grafana
```
helm install grafana grafana/grafana \
    --namespace grafana \
    --set persistence.storageClassName="gp2" \
    --set persistence.enabled=true \
    --set adminPassword='EKS!sAWSome' \
    --values prometheus-datasource.yaml \
    --set service.type=LoadBalancer 
```

- Verify the Grafana installation by using the following kubectl command
```
kubectl get all -n grafana
```

![Inkedgrafana-1](https://github.com/ArpitB95/Kubernetes-cluster-monitoring-using-prometheus-and-grafana/assets/110182832/b8177f61-2412-429b-bd5c-1ccaaab19701)



- Access grafana through your browser
- get this AWS public IP by running
```
kubectl get service -n grafana 
```

![Inkedgrafana-2](https://github.com/ArpitB95/Kubernetes-cluster-monitoring-using-prometheus-and-grafana/assets/110182832/d4094b3e-aa7f-4ea4-a873-b43deac01319)


- copy EXTERNEL IP and paste it in your browser
- A new grafana login page will appear

![Inkedgrafana dashboard](https://github.com/ArpitB95/Kubernetes-cluster-monitoring-using-prometheus-and-grafana/assets/110182832/21118142-8f90-4d0c-8f05-09dfe6eeeaaf)


- Enter username & password as mentioned in step-8 (username- admin , password- EKS!sAWSome )

## 9) Import Grafana dashboard from Grafana Labs
- we can import any dashboard which has already created (use this link to check dashboards: https://grafana.com/grafana/dashboards/ )
- we can import dashboard by its ID, here I have imported a dashboard ID 6417

![Inkedgrafana dashboard id](https://user-images.githubusercontent.com/110182832/235632924-c264e162-a7d6-442d-9b17-6d2d3fafd90c.jpg)

- after giving ID, click import and select prometheus as a data source

- Your final dashboard will look like this: 

![dashboard-1](https://user-images.githubusercontent.com/110182832/235633187-d69f2182-30cc-41d9-b634-8b9780882a2a.PNG)

- It shows metrics of your kubernetes cluster.
