# sd2019a-project
Proyecto Sistemas Distribuidos



## Comparacion DockerSwarm vs Kubernetes

### Arquitectura de Kubernetes

Kubernetes (K8s) es una plataforma de código abierto para automatizar la implementación, el escalado y la administración de aplicaciones en contenedores.

Su Infraestructura se compone de distintos elementos, algunos opcionales y otros obligatorios, como se puede observar en la siguiente grafica:

![alt text](https://res.cloudinary.com/dukp6c7f7/image/upload/f_auto,fl_lossy,q_auto/s3-ghost/2016/06/o7leok.png "Arquitectura basica de kubernetes")

Entre los elementos que lo componen se pueden observar:

* **Pods**: En Kubernetes la administracion de aplicaciones consiste en multiples microservicios comunicandose entre sí. Usualmente estos microservicios se componen de un grupo de contenedores altamente acoplados que en un entorno tradicional se ejecutarian en la misma maquina. Este grupo de contenedores conforma la unidad mas pequeña que puede desplegarse en kubernetes, los pods. Los contenedores dentro de un pod comparten **Espacios de nombre**, **cgroups**, **direccion IP**, entre otros. Estos contenedores son coubicados por lo qu comparten recursos y siempre se programan juntos. Los pods estan pensados para tener vidas cortas.

* **Servicio**: dado que los pods tienen ciclos de vida cortos, no existe una garantia sobre las direcciones IP en las cuales se sirven, lo que podria dificultar la comunicacion entre microservicios. Para esto kubernetes introduce el concepto de servicio, una abstraccion sobre un grupo de pods que usualmente requiere correr un proxy sobre los pods para permitir la comunicacion de otros servicios con el via IP Virtual. Este ademas permite la configuracion de un balanceador de carga sobre los diferentes pods que se exponen via servicio.

* **deployments**: Es la unidad de mas alto nivel que puede ser gestionada en Kubernetes, Permite definir funciones como control de replicas, escalabilidad de los pors, actualizaciones continuas, despliegues automaticos y rollbacks a versiones anteriores.


La interaccion de estos 3 niveles de abstraccion se puede ilustrar de la siguiente forma: 

Los deployments usan replicasets...

![alt text](https://storage.googleapis.com/cdn.thenewstack.io/media/2017/11/07751442-deployment.png "deployment in Kubernetes")


![alt text](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/2017/03/Kubernetes-architecture-logical-constructs-1024x577.png "Service vs Deployment")

Dentro de la infraestructura de K8s existen 2 tipos de Nodos:
* **nodo maestro**
* **nodo esclavo**

#### Nodo Maestro

El nodo maestro es el responsable de la administración del cluster de kubernetes. Actua como punto de entrada para todas las tareas administrativas y de la orquestacion de los nodos esclavos, que es donde realmente corren los servicios.
Dentro de los elementos que se integran en el nodo maestro se encuentran: 
* **API server**: Es el punto de entrada para todos los comandos REST de administracion del cluster. procesa todas las solicitudes REST, las valida y las ejecuta. el estado resultante de estos comandos es almacenado en el siguiente componente, el **almacenamiento etcd**
Master Node

* **etcd**: es un almacenamiento simple, distribuido y consistente de tipo clave valor. Es principalmente usado para compartir las configuraciones y el descubrimiento de servicios. Provee un API REST para operaciones CRUD y una interfaz para el registro de los observadores en los distintos nodos, lo que permite una forma confiable de notificar al resto del cluster cuando se realiza un cambio en la configuración.

* **Scheduler**: el despliegue de los pods y servicios configurados ocurre gracias a este componenete. Este contiene informacion acerca de los recursos disponibles en cada miembro del cluster, asi como de los requeridos para la configuracion y ejecucion de los servicios, y decide en base a esto donde desplegarlos.

* **administrador de controladores**: Opcionalmente kubernetes puede tener distintos tipos de controladores en el nodo maestro. un administrador de controlador es un demonio incrustado en estos. un controlador usa el API server para observar y compartir el estado actual del cluster, ademas de tomar acciones correctivas para llevarlo a un estado deseado. Un ejemplo de esto es un controlador de replicacion, el cual se encarga de observar la cantidad de pods en el sistema, ademas de encargarse de de recrear los que fallen o retirar los que no sean necesarios. Existen otros como controladores de endpoints, de espacio de nombre, entre otros.


#### Nodo esclavo
Es donde los pods son ejecutados, ademas de contener todo lo necesario para la comunicacion de red entre contenedores, la comunicacion con el nodo maestro y la asignacion de los recursos agendados.
Entre sus principales componentes se encuentran:

* **Docker**: docker corre en cada uno de los nodos esclavos y ejecuta los pods configurados, descarga las imagenes y corre los contenedores.

* **Kube-Proxy**: actua como un proxy de red y un balanceador de carga para un servicio en un unico nodo.Además se encarga del enrutamiento de paquetes tcp y udp.

* **Kubelet**: obtiene la configuracion del pod desde el API server y se asegura que los contenedores describidos esten ejecutandose. este es el servicio del esclavo responsable de la comunicación con el nodo maestro.

* **Kubectl**: una herramienta de line ade comandos para comunicarse con el API server y enviar comandos al nodo maestro.

En cuanto a networking, Kubernetes permite la incorporacion de plugins que cumplen con el modelo de red de kubernetes, el cual se basa en que:
* todos los pods se pueden comunicar con otros pots sin usar NAT
* Todos los nodos se pueden comunicar con todos los pots sin usar NAT.
* La IP con la que un POD ve de si mismo es la misma que los demas ven de él.

Estos  Plugins permiten una serie de funcionalidades de red tales como:
* Container-to-Container networking
* Pod-to-Pod networking
* Pod-to-Service networking
* Internet-to-Service networking

Entre los mas destacados se encuentran Calico, Flannel, Canal y Wave Net, por mencionar Algunos.
Pese a que todos ofrecen distintas funcionalidades, su objetivo principal es permitir el networking. La infraestructura de Calico, en general aplicable para todos, es la siguiente:

![alt text](https://i2.wp.com/blog.docker.com/wp-content/uploads/f5755314-2f6c-46b3-a2e3-c3a6c4e17108.jpg?fit=668%2C598&ssl=1 "Calico Architecture")

####Comparativa Kubernetes vs DockerSwarm

|        Caracteristica        |         Kubernetes                 |            DockerSwarm           |
|------------------------------|:-----------------------------------|:---------------------------------|
|Instalación y Configuración   |  Mas compleja de realizar de forma manual. Involucra una gran cantidad de compoenentes como networking, puestos, almacenamiento y rangos de IP para que los pods funcionen de forma correcta|Sencillo de instalar y coonfigurar con pocas complicaciones.un solo conjunto de herramientas es utilizado para configurar el cluster |
|Almacenamiento| Permite compartir volumenes entre los contenedores del mismo pod | permite compartir volumenes con cualquier contenedor en los otros nodos |
|Administración | Provee una CLI, API REST y un panel para controlar y monitorear los servicios| provee una CLI para interactuar con los servicios| 
|Networking | su modelo de red es plano, permitiendo que todos los pods se comuniquen entre si. Cuenta con multiples Plugins de red | se crea una red overlay entre los nodos que se unen al swarm|
| Logging y monitoreo | cuenta con herramientas integradas para loggin y monitoreo| carece de herramientas integradas para esta funcionalidad|
|Balanceo de carga | cada pod es expuesto a través de un servicio, el cual puede ser usado como balanceador de carga dentro de un cluster. tipicamente, ingress es usado| tiene un componente de DNS que puede ser usado para distribuir las solicitudes entrantes hacia un servicio. los servicios pueden correr en puerto especificados o ser asignados automaticamente|
