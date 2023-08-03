# MP12 - Virtualization

## Overview
Having developed one of the best image classification neural networks out there, it is now time to provide it as an online service and capture some market share. To do so, we set up and manage the infrastructure using Kubernetes and Docker containers over a cluster of nodes in this MP.

Essentially, you will start a web server interface for end-users to access machine learning services. When users send HTTP requests, your server is responsible for launching the corresponding ML jobs via Kubernetes. The autograder will act as an end-user, sending HTTP requests to your exposed server to grade the submissions.

## Requirements
Note: Please use region -> us-east-1 for your deployment.

You need an AWS account with some free credit available and will work on EKS, EC2, Kubernetes, and Docker. AWS Educate accounts will not work as these accounts cannot create the network resources (EKS) needed for this assignment. Also, you need to be familiar with one of the following programming languages for implementing your server and interacting with Kubernetes: Python 3 / Javascript / Java / Go. While we will attempt to help out irrespective of your chosen language, we can best assist with Python. 

This GitHub repository contains all the necessary files for this MP, including an FAQ: https://github.com/UIUC-CS498-Cloud/MP12_PublicFiles \
WE RECOMMEND DOING THIS ENTIRE MP ON AN EC2 INSTANCE. YOU CAN CREATE AN EC2 INSTANCE, CREATE CLUSTER FROM THAT INSTANCE, CREATE DOCKER IMAGE IN THE SAME INSTANCE AND DO ALL CLUSTER OPERATIONS FROM THAT INSTANCE. YOU NEED NOT USE YOUR PERSONAL COMPUTER. THIS WILL MAKE SETTING UP PERMISSIONS VERY EASY SINCE BOTH THE EC2 INSTANCE AND THE EKS WILL BE IN THE SAME AWS ACCOUNT. YOU CAN ALSO RUN THE WEB APPLICATION'S SERVER IN THE SAME INSTANCE AS THE INSTANCE WILL HAVE CLUSTER ACCESS.

## Procedure
### AWS EKS / Kubernetes Setup
Prepare a host machine through which you will create and manage your EKS cluster. Technically, you can use your personal computer or machine to create and manage EKS cluster. But, we highly recommend you to use an EC2 instance instead of your own machine. You can read the note in the above section.

You need to install aws cli, kubectl and eksctl in order to create and manage the EKS cluster.

The following is an introduction to EKS, Amazon's Elastic Kubernetes Service.
1. https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html 
2. https://eksctl.io/ 

Some other helpful resources:
1. https://www.youtube.com/watch?v=p6xDCz00TxU 
2. https://kubernetes.io/docs/reference/kubectl/cheatsheet/ 

We highly recommend to use a "ClusterConfig" type YAML file to create the cluster using eksctl instead of configuring the cluster yourself. Below is the yaml file you can use to create the cluster. The below file contains the configuration for the cluster which will have 2 nodes of type (t2.medium). We recommend you to use t2.medium instances which will have 4 CPUs (2 CPUs in each node) which is required for this MP.
```
apiVersion: eksctl.io/v1alpha5 
kind: ClusterConfig 

metadata: 
    name: mp12-cluster 
    region: us-east-1 
    
availabilityZones:
   - us-east-1a
   - us-east-1b

nodeGroups: 
  - name: ng-1 
    instanceType: t2.medium 
    desiredCapacity: 2
    privateNetworking: true
```

