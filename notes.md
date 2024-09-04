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
- A Kubernetes cluster is a set of nodes (group of machines) that work together to run containerized applications. 
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
  - kubelet: An agent that runs on each worker node, responsible for ensuring that the containers are running on the node as expected and communicate with the control plane.
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
  - Delete a replicaset: kubectl delete replicaset [replicaset-name]
  - View logs of a specific pod: kubectl logs [pod-name]
  - View all namespaces: kubectl get namespaces

What is a Pod?
- A pod is the smallest deployable unit that you can create and manage in kubernetes. You can think of them as a wrapper around containers
- A pod is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers
- A pod typically contains only one container (but can have more)
- Scaling an application is done by increasing/decreasing the number of pod replicas.
- To create a pod: 
  - Create a YAML file with the specifications of the pod and apply it
  - Use the command: kubectl run pod-name --image=image-name (not pref)
- Pod states:
  - Pending:  The pod has been accepted by the cluster but isn't running yet.
  - Running: At least one container is still running or in the process of starting/restarting.
  - Succeeded: All containers in the pod have terminated successfully and won't be restarted.
  - Failed: All containers in the Pod have terminated, and at least one container has terminated in a failure (exit code not equal to 0).
  - CrashLoopBackOff: The Pod is trying to start, but one or more containers are repeatedly failing.

YAML files and Kubernetes:
- Kubernetes uses YAML files as input for creating resources such as Pods, ReplicaSets, Deployments, Services, etc
- YAML files are structured as key-value pairs, with keys and values separated by a colon
- Indentation is used to denote hierarchy and relationships between elements
- A basic Kubernetes YAML file generally contains the following sections:
  - apiVersion: Specifies the API version used for the resource.
  - kind: Defines the type of Kubernetes resource you are creating
  - metadata: Provides metadata about the resource, such as its name, namespace, and labels.
    - name: The name of the resource
    - labels: Key-value pairs that can be used to categorize and organize the resource
  - spec: Describes the desired state of the resource. 

What is a ReplicaSet?
- A ReplicaSet is a fundamental Kubernetes resource which ensures that a specified number of pod replicas are running at any given time. 
- It maintains the desired state by automatically creating or deleting Pods to match the specified replica count, providing high availability, scalability, and fault tolerance for your applications.
- Components of a ReplicaSet:
  - apiVersion: Specifies the API version (e.g., apps/v1).
  - kind: Identifies the resource type (ReplicaSet).
  - metadata: Contains metadata such as name, labels, and annotations
  - spec: Defines the desired state, including:
    - replicas: Number of desired Pod replicas.
    - selector: Defines how the ReplicaSet identifies the Pods it manages using labels.
    - template: Specifies the Pod template used to create new Pods.
- To create a ReplicaSet: Create a YAML file with the specifications of the ReplicaSet and apply it
- While ReplicaSets are crucial for maintaining the number of Pod replicas, they are rarely used directly in modern Kubernetes workflows.
- Instead, Deployments are used to manage ReplicaSets, providing a higher level of abstraction and additional features like rolling updates and rollbacks.

What is a Deployment?
- A Deployment is a higher-level resource that manages ReplicaSets and provides declarative updates to applications.
- Key features include rolling updates, rollbacks, and scaling.
- Components of a Deployment:
  - apiVersion: Specifies the API version (e.g., apps/v1).
  - kind: Identifies the resource type (Deployment).
  - metadata: Contains metadata such as name, labels, and annotations
  - spec: Defines the desired state, including:
    - replicas: Number of desired Pod replicas.
    - selector: Defines how the Deployment identifies the Pods it manages using labels.
    - template: Specifies the Pod template used to create new Pods.
    - strategy: Defines the strategy for updating Pods (e.g., rolling update).
- To create a Deployment: Create a YAML file with the specifications of the Deployment and apply it
- Deployments strategy:
  - Rolling Update: Gradually replaces old Pods with new ones, ensuring zero downtime by incrementally updating Pods.
  - Recreate: Stops all existing Pods and starts new ones, causing a brief downtime.

What is a Service?
- A Service is a Kubernetes resource that allows you to expose your Pods either internally within the cluster or externally to the internet.
- Reasons for using a Service:
  - Provides a stable network endpoint for your Pods, allowing communication between Pods within the cluster.
  - Allows load balancing across multiple Pods, distributing network traffic evenly.
  - Enables automatic service discovery and DNS resolution for Pods.
- Types of Services:
  - ClusterIP: Exposes the Service on a cluster-internal IP, allowing communication within the cluster only.
  - NodePort: Exposes the Service on each node's IP at a static port, allowing external users to access the Service.
  - LoadBalancer: Integrates with cloud providers' load balancers to provide external access to the Service.
- Components of a Service:
  - apiVersion: Specifies the API version (e.g., v1).
  - kind: Identifies the resource type (Service).
  - metadata: Contains metadata such as name, labels, and annotations
  - spec: Defines the desired state, including:
    - selector:  Identifies the Pods the Service routes to, using labels.
    - ports: Defines the ports on which the Service is exposed.
    - type: Specifies the type of Service (ClusterIP, NodePort, LoadBalancer).
- To create a Service: Create a YAML file with the specifications of the Service and apply it

