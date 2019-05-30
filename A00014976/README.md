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

- [x] Image from https://image.slidesharecdn.com/awsmeetup16-160408140028/95/building-a-globalscale-multitenant-cloud-platform-on-aws-and-docker-lessons-learned-59-638.jpg?cb=1460124146  


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

 Kops | Amazon EKS
--------------- | ---------- |
Selection of Kubernetes cluster version e.g. it support various versions | Amazon Controlled, and the recent deployments are slow than the mainline.  
Initial instrumentation as by default, it will not add any RBAC or User management | Tightly coupled with Amazon IAM, and leverage the open source AWS authenticator for Access control   
Fully managed by yourself It can be an advantage or disadvantage for the future use | Managed by AWS   
Ensuring etcd cluster is up and running | Managed by AWS  
Ensure Kube master node services are up and running. Kube api server, kube DNS, kube-scheduler etc. | Managed by AWS  
CNI integration. Various options: Flannel Amazon-vpc-routed-eni Kubenet (default) | VPC level networking is already implemented at the network level, it means, the container routes will be published by default through Amazon using amazon-vpc-cni-k8s  
KOPS maintains its state file in a S3 bucket | Maintained by AWS  
nstance groups with tainting/labeling. | EKS doesn’t support and not even supported by EKSCTL.  
Rotating the cluster | Manually, and one by one instance  

When you choose a tool for any solution, you get the most close alternative according to your needs. EKS by AWS services offer so many configurations for deploy and management that can make your job easier.   

##### 5). Crear una tabla comparativa con al menos 5 diferencias entre el monitoreo con Datadog y el monitoreo con Prometheus. Incluya aspectos como costo, obtención de métricas, alertas, entre otros   

**Comparison** | Prometheus | Datadog
--------------- | ---------- | ------------
**Installation Process** |Prometheus is one of the monitoring solutions that the Kubernetes documentation specifically recommends, it’s unsurprising that the installation process is pretty easy. All you need to do it create a cluster role, a config map, and a deployment for Prometheus, most of which can be copy pasted from any number of tutorials online (here’s one to get you started). It takes only a couple minutes to get set up. However, Prometheus comes with a whole ecosystem of tools that support it. If you want more sophisticated dashboards and graphs, you’ll have to spin up Grafana and integrate with it. If you want a decent alerting system, you’ll need to install AlertManager. As you’ll see, this ends up making Prometheus a little more complicated to set up than Datadog. It’s still not difficult, but it’s not trivial either. | If you already have a Datadog account, getting metrics into Datadog is extremely simple. You’ll just configure RBAC permissions and install the Datadog agent as a DaemonSet. If you want to send custom metrics to Datadog, you’ll need to do a little more configuration. Datadog ships with visualizations and an alerting layer, so there’s no need to do any additional setup.    
**Visualizations** | While Prometheus itself ships with a graphing tool, its functionality is fairly rudimentary. It’s more likely the case that any Prometheus installation actually relies on Grafana for visualization, and for the purpose of this comparison, we’ll consider Grafana’s visualization capabilities. Grafana makes it easy to create dashboards and graphs of your metrics. There are loads of different widgets and options that allow you to tailor your visualization to your needs. And it comes with an easy-on-the-eyes dark theme, if that’s a consideration for you. | Datadog provides very full-featured graphs and dashboards with great performance. Like Grafana, there are many widgets to create any dashboard you could think of. The tagging system is great and the query language is very robust. Datadog also comes with preset Kubernetes dashboards. Grafana and Datadog are very similar in their visualization capabilities. Both have every feature you need to get started and allow you to put together dashboards that will help you monitor your Kubernetes cluster. This one’s a tie. 
**Kubernetes Events** | Prometheus doesn’t automatically expose Kubernetes events, so you’ll need a different solution to help you with this problem. There is a project on Github that will export Kubernetes events as Prometheus metrics. You can then import these event metrics into Grafana. | Datadog’s agent will gather events from your Kubernetes cluster and then dump them into the “Events” section of the app. It’s not entirely obvious, but you will have to enable it, and you will also have to understand and set up alerts for events to get value from them. As far as visualizing Kubernetes events, each event typically only lists the “Reason” part of an event, so you end up with an event containing a ReplicaSet and a string like “OOM” or “KILL.” The UI that shows pod labels is also pretty cramped and hard to read.  
**Alerting and Notifications** | Just like Grafana is required to do any meaningful visualization, you’ll need to install AlertManager to get useful notifications from Prometheus. That said, once you’ve installed AlertManager, it’s got some pretty cool capabilities. You can group alerts together into one notification, or even silence specific alerts for a period of time. There’s also a neat feature called inhibition that allows you to silence certain classes of alerts when another type of event is active (consider the case that if CPU is high on a container, you might not care to know that the load is also high). | While Datadog lacks some of AlertManager’s cool notification rules like inhibition or granular silencing, it has an incredibly robust system of detecting error states. Datadog calls these alerts Monitors, and they can do anything from threshold and anomaly detection on a metric to checking an event for a specific string. By default, Datadog will notify your team through email, but many services have Datadog integrations that let you get notifications through your preferred incident management system.   


Both Prometheus and Datadog are fully featured, robust monitoring solutions. However, they require extensive configuration to get maximum benefit from monitoring, and they assume that as the DevOps professional, you will know all the things that could possibly go wrong and how to check for them. However, Kubernetes is a relatively new tool, and even with experience, there is always more to learn. 

Datadog is the leading service for cloud-scale monitoring. It is used by IT, operations, and development teams who build and operate applications that run on dynamic or hybrid cloud infrastructure. Start monitoring in minutes with Datadog. 
In another hand, Prometheus is a systems and service monitoring system. It collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results, and can trigger alerts if some condition is observed to be true.











### References:

https://kubernetes.io/case-studies/
https://kubernetes.io/case-studies/slingtv/
https://www.computerworld.es/tecnologia/ovh-ofrece-kubernetes-administrados-en-su-nube-publica
https://docs.ovh.com/es/hosting/gestion-de-una-base-de-datos-desde-un-alojamiento-compartido/
https://azure.microsoft.com/es-es/services/kubernetes-service/
https://keplerworx.com/kops-vs-amazon-eks/  
https://www.bluematador.com/blog/how-to-monitor-your-kubernetes-cluster-prometheus-vs-datadog












