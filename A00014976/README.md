## Proyecto Sistemas Distribuidos

**Universidad ICESI**  
**Curso:** Sistemas Distribuidos  
**Docente:** James Steven Montealegre Gutiérrez  
**Tema:**  Orquestación de Contenedores Docker Swarm & Kubernetes   
**Correo:** james.montealegre@correo.icesi.edu.co

### Actividades
##### 1). Realice un gráfico donde muestre la arquitectura de docker swarm y kubernetes. Incluya aspectos como las tecnologías empleadas para la comunicación de sus elementos, descubrimiento de servicio, entre otros.



**Kubernetes:**  

*Master components:*  

* **Cloud-controller-manager:** cloud-controller-manager runs controllers that interact with the underlying cloud providers. The cloud-controller-manager binary is an alpha feature introduced in Kubernetes release 1.6.
cloud-controller-manager runs cloud-provider-specific controller loops only. You must disable these controller loops in the kube-controller-manager. You can disable the controller loops by setting the --cloud-provider flag to external when starting the kube-controller-manager.

* **kube-controller-manager:** Component on the master that runs controllers.   
Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.  
These controllers include:
   - Node Controller: Responsible for noticing and responding when nodes go down.
   - Replication Controller: Responsible for maintaining the correct number of pods for every replication controller object in the system.
   - Endpoints Controller: Populates the Endpoints object (that is, joins Services & Pods).
   - Service Account & Token Controllers: Create default accounts and API access tokens for new namespaces.  

* **Kube Scheduler:** Component on the master that watches newly created pods that have no node assigned, and selects a node for them to run on. Factors taken into account for scheduling decisions include individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference and deadlines.

* **etcd:** Consistent and highly-available key value store used as Kubernetes’ backing store for all cluster data. Always have a backup plan for etcd’s data for your Kubernetes cluster.  

* **Kube-apiserver:** Component on the master that exposes the Kubernetes API. It is the front-end for the Kubernetes control plane. It is designed to scale horizontally – that is, it scales by deploying more instances.

*Node components:*

* **Kubelet:** An agent that runs on each node in the cluster. It makes sure that containers are running in a pod. The kubelet takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn’t manage containers which were not created by Kubernetes.  

* **Kube-proxy:** kube-proxy enables the Kubernetes service abstraction by maintaining network rules on the host and performing connection forwarding.  

* **Container runtime:** The container runtime is the software that is responsible for running containers. Kubernetes supports several runtimes: Docker, containerd, cri-o, rktlet and any implementation of the Kubernetes CRI (Container Runtime Interface)





