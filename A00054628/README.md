## Miniproyecto Sistemas Distribuidos

**Universidad ICESI**  
**Curso:** Sistemas Distribuidos  
**Docente:** Miguel Andrés Isaza Barona  
**Tema:**  Orquestación de Contenedores  
**Correo:** miguel11andres@gmail.com

### Objetivos
* Realizar de forma autónoma el aprovisionamiento automático de infraestructura
* Diagnosticar y ejecutar de forma autónoma las acciones necesarias para lograr infraestructuras estables
* Integrar servicios ejecutándose en nodos distintos

### Prerrequisitos
* Conocimientos teórico-prácticos en orquestación de contenedores
* Conocimientos en monitoreo de aplicaciones

### Descripción
Responder las preguntas acerca de orquestación de contenedores

### Actividades
### 1 Realice un gráfico donde muestre la arquitectura de docker swarm y kubernetes. Incluya aspectos como las tecnologías empleadas para la comunicación de sus elementos, descubrimiento de servicio, entre otros.  

*Kubernetes*  

![kubernetes](https://user-images.githubusercontent.com/23371077/58443990-5da75d80-80bb-11e9-8de6-449b5d9375ef.jpg)  

**Componentes:**  

1. Manager  

   * Kube api server: El servidor de la API de Kubernetes valida y configura los datos de los objetos api que incluyen pods, servicios, controladores de replicación y otros. El Servidor API presta servicio a las operaciones REST y proporciona el frontend al estado compartido del clúster a través del cual interactúan todos los demás componentes.  
   
   * etcd: Almacén de valores clave consistente y altamente disponible que se utiliza como almacén de respaldo de Kubernetes para todos los datos de clústeres. Siempre hay que tener un plan de copias de seguridad de los datos del clúster de Kubernetes.  
   
   * Scheduler: componente en el maestro que vigila los PODS recién creados que no tienen ningún nodo asignado, y selecciona un nodo para que se ejecute en él.  
   
   * Control Manager: Componente en el maestro que ejecuta los controladores.  

     Lógicamente, cada controlador es un proceso separado, pero para reducir la complejidad, todos están compilados en un solo binario y      se ejecutan en un solo proceso.  

     Estos controladores incluyen:

     Controlador de nodos: Responsable de notar y responder cuando los nodos se caen.
     Controlador de replicación: Responsable de mantener el número correcto de vainas para cada objeto del controlador de replicación en      el sistema.  
     Controlador de Endpoints: Completa el objeto Endpoints (es decir, se une a Services & Pods).
     Cuenta de Servicio y Controladores de Fichas: Cree cuentas predeterminadas y tokens de acceso a la API para los nuevos espacios de      nombres.  
     
2. Workers  

   * POD: grupo de contenedores que se ejecutan en un sólo nodo, estos comparten direcciones IP, hostnames y otros recursos.
   
   * Kubelet: Un agente que se ejecuta en cada nodo del cluster. Se asegura de que los contenedores estén corriendo en un POD. El kubelet toma un conjunto de PodSpecs que se proporcionan a través de varios mecanismos y asegura que los contenedores descritos en esas PodSpecs están funcionando de manera correcta. El kubelet no gestiona contenedores que no hayan sido creados por Kubernetes.  
   
   * Proxy: Kube-proxy es un proceso que nos ayuda a tener proxy de red y loadbalancer para los servicios en un solo nodo de trabajo. Realiza el enrutamiento de red para paquetes tcp y udp, y realiza el plegado de conexiones. Los nodos de trabajo pueden ser expuestos a Internet a través de kubeproxy.  
   
   * Calico: Calico permite la creación de redes y políticas de red en los clústeres de Kubernetes a través de la nube. Calico funciona en todas partes, en todos los principales proveedores de nube pública y nube privada también.  
   
*Docker Swarm*  

![Docker-Swarm-Production-Environment-Reference-Diagram](https://user-images.githubusercontent.com/23371077/58445832-d8747680-80c3-11e9-969c-b8351f522e4f.png)  
- [x] Imagen tomada de https://www.levvel.io/blog/running-distributed-docker-swarm-on-aws  

**Componentes:**  

1.   Swarm manager: el servicio que un usuario utiliza para gestionar contenedores a través de los swarm managers registrados. Este es el punto final para la interfaz con un entorno Swarm.  

2. Service discovery: proceso que permitirá que otros puedan descubrir la información que almacenamos durante el proceso de registro.  

3. Consul: Consul es un almacén de datos muy consistente que utiliza el "chisme" (gossip) para formar clusters dinámicos. Dispone de un almacén jerárquico de claves/valores que puede utilizarse no sólo para almacenar datos, sino también para registrar relojes que pueden utilizarse para una gran variedad de tareas, desde el envío de notificaciones sobre cambios en los datos hasta la ejecución de comprobaciones de estado y comandos personalizados en función de su salida.  

4. Swarm node: una máquina anfitriona en el Swarm que sólo es responsable de hacer funcionar los contenedores. Como mínimo, un Nodo Swarm necesita tener el Docker Daemon y el Swarm join corriendo en él.  

5. Swarm join: es un proceso que gestiona el registro de un único host con un Administrador de detección de servicios y expone el Docker Daemon del host como un servicio disponible.  

6. Docker Daemon: Un proceso que gestiona la gestión de contenedores en un único host o máquina virtual.  

### 2 Crear una tabla comparativa con al menos 5 diferencias entre Docker Swarm y Kubernetes.  
### 3 Mencione las tecnologías de orquestación de contenedores nativas con kubernetes para al menos tres proveedores de servicios en la nube. Describa sus principales características y limitaciones.  
### 4 Describa las implicaciones a nivel de operaciones de emplear una solución de kubernetes nativa con respecto a desplegar kubernetes manualmente a través de scripts, por ejemplo: KOPS. Tenga en cuenta aspectos de costo y tolerancia a fallas.  
### 5 Crear una tabla comparativa con al menos 5 diferencias entre el monitoreo con Datadog y el monitoreo con Prometheus. Incluya aspectos como costo, obtención de métricas, alertas, entre otros

## Bibiografía  

https://technologyconversations.com/2015/09/08/service-discovery-zookeeper-vs-etcd-vs-consul/  
https://www.levvel.io/blog/running-distributed-docker-swarm-on-aws  
https://www.learnitguide.net/2018/08/what-is-kubernetes-learn-kubernetes.html  
https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/  
