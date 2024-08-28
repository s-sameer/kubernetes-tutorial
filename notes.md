Recall containers:
- A container is a running instance of an image that provides an isolated execution environment for running an application. 
- It contains the application code and all the dependencies required to run the application, enabling it to run consistently across different computing environments.

What is Kubernetes?
- Kubernetes is an open-source platform designed to automate the deployment, scaling, and management of containerized applications.
- It is essentially a container orchestration platform.

What can you do with Kubernetes?
  - Deploy complex, multi-container applications
  - Manage application updates and rollbacks
  - Create several instances of your application (horizontal scaling)
  - Expose applications to the internet or to other services within the cluster
  - Distribute network traffic to maintain application availability
  - Restart containers that fail & replace failed containers
  - Deploy and update secrets and application configuration without rebuilding images
  - Roll out updates to a subset of users for testing
  - Define the desired state of your system, and Kubernetes works to maintain that state

What is a Kubernetes cluster?
- A Kubernetes cluster is a set of nodes (group of machines) that work together to run containerized applications managed by Kubernetes. 
- The cluster consists of at least one master (control plane) node and multiple worker nodes.

What is a Node?
- A node is a machine where Kubernetes is installed and is part of a kubernetes cluster.
- Two types of nodes: Master Node and Worker Node

Master node (Control Plane):
- The master node is responsible for managing the Kubernetes cluster. It controls the cluster, schedules tasks, and manages the overall state of the system.
- Components:
  - kube-apiserver: Acts as the frontend for kubernetes. Users, CLI tools, etc talk to the api server to interact with the kubernetes cluster.
  - etcd: A distributed key-value store that holds all the cluster data, including the configuration and state of the cluster.
  - controller-manager: Monitors the cluster and ensures the desired state of the system is maintained. Controllers are the brains behind orchestration.
  - kube-scheduler: Responsible for distributing pods/containers to worker nodes based on resource availability and other constraints.

Worker Node:
- Worker nodes are where the actual application workloads run. They host the pods, which are the smallest deployable units in Kubernetes.
- Components:
  - kubelet: An agent that runs on each worker node, responsible for maintaining the state of the pods on that node and communicating with the control plane
  - kube-proxy: Manages networking for the pods, handling communication both within the cluster and with the outside world.
  - container runtime: The software responsible for running the containers (e.g. docker, containerd)

kubectl:
- Is the command-line tool used to interact with a Kubernetes cluster
- It allows you to manage Kubernetes resources, deploy applications, view logs, etc
- Basic syntax:
  - kubectl [command] [TYPE] [NAME] [flags]
  - command: The action you want to perform (e.g., get, apply, delete).
  - TYPE: The type of Kubernetes resource (e.g., pod, service, deployment).
  - NAME: The name of the resource (optional, depending on the command).
  - flags: Optional parameters to modify the command's behavior.
- Common commands:
  - List all pods (in default namespace): kubectl get pods
  - Detailed info about a specific pod: kubectl describe pod [pod-name]
  - List all nodes in the cluster: kubectl get nodes
  - Create a resource from a YAML file: kubectl apply -f [file.yaml]
  - Create a deployment directly from a image: kubectl create deployment [deployment-name] --image=[image-name]
  - Scale a deployment: kubectl scale deployment [deployment-name] --replicas=[number]
  - Delete a specific pod: kubectl delete pod [pod-name]
  - View logs of a specific pod: kubectl logs [pod-name]
  - View all namespaces: kubectl get namespaces

What is a Pod?
- A pod is the smallest and most basic deployable unit.
- 
