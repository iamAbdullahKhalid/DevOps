Deploying a Containerized Application on Kubernetes: A Step-by-Step Guide
Introduction:

In this blog post, we will walk you through the process of deploying a containerized application on a Kubernetes cluster. We will cover the essential Kubernetes concepts and commands required to deploy and manage a containerized application on a Kubernetes cluster. We will also introduce you to basic components of Kubernetes.


What is Kubernetes???
Kubernetes is an open-source container orchestration platform for automating deployment, scaling, and management of containerized applications. It was initially developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF).

Kubernetes provides a container-centric infrastructure that abstracts away the underlying hardware and operating system, allowing developers to deploy and manage their applications in a consistent and scalable manner. It enables the deployment of containers in a distributed system, providing features such as service discovery, load balancing, and automatic failover.

Kubernetes is a powerful tool for managing containerized applications, and it has become the de facto standard for container orchestration in the cloud-native ecosystem. It is used by many organizations to run their applications in production, including large enterprises such as Airbnb, eBay, and Lyft.

Components of Kubernetes:

Kubernetes is an open-source container orchestration platform used to automate the deployment, scaling, and management of containerized applications. Kubernetes clusters consist of worker machines called nodes that run containerized applications. The control plane manages the worker nodes and the pods in the cluster. The components of a Kubernetes cluster include control plane components and node components.

Control plane components are responsible for making global decisions about the cluster, detecting and responding to cluster events, and scheduling. These components can be run on any machine in the cluster. The components of the control plane include:

kube-apiserver: The API server that exposes the Kubernetes API. It is the front end for the Kubernetes control plane.
etcd: A consistent and highly-available key-value store used as Kubernetes’ backing store for all cluster data.
kube-scheduler: A component that watches for newly created Pods with no assigned node and selects a node for them to run on.
kube-controller-manager: A component that runs controller processes responsible for noticing and responding when nodes go down, watches for Job objects that represent one-off tasks, populates EndpointSlice objects to provide a link between Services and Pods, and creates default ServiceAccounts for new namespaces.
cloud-controller-manager: A Kubernetes control plane component that embeds cloud-specific control logic.
Node components run on every node and maintain running pods, providing the Kubernetes runtime environment. These components include:

kubelet: An agent that runs on each node in the cluster and makes sure that containers are running in a Pod.
kube-proxy: A network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.
Container runtime: The software responsible for running containers.
Addons are Kubernetes resources that implement cluster features, and namespaced resources for addons belong within the kube-system namespace. Some important addons include:

DNS: A DNS server that serves DNS records for Kubernetes services.
Web UI (Dashboard): A web-based UI for Kubernetes clusters that allows users to manage and troubleshoot applications running in the cluster.
Container Resource Monitoring: Records generic time-series metrics about containers in a cluster.
Deploying a Containerized Application on Kubernetes:
Step 1: Setting up a Kubernetes Cluster
To deploy a containerized application on Kubernetes, you first need to set up a Kubernetes cluster. You can set up a Kubernetes cluster on your local machine or cloud environment. In this tutorial, we will set up a Kubernetes cluster on a local machine using Minikube.

Minikube is a tool that allows you to run a single-node Kubernetes cluster on your local machine. To install Minikube, follow these steps:

1.Install kubectl CLI by running the following command:

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
2. Make the kubectl binary executable by running the following command:

chmod +x kubectl
3.Move the kubectl binary to your PATH by running the following command:

sudo mv ./kubectl /usr/local/bin/kubectl
4.Install Minikube by running the following command:

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
5.Make the minikube binary executable by running the following command:

chmod +x minikube-linux-amd64
6.Move the minikube binary to your PATH by running the following command:

sudo mv minikube-linux-amd64 /usr/local/bin/minikube
7.Start the Minikube cluster by running the following command:

minikube start
Step 2: Creating a Docker Image
To deploy a containerized application on Kubernetes, you need to create a Docker image of your application. Docker allows you to package your application and its dependencies into a single image that can be easily deployed to any environment.

Assuming you have a Dockerfile for your application, follow these steps to create a Docker image:

Open a terminal window and navigate to your application’s directory.
Build the Docker image by running the following command:
docker build -t <image-name> 
Replace <image-name> with a name for your Docker image.

3. Verify that the Docker image has been created by running the following command:

docker images
This command should display a list of all Docker images on your local machine, including the one you just created.

Step 3: Deploying Your Application
Now that you have a Docker image of your application, you can deploy it to Kubernetes. To deploy a containerized application on Kubernetes, you need to create a Kubernetes deployment.

