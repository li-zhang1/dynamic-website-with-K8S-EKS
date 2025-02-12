# Hosting a Dynamic Application with Kubernetes on AWS EKS

## Prerequisites
- AWS CLI installed and configured
- kubectl installed https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos
- eksctl installed  https://eksctl.io/installation/
- helm installed    https://helm.sh/docs/intro/install/
- Docker installed
- AWS IAM permissions to manage EKS, ECR, Route 53, ACM, and Secrets Manager

## 1. Create a Dockerfile and Build Docker Image Locally
Create a `Dockerfile` in your project root:

Build the Docker image:
```sh
docker build --platform linux/amd64 \
-t rentzone .
```
## 2. Push the Docker Image to Amazon ECR
Run the following command in your terminal to build docker image locally.
```sh
chmod +x build_image.sh
./build_image.sh
```

aws cli command to create an amazon ecr repository
```sh
aws ecr create-repository --repository-name rentzone --region us-east-1
```

retag docker image 
```sh
docker tag <image-tag> <repository-uri>
```

login to ecr
```sh
aws ecr get-login-password | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```

push docker image to ecr repository 
```sh
docker push <repository-uri>
```

## 3. Create EKS Cluster and Node Group on AWS management group.


## 4. Create Kubernetes YAML Files and deploy nodes to EKS worker nodes.
Link to yaml files and deployement instruction is [here](https://github.com/li-zhang1/eks-projects).

## 5. Configure Network load balancer and Target Groups
```sh
SERVICE_FILE_NAME="service.yaml"

kubectl apply -f $SERVICE_FILE_NAME
```
## 6. Configure Route 53 for a Custom Domain
Create an A record in Route 53 to point to the LoadBalancer DNS of the service.

## 7. Request an SSL Certificate with ACM

## 8. Store Secrets in AWS Secrets Manager


## Conclusion
Your application is now hosted on AWS EKS with a custom domain, SSL, and secret management. 🎉


