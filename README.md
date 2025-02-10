# Turn Your Data into an AI Chatbot (RAG) using Amazon Bedrock

This project shows you how to transform your data into an AI-powered chatbot using a Retrieval Augmented Generation (RAG) approach with Amazon Bedrock. The solution leverages AWS Cloud9, EC2, DynamoDB, EKS, Istio, and Cognito to deploy a scalable, multi-tenant chatbot.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
  - [1. Create a Cloud9 Environment](#1-create-a-cloud9-environment)
  - [2. Create an EC2 Instance Role](#2-create-an-ec2-instance-role)
  - [3. Configure the Cloud9 Environment](#3-configure-the-cloud9-environment)
  - [4. Clone the Repository & Run Setup Script](#4-clone-the-repository--run-setup-script)
  - [5. Create and Populate DynamoDB Tables](#5-create-and-populate-dynamodb-tables)
  - [6. Create the EKS Cluster](#6-create-the-eks-cluster)
  - [7. Deploy Istio Service Mesh](#7-deploy-istio-service-mesh)
  - [8. Deploy Cognito User Pools](#8-deploy-cognito-user-pools)
  - [9. Configure Istio Ingress Gateway](#9-configure-istio-ingress-gateway)
  - [10. Deploy Tenant Application Microservices](#10-deploy-tenant-application-microservices)
  - [11. Access the Chatbot Application](#11-access-the-chatbot-application)
- [Cleanup](#cleanup)
- [License](#license)
- [Contributing](#contributing)
- [Support](#support)

## Overview

This solution deploys a multi-tenant AI chatbot that uses a Retrieval Augmented Generation (RAG) approach to retrieve context from your data and generate accurate responses using Amazon Bedrock models. Follow the instructions below to set up your environment and deploy the chatbot.

## Prerequisites

- An AWS account with an IAM user that has Administrator privileges
- AWS Cloud9 (recommended: Amazon Linux 2 environment)
- AWS CLI and Git installed and configured

## Setup Instructions

### 1. Create a Cloud9 Environment

- Log in to the AWS Console and navigate to [Cloud9](https://console.aws.amazon.com/cloud9/).
- Create a new environment:
  - **Name:** (Choose a descriptive name)
  - **Instance type:** `t3.small`
  - **Platform:** Amazon Linux 2

### 2. Create an EC2 Instance Role

- In the IAM Console, create a new role for EC2.
- Attach the **AdministratorAccess** policy.
- Name the role (e.g., `Cloud9AdminRole`).

### 3. Configure the Cloud9 Environment

- In Cloud9, open **Settings** (gear icon).
- Under **AWS Settings → Credentials**, disable AWS Managed Temporary Credentials.
- Click the round icon in the upper right, choose **Manage EC2 Instance**, and attach the `Cloud9AdminRole` to the Cloud9 instance.

### 4. Clone the Repository & Run Setup Script

Open a new terminal in Cloud9 and execute:

```bash
git clone https://github.com/aws-samples/multi-tenant-chatbot-using-rag-with-amazon-bedrock.git
cd multi-tenant-chatbot-using-rag-with-amazon-bedrock
chmod +x setup.sh
./setup.sh
```

This script installs Kubernetes tools, updates the AWS CLI, and sets up other dependencies.

### 5. Create and Populate DynamoDB Tables

In a new terminal, navigate to the project directory and run:

```bash
chmod +x create-dynamodb-table.sh
./create-dynamodb-table.sh
```

This creates the required DynamoDB tables for session management and chat history.

### 6. Create the EKS Cluster

Run the following command to generate the cluster configuration and provision the cluster (this may take approximately 30 minutes):

```bash
chmod +x deploy-eks.sh
./deploy-eks.sh
```

### 7. Deploy Istio Service Mesh

After the EKS cluster is ready, deploy Istio by running:

```bash
chmod +x deploy-istio.sh  
./deploy-istio.sh
```

### 8. Deploy Cognito User Pools

Open a new terminal, navigate back to the project directory, and run:

```bash
chmod +x deploy-userpools.sh
./deploy-userpools.sh
```

Follow the prompts to set up user pools for sample tenants (e.g., `tenanta` and `tenantb`).

### 9. Configure Istio Ingress Gateway

Configure TLS and deploy necessary namespaces and gateway resources:

```bash
chmod +x configure-istio.sh
./configure-istio.sh
```

### 10. Deploy Tenant Application Microservices

Deploy the tenant services by running:

```bash  
chmod +x deploy-tenant-services.sh
./deploy-tenant-services.sh
```

### 11. Access the Chatbot Application

- Run the following to generate the hosts file entry:

  ```bash
  chmod +x hosts-file-entry.sh
  ./hosts-file-entry.sh  
  ```

- Copy the output and append it to your local hosts file.
- To avoid TLS certificate warnings, use a separate browser profile (e.g., in Firefox via `about:profiles`).
- Open the URLs:
  - `https://tenanta.example.com/`
  - `https://tenantb.example.com/`
- Log in using the provided credentials (e.g., `user1@tenanta.com` and `user1@tenantb.com`).

## Cleanup

To remove all deployed resources, run:

```bash
chmod +x cleanup.sh
./cleanup.sh
```

Also, delete the Cloud9 environment and the EC2 instance role (`Cloud9AdminRole`) if no longer needed.

## License

This project is licensed under the [MIT-0 License](LICENSE).

## Contributing

Contributions are welcome. Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## Support

For issues or questions, please open an issue in this repository.