### Containerization
We have provided the image classification neural network implementation with two versions - [classify.py](https://github.com/UIUC-CS498-Cloud/MP12_PublicFiles/blob/main/classify.py). This file contains both the versions. The first is a simple feed-forward neural network that you will use to market your product, providing it as a free service. The second is your premium service - a convolutional neural network. The provided implementation retrieves the dataset from the internet, trains the model, and then performs testing/classification on it. The 'DATASET' can be mnist or kmnist, and the 'TYPE' can be ff or cnn, both passed in using environment variables. Here, you only need to know how to run classify.py; not worry about its implementation. 

First, you have to dockerize the classification script to run it on your cluster.  You need to create a docker image for the classifier. The environment variables can either be passed through the Dockerfile or later via Kubernetes. We have also provided requirements.txt, which specifies the python packages that your docker image needs. Once you have created the image, you should test that it works when you run it in a docker container. The container should take approximately 50 seconds to run.

We provided a [template](https://github.com/UIUC-CS498-Cloud/MP12_PublicFiles/blob/main/Dockerfile) for the Dockerfile which you need to complete. You can refer the template and create the Dockerfile. The template is only for reference, you can create the Dockerfile in any other way as you wish as long as it works.

Docker resource: https://docs.docker.com/get-started/02_our_app/

The expected output of successfully running a docker container is similar to the following:
```
dataset: mnist
Downloading http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz to ./data/MNIST/raw/train-images-idx3-ubyte.gz
100.1%Extracting ./data/MNIST/raw/train-images-idx3-ubyte.gz to ./data/MNIST/raw
...
Processing...
Done!
Epoch [1/5], Step [100/600], Loss: 0.2846
Epoch [1/5], Step [200/600], Loss: 0.3859
Epoch [1/5], Step [300/600], Loss: 0.1448
...
Accuracy of the network on the 10K test images: 98 %
```

### Deploying to cluster
Now that the container image is ready, it is time to deploy it to your Kubernetes server. First, you need to push the image to a repository/registry after creating the image. You can either use AWS ECR or Dockerhub for this purpose. The worker nodes in the kubernetes cluster can then pull the image from the registry and use them for the job. 

Dockerhub resources:

https://docs.docker.com/docker-hub/repos/

https://docs.docker.com/engine/reference/commandline/push/


AWS ECR documentation: https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html

Note: if the EKS host is the same one you used to create the container image, you do not need to push it to the Docker hub. But we do not recommend to take this approach as it is not advisable to change the docker permissions on the machine. We highly recommend to either use AWS ECR or Dockerhub which is a good practice.

Then you need to create a configuration file and use the Kubernetes API to deploy it to the cluster nodes. You will do this part (using API to deploy job) in the server code. You will see the instructions for the server code in the later part of this page. Kubernetes will execute each job in its own pod. Since your python code runs to completion (and does not stay alive as a service), you will be using a “job” as the Kubernetes resource type. 

You can follow the official document of Kubernetes to learn how to deploy your job:

https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/  

Here is an example of how to set environment variables in a Kubernetes YAML file (note that you will not be using “kind: Pod” as described in the following guide. This is just an example of environment variables usage in YAML.)

https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/

### Resource Provisioning
As you will be hosting both free and premium services, you must provision your cluster appropriately to run the jobs. You do not want the free service to take up all the available hardware resources. To do so, you will provision at most two CPUs out of the total four (t2.medium has two cores) for your free service. We do this by using the Kubernetes namespace to create a virtual cluster. You can use a Resource Quota YAML file for the "free-service" namespace. 

https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/quota-memory-cpu-namespace/  

Each of the jobs should request and have a limit of 0.9 CPU. The same requirement applies to premium service as well. We want to ensure that at any time, only two free service pods are running (the other free service pods should be queued up). However, there can be any number of premium pods running. You must use a Job YAML to specify this resource limit. 

https://kubernetes.io/docs/concepts/workloads/controllers/job/

Premium service pods should execute under the "default" namespace and the free ones under the "free-service".

###  Exposing Image Classification
Now that the cluster is ready to use, you will build a server that accepts image classification requests and the necessary configurations from the autograder to verify your implementation. You will implement the server on the machine you used to set up the EKS, which has the Kubernetes configuration. We recommend running the server on an EC2 instance, not exposing your local network to the autograder. The main part is programmatically interacting with Kubernetes, which you can achieve by using one of the following client libraries:

https://kubernetes.io/docs/reference/using-api/client-libraries/
We provide you with the [template code](https://github.com/UIUC-CS498-Cloud/MP12_PublicFiles/blob/main/server.py) which you can use.

The two APIs you will need are list_pod_for_all_namespaces() and create_namespaced_job().

You will be implementing the following API's:

1. Free Service - Create a container (job) with the feed-forward neural network within the free service namespace
  * HTTP POST: /img-classification/free 
  * BODY: {"dataset":"mnist"} (even though this looks like JSON string, the content type might not be application/json, so parse the request body accordingly)
  * The response status code should be 200 if the job has been successfully created. Based on your namespace configuration, only two free service jobs should be running at a time.
  
2. Premium Service - Create a container (job) with the convolutional neural network within the default namespace
  * HTTP POST: /img-classification/premium
  * BODY: {"dataset":"kmnist"} (even though this looks like JSON string, the content type might not be application/json, so parse the request body accordingly)
  * The response status code should be 200. You are hypothetically getting paid for this service. 

3. Configuration - Sends a snapshot of the current outlook of your Kubernetes cluster by returning information about all pods across all namespaces.
  * HTTP GET: /config
  * Response status code should be 200, and the body should be valid JSON. The body contains all the pods, including those created by  Kubernetes that are currently executing with some specifications. The following are the specifications:
  * BODY: { "pods": [pod1, pod2 ...]}
    * pods : a list of pods
    * a pod:  {"node" : "node on which the pod is executing", "ip" : "ip address of the pod", "namespace" : "namespace of the node", "name" : "name of the pod", "status":"status of the pod"}

For example:
```
{
   "pods": [
      {
         "node": "ip-192-168-123-7.ec2.internal", 
         "ip": "192.168.125.2", 
         "namespace": "default", 
         "name": "mnist-deployment-6dc644dd6d-grpfd", 
         "status": "Succeeded"
      },
      {
         "node": "ip-192-168-123-7.ec2.internal", 
         "ip": "192.168.125.2", 
         "namespace": "default", 
         "name": "mnist-deployment-6dc644dd6d-grpfd", 
         "status": "Succeeded"
      } 
  ] 
}
```

The following are the object mappings for the python code (https://github.com/kubernetes-client/python)
```
{ 
"name": pod.metadata.name,
"ip": pod.status.pod_ip,
"namespace":pod.metadata.namespace,
"node":pod.spec.node_name, 
"status": pod.status.phase 
}
```

If your server is running correctly, you should be seeing three default pods and two free-service pods running. 
![image](https://github.com/CHIHCHIEH-LAI/Project-Portfolio-Collection/blob/main/AWSEKS_Docker/imgs/pods.png)

## Final Result
Virtualization helped us effectively provision our hardware resources and achieve isolation between different executions. Moreover, we could easily deploy our neural network in any of our nodes without worrying about the environment, dependencies, etc.

## Resource clean up
After submitting the MP successfully, make sure you delete all the resources including EC2 instance, EKS cluster, CloudFormation stacks, and VPC.

If you receive an error deleting one of the CloudFormation stack, then you need to delete the VPC and then comeback and delete the stack again.

## Tips
Common mistake: do not use a constant job name in the YAML file. This will result in  only one pod being created with multiple POST requests. 

## My Overall Workflow
1. launch and ssh an EC2 instance
2. install docker https://www.cyberciti.biz/faq/how-to-install-docker-on-amazon-linux-2/
3. prepare Dockerfile and build an image https://docs.docker.com/get-started/02_our_app/
    ```
    # build the Docker image
    docker build -t classifier-image .
    # run the image in a Docker container
    docker run -p 5035:5035 classifier-image
    ```
4. push the image to dockerhub https://docs.docker.com/docker-hub/repos/
    ```
    # Log in to Docker Hub
    docker login --username=your-dockerhub-username
    # Tag your Docker image using your Docker Hub username and the name of your repository
    docker tag classifier-image myusername/classifier-image
    # Push the tagged image to Docker Hub
    docker push myusername/classifier-image
    ```
5. prepare cluster, job and resource quota yaml files
    * cluster yaml: https://eksctl.io/
    * job yaml: https://kubernetes.io/docs/concepts/workloads/controllers/job/
    * resource quota: https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/quota-memory-cpu-namespace/
7. (Optional) test yaml file and docker image on minikube https://minikube.sigs.k8s.io/docs/start/
8. install aws cli, kubectl and eksctl in order to create and manage the EKS cluster on EC2 instance
    * install kubectl https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
    * install eksctl https://blog.knoldus.com/how-to-install-eksctl-the-official-cli-for-amazon-eks/
9. install python and libraries
10. modify IAM role for ec2 instance https://z2jh.jupyter.org/en/stable/kubernetes/amazon/step-zero-aws.html#the-procedure
11. create access key from aws account security credentials https://www.msp360.com/resources/blog/how-to-find-your-aws-access-key-id-and-secret-access-key/
12. aws configure
13. (Optional) docker pull from dockerhub
14. create the cluster using eksct https://eksctl.io/
15. create the free-service namespace https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/quota-memory-cpu-namespace/
    ```
    kubectl create namespace free-service
    ```
15. apply yaml files to the cluster https://kubernetes.io/docs/concepts/workloads/controllers/job/
    ```
    kubectl delete -f <yaml> -n <namespace>
    kubectl apply -f <yaml> -n <namespace>
    ```
16. implement server.py file
17. run server.py
18. run test.py or ```curl -X GET http://<application-url>/get-config to test the functionality```
    * delete existing pods before testing
    ```
    kubectl delete pods --all -n free-service
    kubectl delete pods --all -n default
    ```
    * get pods information
    ```
    kubectl get pods -A
    ```
 