![kubernetes](https://user-images.githubusercontent.com/35767872/58590732-5c487300-822a-11e9-9667-c6949c98c34c.png)  


**Docker Swarm:**   

![swarm](https://user-images.githubusercontent.com/35767872/58591856-d8dc5100-822c-11e9-8416-b24a7780128e.png)  

- [x] Imagen tomada de https://image.slidesharecdn.com/awsmeetup16-160408140028/95/building-a-globalscale-multitenant-cloud-platform-on-aws-and-docker-lessons-learned-59-638.jpg?cb=1460124146  


*Components description:*  

* **Docker Compose (compose):** This is a tool for defining and running multi-container Docker applications. This tool takes a simple configuration yml file and deploys the containers like a manifest. As a sample, here’s one I released for deploying the HA RabbitMQ cluster as a Docker Compose configuration yml file. It is the deployment tool for utilizing the new multi-host networking features that allow the Swarm Nodes to have applications running across distributed hosts. Compose also handles placement strategies for ensuring your containers are distributed evenly (or not) across the Swarm Nodes. This allows for container redundancy at the host level which is good for production resiliency.  

* **Overlay Network:** This is the new native Docker networking type for deploying containers that are linked only with the other containers on the network. This is utilizing VXLAN technology and is really interesting for its ability to separate containers (even on the same node) as well linking containers across multiple Swarm Nodes. The containers deployed with an overlay network can see the other linked containers on the network by using the entries defined in the /etc/hosts file in the container. I highly recommend.  

* **Swarm Manager (manager):** The service a user uses for managing containers across the registered Swarm Node(s). This is the endpoint for interfacing with a Swarm environment.  

* **Swarm Node (node):** This is not an official Docker term but a logical association for a host machine in the Swarm that is only responsible for running containers (I think of these as similar to an OpenShift Node for hosting applications). At a minimum, a Swarm Node needs to have the Docker Daemon and the Swarm Join running on it.  

* **Swarm Join (join):** A process that handles registering a single host with a Service Discovery Manager and exposing the host’s Docker Daemon as an available service.  

* **Docker Daemon (daemon):** A process that handles container management on a single host or vm.  



##### 2). Crear una tabla comparativa con al menos 5 diferencias entre Docker Swarm y Kubernetes.   

**Comparison** | Kubernetes | Docker Swarm
--------------- | ---------- | ------------
**Developed by** | Google | Docker Inc 
**Year Released** | 2014 | 2013  
**Controller** | Master | Manager    
**Storage** | Persistent and Ephermal | Volumes  
**Deployment unit** | Pod | Task  
**Network** | Flat Networking space | Overlay  
**Public Cloud Service Provider** | Azure | Google, Azure, AWS, OTC
**Port** | Endpoint | Published Port  
**Load Balancing** | Provides automated internal load balancing throught any node in the cluster | Provides load balancing when pods in the container are defined as service  
**Container set up** | Client API and YAML are unique un Kubernetes | Functionality is provided and limited by Docker API
**Scalability** | Provides strong guarantees to the cluster states at expense of speed | Quick container deployment and scaling even in large containers  


##### 3). Mencione las tecnologías de orquestación de contenedores nativas con kubernetes para al menos tres proveedores de servicios en la nube. Describa sus principales características y limitaciones.  

* **AZURE (AKS)**  
The fully managed Azure Kubernetes Service (AKS) facilitates the implementation and administration of applications in containers. It offers Kubernetes without a server, an integrated integration and delivery (CI / CD) experience and enterprise-level security and governance. Join your development and operations teams on a single platform to create, deliver and scale applications with confidence.

*Characteristics:*  
* **Create microservice applications:** Easily define and compile microservices orchestrated with Kubernetes in Visual Studio Code (VS Code).  
* **Implement a Kubernetes cluster:** Deploy a multi-node cluster of Azure Kubernetes Service (AKS) without having to worry about reliability, availability or updates.  
* **Monitor and manage Kubernetes easily:** Get full visibility of your managed Kubernetes environment with control plane telemetry, record aggregation and container maintenance as part of Azure Portal.  

*Limits:*  
* AKS is not a fully managed cluster solution. Some components, such as work nodes, have shared responsibility , where users must help maintain the AKS cluster. It is required provided by the user, for example, to apply an operating system (OS) security review of work node.
Services are managed in the sense that Microsoft and the AKS team is implemented, operates and is responsible for the availability of the service and functionality. Customers can not modify these managed components. Microsoft limits customization and does not offer a personalized service  

* **GOOGLE CLOUD KUBERNETES ENGINE**  
Kubernetes Engine is a managed production environment that allows applications to be implemented in containers. It offers our latest innovations in developer productivity, resource efficiency, operations automation and open source flexibility to accelerate time to market.  
Kubernetes Engine allows the development and iteration of applications more quickly, since it facilitates implementing, updating and managing applications and services. This environment does not only work with stateless applications; You can also add continuous storage or even run a database in your cluster. It simply describes the processing, memory and storage resources required by the containers in your application and Kubernetes Engine will automatically provision and manage the underlying cloud resources. Support for hardware accelerators facilitates the execution of machine learning, general-purpose GPUs, high-performance processing, and other workloads that benefit from some specialized accelerators  

*Characteristics:*  
* **Create microservice applications:** Easily define and compile microservices orchestrated with Kubernetes in Visual Studio Code (VS Code).   
* **Identity and access management:** Control access to the cluster with your Google accounts and feature permissions.  
* **Automatic scaling:** Increase and decrease the scale of implementation of your applications automatically according to the use of resources (CPU, memory).  
* **Private Container Registry** The integration with Google Container Registry facilitates the storage of your private Docker images and access to them.  
* **Compatibility with Docker images:** Kubernetes Engine is compatible with the common Docker container format.  
* **Uniform and fast compilations:** Use Google Cloud Build to reliably implement your containers in Kubernetes Engine, without having to configure authentication.  
* **Resource limits:** Kubernetes allows you to specify the amount of memory (RAM) and CPU each container needs, which is used to better organize the workloads within your cluster.  

*Limits:*  
* If you choose a minimum CPU platform, Google Kubernetes Engine tries to create the cluster or group of nodes with the minimum CPU platform whenever possible. In some cases, this is not possible. For example: 
If the minimum CPU platform is older than the default platform in the zone or if it is no longer available and there is a new one at the same cost, GKE creates the cluster or group of nodes with the newest platform.
If you specify a platform that is not available and there is no equivalent that is newer or has the same price, the creation of clusters or nodes fails. Nodes never use a platform that is older than the specified minimum CPU platform and the cost of the nodes does not change if GKE chooses a newer platform. The nodes retain the same platform throughout their life cycle, unless the specified CPU platform is removed, in which case the nodes are executed on a newer platform  

* **OVH**   
One of the main claims of Kubernetes is that it offers a standard for the different cloud providers, necessary for multicloud and hybrid cloud scenarios . Thus, Managed Kubernetes of OVH remains true to open source standards and, at the same time, takes advantage of the benefits of OVH's public cloud. Among other things, the managed Kubernetes service includes the following components:  
  - A load balancer integrated in Kubernetes natively
  - Possibility to configure security update policy.
  - Possibility of choosing between versions 1.11 and 1.12 of Kubernetes (the most recent versions offered by other managed solution providers).

*Characteristics:*   
* **Scalability and high availability:** It is very easy to expose a service in multiple work nodes with just a few lines of commands. Kubernetes® starts the containers and configures the load balancer for you. If you wish, you can establish health conditions for each service, and Kubernetes® will relaunch the pods and containers that do not meet these conditions. Likewise, the nodes enjoy our monitoring service . In just a moment you can add new calculation nodes and have all the advantages of the high availability of infrastructures as a service (IaaS) of OVH.  
* **Easy deployment from development to production:** Today, most applications, both open source and commercial, are distributed in Docker container or Helm chart format and can be deployed in a Kubernetes® service with a single command line. Kubernetes® intelligently distributes containers and services between the different nodes. If you want to isolate development, preproduction and production, simply move the configuration file from one cluster to another. Thanks to the declarative syntax, you will only have to describe the expected state.  
* **Persistent volumes:** You can add persistent volumes to your work nodes. These are based on additional drives of the sizes and classes of your choice (standard or high performance, billed to the nearest gigabyte). This ensures the durability to the data of your stateful applications.  

##### 4). Describa las implicaciones a nivel de operaciones de emplear una solución de kubernetes nativa con respecto a desplegar kubernetes manualmente a través de scripts, por ejemplo: KOPS. Tenga en cuenta aspectos de costo y tolerancia a fallas.  