A Kubernetes deployment manages a set of replicas of your application and ensures that the desired number of replicas are running at all times. To create a Kubernetes deployment, follow these steps:

Create a deployment YAML file by running the following command:
kubectl create deployment <deployment-name> --image=<image-name> --dry-run=client -o yaml > deployment.yaml
Replace <deployment-name> with a name for your Kubernetes deployment.For example:

kubectl create deployment hello-world --image=<your-image-name>
Replace <your-image-name> with the name of the container image that you pushed to Docker Hub

This command creates a deployment named hello-world with a single replica. The --image flag specifies the Docker image to use for the deployment.

You can verify that the deployment was created by running:

kubectl get deployments
This should output a table with information about the hello-world deployment.

Next, we need to create a Kubernetes service to expose our deployment to the internet. A service is an abstraction that defines a logical set of pods and a policy by which to access them.

We will create a NodePort service, which exposes the deployment to the outside world by mapping a port on each node in the cluster to the hello-world deployment:

kubectl create service nodeport hello-world --tcp=80:80
This command creates a service named hello-world of type NodePort that maps port 80 on the node to port 80 in the hello-world deployment.

You can verify that the service was created by running:

minikube service hello-world --url
This should output a URL that you can copy and paste into a web browser to access the application.

Congratulations! You have successfully deployed a containerized application to a Kubernetes cluster!!!
Step 4: Cleaning Up
When you are finished experimenting with Kubernetes, you can clean up the resources that you created by running:

kubectl delete service hello-world
kubectl delete deployment hello-world
These commands delete the hello-world service and deployment.

You can also stop and delete the Kubernetes cluster by running:

minikube stop
minikube delete
Scaling Your Application:
One of the key benefits of Kubernetes is its ability to scale applications based on demand. In this section, we will demonstrate how to scale your application by increasing the number of replicas.

To check the current number of replicas for your deployment, use the following command:
kubectl get deployments
This will display the list of deployments, along with their current replicas and desired replicas.

2. To increase the number of replicas, use the following command:

kubectl scale deployment <deployment-name> --replicas=<number-of-replicas>
Replace <deployment-name> with the name of your deployment and <number-of-replicas> with the desired number of replicas.

For example, to increase the number of replicas for a deployment named myapp to 3, use the following command:

kubectl scale deployment myapp --replicas=3
Updating Your Application:
Another important feature of Kubernetes is the ability to update your application by rolling out a new version using the rolling update strategy. This strategy ensures that the update is rolled out gradually, minimizing downtime and ensuring that the application remains available throughout the update process.

To update your application, you first need to create a new version of your container image and push it to your container registry.
Once you have a new version of your container image, update the image tag in your deployment YAML file to point to the new version.
After updating the YAML file, use the following command to apply the changes to your deployment:
kubectl apply -f <deployment-file>.yaml
Replace <deployment-file> with the name of your deployment YAML file.

Kubernetes will automatically update the pods in your deployment one at a time, ensuring that the application remains available throughout the update process.

When you need to update your application, Kubernetes provides a rolling update strategy that allows you to deploy new versions of your application with zero downtime. The rolling update strategy will gradually replace old pods with new ones, ensuring that your application remains available during the deployment.

To update your deployment, you can also use the kubectl set image command:

kubectl set image deployment/my-app-deployment my-app-container=my-app:v2
This command will update the my-app-container container to version 2 of the my-app image. Kubernetes will automatically start rolling out the new version to your pods.

Monitoring and Troubleshooting
Kubernetes provides several tools to monitor and troubleshoot your cluster and applications. One of the most useful tools is kubectl, which allows you to interact with your cluster from the command line.

Here are some useful kubectl commands for monitoring and troubleshooting:

kubectl get pods - Lists all the pods in your cluster.
kubectl logs <pod-name> - Displays the logs for a specific pod.
kubectl describe <resource-name> - Displays detailed information about a specific resource, such as a deployment or a service.
kubectl exec <pod-name> <command> - Runs a command inside a specific pod.
kubectl top <resource-name> - Displays resource usage for a specific resource, such as a pod or a node.
You can also use Kubernetes dashboards, such as the Kubernetes Dashboard or Grafana, to monitor your cluster and applications visually.

Conclusion:
Kubernetes is a powerful tool for managing containerized applications. In this blog post, we covered the basics of Kubernetes, including creating and managing deployments, services, and pods. We also looked at scaling and updating your applications and monitoring and troubleshooting your cluster and applications.

With this knowledge, you should be able to get started with Kubernetes and take advantage of its features to manage your containerized applications effectively.
