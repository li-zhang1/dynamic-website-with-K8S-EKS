Here’s a reformatted version of your README file to make it more readable and structured:

---

# Hosting a Dynamic Application with Kubernetes on AWS EKS

## Prerequisites

Ensure you have the following tools installed and configured:

- **AWS CLI**: Installed and configured
- **kubectl**: [Installation Guide](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos)
- **eksctl**: [Installation Guide](https://eksctl.io/installation/)
- **Helm**: [Installation Guide](https://helm.sh/docs/intro/install/)
- **Docker**: Installed
- **AWS IAM Permissions**: Permissions to manage EKS, ECR, Route 53, ACM, and Secrets Manager

---

## 1. Create a Dockerfile and Build Docker Image Locally

Create a `Dockerfile` in your project root directory. Then, build the Docker image using the following command:

```sh
docker build --platform linux/amd64 -t rentzone .
```

---

## 2. Push the Docker Image to Amazon ECR

### Build the Docker Image Locally

Run the following commands to make the build script executable and run it:

```sh
chmod +x build_image.sh
./build_image.sh
```

### Create an Amazon ECR Repository

Create a new ECR repository using AWS CLI:

```sh
aws ecr create-repository --repository-name rentzone --region us-east-1
```

### Retag Docker Image

Retag your Docker image with the repository URI:

```sh
docker tag <image-tag> <repository-uri>
```

### Login to Amazon ECR

Log in to your ECR registry using the following command:

```sh
aws ecr get-login-password | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```

### Push Docker Image to ECR Repository

Push your Docker image to the ECR repository:

```sh
docker push <repository-uri>
```

---

## 3. Create EKS Cluster and Node Group

Create an EKS cluster and worker nodes using AWS Management Console or `eksctl`. Refer to the official [eksctl documentation](https://eksctl.io/) for detailed steps.

---

## 4. Create Kubernetes YAML Files and Deploy to EKS

You can find the YAML files for Kubernetes deployment and detailed instructions on deploying to EKS [here](https://github.com/li-zhang1/eks-projects).

---

## 5. Configure Network Load Balancer and Target Groups

Once the service is deployed, create a Kubernetes service that exposes your application to the outside world. Apply the service YAML file:

```sh
SERVICE_FILE_NAME="service.yaml"
kubectl apply -f $SERVICE_FILE_NAME
```

---

## 6. Configure Route 53 for a Custom Domain

Create an **A Record** in **Route 53** to point to the Load Balancer DNS name associated with your service.

---

## 7. Request an SSL Certificate with ACM

Request an SSL certificate via **AWS ACM** for your custom domain. Follow the AWS [ACM Documentation](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) for detailed steps.

---

## 8. Store Secrets in AWS Secrets Manager

Use **AWS Secrets Manager** to securely store and manage sensitive information, such as database credentials, API keys, etc.

---

## Conclusion

Your dynamic application is now hosted on **AWS EKS**, complete with a custom domain, SSL encryption, and secret management. 🎉

---

This format breaks the instructions down into clear, actionable steps with headings that make it easier to follow and locate relevant sections. Let me know if you need further improvements!