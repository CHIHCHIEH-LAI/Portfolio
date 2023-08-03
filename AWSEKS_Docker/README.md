# Virtualization for Image Classification Service

## Overview

Welcome to the Virtualization for Image Classification Service project! Having developed one of the best image classification neural networks, the next step is to provide it as an online service and capture some market share. In this project, we will set up and manage the infrastructure using Kubernetes and Docker containers over a cluster of nodes.

The main goal is to start a web server interface for end-users to access machine learning services. When users send HTTP requests, the server will be responsible for launching the corresponding machine learning jobs via Kubernetes. Additionally, an autograder will act as an end-user, sending HTTP requests to the exposed server to grade the submissions.

## Requirements

To successfully complete this project, you will need the following:

1. An AWS account with available free credits to work on AWS Elastic Kubernetes Service (EKS), EC2, Kubernetes, and Docker. Please note that AWS Educate accounts will not work as they cannot create the necessary network resources for this assignment.

2. Familiarity with one of the following programming languages for implementing the server and interacting with Kubernetes: Python 3, JavaScript, Java, or Go. While assistance is available regardless of the chosen language, Python is recommended for optimal support.

3. The provided [GitHub repository](https://github.com/UIUC-CS498-Cloud/MP12_PublicFiles) contains all the necessary files for this project, including a helpful FAQ.

4. We highly recommend performing the entire project on an EC2 instance to simplify setting up permissions, as both the EC2 instance and EKS will be in the same AWS account.

## Procedure

### AWS EKS / Kubernetes Setup

Prepare a host machine, preferably an EC2 instance, for creating and managing your EKS cluster. You need to install `aws cli`, `kubectl`, and `eksctl` to work with EKS.

The following are some helpful resources to get started with EKS and Kubernetes setup:
- [EKS Introduction](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
- [EKS Setup with eksctl](https://eksctl.io/)
- [EKS YouTube Tutorial](https://www.youtube.com/watch?v=p6xDCz00TxU)
- [kubectl Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

We recommend using a YAML file of type "ClusterConfig" to create the cluster using `eksctl`, as shown in the provided YAML example. This cluster will have two nodes of type `t2.medium`, each with 2 CPUs, which is essential for this project.

### Containerization

Two versions of the image classification neural network are provided: a simple feed-forward neural network (free service) and a convolutional neural network (premium service). The classification script `classify.py` retrieves the dataset from the internet, trains the model, and performs testing/classification. Your task is to dockerize the classification script and create a Docker image for the classifier. You also need to test that the image works correctly when run in a Docker container.

### Deploying to Cluster

After creating the Docker image, you need to push it to a repository/registry, such as AWS ECR or Dockerhub. Then, you can deploy the image to your Kubernetes server using the Kubernetes API. The provided [template code](https://github.com/UIUC-CS498-Cloud/MP12_PublicFiles/blob/main/server.py) will be used to implement the server on the machine used to set up EKS. You will interact programmatically with Kubernetes using the Kubernetes client libraries.

### Resource Provisioning

To host both free and premium services, you must provision your cluster appropriately to run the jobs. For the free service, at most two CPUs out of the total four should be provisioned. You can use a Resource Quota YAML file for the "free-service" namespace. Each job should request and have a limit of 0.9 CPU. Premium service pods should execute under the "default" namespace.

### Exposing Image Classification

The server will accept image classification requests and configurations from the autograder to verify your implementation. You will implement APIs for creating containers (jobs) with the neural networks within the appropriate namespaces. The server will also provide a configuration API to send information about all pods across all namespaces.

## Final Result

The virtualization of the image classification service allows for efficient resource provisioning and isolation between different executions. The server can easily deploy the neural network on any of the nodes without worrying about the environment and dependencies, providing a robust and scalable solution.

## Resource Cleanup

After successfully submitting the project, make sure to delete all resources, including EC2 instances, EKS clusters, CloudFormation stacks, and VPCs. If you encounter any errors while deleting CloudFormation stacks, try deleting the VPC first and then retry deleting the stack.

## Tips

A common mistake is to use a constant job name in the YAML file, resulting in only one pod being created with multiple POST requests. Avoid this issue by using dynamic job names.

## My Overall Workflow (Student's Note)

As a student working on this project, here's an overall workflow that can be followed to successfully complete the project:

1. Launch and SSH into an EC2 instance.
2. Install Docker on the EC2 instance.
3. Prepare the Dockerfile and build an image.
4. Push the Docker image to Dockerhub or AWS ECR.
5. Prepare the YAML files for the EKS cluster setup, job deployment, and resource quota.
6. Install AWS CLI, kubectl, and eksctl on the EC2 instance.
7. Modify IAM role for the EC2 instance to have the required permissions for EKS.
8. Create and configure the EKS cluster using eksctl.
9. Implement the server.py file on the EC2 instance.
10. Run the server.py on the EC2 instance.
11. Test the functionality by running test.py or using `curl` to send HTTP requests to the application URL.
12. Delete existing pods before testing and use `kubectl get pods -A` to check the pods' status.

This project offers an excellent opportunity to gain hands-on experience with Kubernetes, Docker, and AWS EKS, and to demonstrate your ability to deploy and manage an image classification service efficiently. Have fun working on the project, and good luck!
