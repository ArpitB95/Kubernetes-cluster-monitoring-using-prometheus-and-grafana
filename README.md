# Kubernetes-cluster-monitoring-using-prometheus-and-grafana


# - This post walks through the process of setting up a Kubernetes cluster on AWS and configuring Prometheus and Grafana for monitoring performance metrics. Prometheus and Grafana are widely used open-source tools that help Site Reliability Engineers (SREs) measure site reliability.

- The post provides step-by-step instructions for installing command-line tools such as AWS CLI, eksctl, kubectl, and Helm Chart. Once the environment is set up, the post explains how to install and configure Prometheus and Grafana, including setting up custom dashboards. With this guide, readers can easily set up a reliable monitoring system for their Kubernetes cluster on AWS.

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

- Here, I've installed AWS CLI and eksctl in  Linux x86(64-bit) operating system.

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


