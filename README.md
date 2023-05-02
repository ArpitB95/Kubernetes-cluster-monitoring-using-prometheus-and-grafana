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

