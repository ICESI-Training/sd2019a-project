# sd2019a-project
Proyecto Sistemas Distribuidos

**Nombre:** Daniel Perez Garcia
**correo:** danielperga97@outlook.com


## Comparacion DockerSwarm vs Kubernetes
### DockerSwarm
![alt text](https://blog.octo.com/wp-content/uploads/2017/08/swarm-diagram-1024x480.png "arquitectura basica de dockerswarm")

Hay dos tipos de nodos en Docker Swarm: el nodo administrador y el nodo worker. El concepto de un nodo es una instancia del  Docker engine en el swarm. Es posible ejecutar uno o más nodos en una sola computadora física o máquina virtual en la nube. También es posible ejecutar nodos distribuidos en múltiples máquinas físicas y máquinas virtuales en la nube.

Los dos nodos tienen los siguientes roles diferentes en el modo de enjambre:

**El  nodo administrador** se encarga de enviar las tareas al nodo de trabajo. Una tarea es una unidad de trabajo que transporta un contenedor Docker y los comandos que se ejecutan dentro del contenedor.

**El  nodo worker** ejecuta servicios de enjambre y recibe y ejecuta tareas enviadas desde nodos de administrador.
![alt text](http://sirile.github.io/images/Swarm_target_architecture.png "Arquitectura basica de swarm")

Dockerswarm proporciona una red overlay para la comunicacion entre todos los nodos del swarm. AIn embargo, no incluye herramientas para recoleccion de logs, monitoreo nidescubrimiento de servicios, por lo que se puede recurrir a herramientas como logstash y kibana, prometheus y consul.

La estructura basica de docker swarm se distribuye entre servicios y tareas.

Un servicio es la definición de las tareas a ejecutar en el administrador o en los nodos worker. Es la estructura central del swarm y la raíz principal de la interacción del usuario con el mismo.

Cuando se crea un servicio, especificas qué imagen de contenedor usar y qué comandos ejecutar dentro de los contenedores en ejecución.

En el modelo de servicios replicados , el administrador del swarm  distribuye un número específico de tareas de réplica entre los nodos en función de la escala que establezca en el estado deseado.

Para los servicios globales , el swarm ejecuta una tarea para el servicio en cada nodo disponible en el clúster.

Una tarea lleva un contenedor Docker y los comandos para ejecutarse dentro del contenedor. Es la unidad de programación atómica del swarm. Los nodos del administrador asignan tareas a los nodos worker según el número de réplicas establecidas en la escala de servicio. Una vez que se asigna una tarea a un nodo, no se puede mover a otro nodo. Solo puede ejecutarse en el nodo asignado o fallar.

##### balanceo de carga

El administrador del swarm utiliza el balanceador de carga ingress para exponer los servicios que desea que estén disponibles externamente para el swarm. El administrador del swarm puede asignar automáticamente  un puerto a un servicio publicado o puede configurar el puerto  para el servicio. Puede especificar cualquier puerto no utilizado. Si no especifica un puerto, el administrador del swarm asigna al servicio un puerto en el rango 30000-32767.

El modo de enjambre tiene un componente DNS interno que asigna automáticamente a cada servicio en el swarm una entrada de DNS. El administrador de enjambres utiliza el balanceador de carga interno para distribuir las solicitudes entre los servicios dentro del clúster en función del nombre DNS del servicio.

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

#### Comparativa Kubernetes vs DockerSwarm

|        Caracteristica        |         Kubernetes                 |            DockerSwarm           |
|------------------------------|:-----------------------------------|:---------------------------------|
|Instalación y Configuración   |  Mas compleja de realizar de forma manual. Involucra una gran cantidad de compoenentes como networking, puestos, almacenamiento y rangos de IP para que los pods funcionen de forma correcta|Sencillo de instalar y coonfigurar con pocas complicaciones.un solo conjunto de herramientas es utilizado para configurar el cluster |
|Almacenamiento| Permite compartir volumenes entre los contenedores del mismo pod | permite compartir volumenes con cualquier contenedor en los otros nodos |
|Administración | Provee una CLI, API REST y un panel para controlar y monitorear los servicios| provee una CLI para interactuar con los servicios| 
|Networking | su modelo de red es plano, permitiendo que todos los pods se comuniquen entre si. Cuenta con multiples Plugins de red | se crea una red overlay entre los nodos que se unen al swarm|
| Logging y monitoreo | cuenta con herramientas integradas para loggin y monitoreo| carece de herramientas integradas para esta funcionalidad|
|Balanceo de carga | cada pod es expuesto a través de un servicio, el cual puede ser usado como balanceador de carga dentro de un cluster. tipicamente, ingress es usado| tiene un componente de DNS que puede ser usado para distribuir las solicitudes entrantes hacia un servicio. los servicios pueden correr en puerto especificados o ser asignados automaticamente|

## Tecnologias de orquestacion nativas para kubernetes de distintos proveedores de cloud

### AWS - AMAZON EKS
Amazon Elastic Container Service for Kubernetes (EKS) es un servicio administrado que le permite ejecutar fácilmente Kubernetes en AWS sin necesidad de instalar, operar ni mantener su propio plano de control de Kubernetes. 
Amazon EKS administra automáticamente la disponibilidad y escalabilidad de los nodos del plano de control de Kubernetes responsables de iniciar y detener contenedores, programar contenedores en máquinas virtuales, almacenar datos de clústeres y otras tareas. Amazon EKS detecta y reemplaza automáticamente nodos de plano de control con errores para cada clúster.

* Plano de control de Kubernetes administrado: Amazon EKS proporciona un plano de control escalable y de alta disponibilidad que se ejecuta en múltiples zonas de disponibilidad de AWS.
* Seguridad y redes: Amazon EKS facilita la seguridad de los clústeres Kubernetes, con funciones avanzadas e integraciones con los servicios de AWS y las soluciones de socios tecnológicos.
* Registro: Amazon EKS está integrado con Amazon CloudWatch Logs y AWS CloudTrail para proporcionar visibilidad y realizar un seguimiento del historial de auditoría de su clúster y la actividad del usuario.
* certificado: Amazon EKS ejecuta Kubernetes en sentido ascendente y cumple con los estandares de Kubernetes, por lo que puede utilizar todos los complementos y herramientas existentes de la comunidad.

Desventajas:
No "Agnostico": Amazon EKS es solo una solución para aquellas empresas que desean realizar trabajos en AWS. Es una mala elección si desea mover fácilmente las aplicaciones entre múltiples proveedores de nube pública. También tendrás que manejar la tarea de orquestación de contenedores en estas otras nubes.
No dinámico: incluso si desea utilizar Amazon EKS como parte de un rompecabezas de múltiples nubes más grande, deberá manejar la parte de administración usted mismo. Esto puede plantear desafíos para los modelos dinámicos de múltiples nubes, donde las aplicaciones necesitan moverse rápida y fácilmente entre diferentes proveedores de la nube.
Sin integración: por supuesto, como un servicio exclusivo de AWS, Amazon EKS no ofrece integraciones con otros servicios administrados de Kubernetes, y es probable que no lo haga en el corto plazo.

### AZURE - Azure Kubernetes Service AKS

El Servicio Azure Kubernetes (AKS) es un servicio administrado de orquestación de contenedores, basado en el sistema de código abierto Kubernetes , que está disponible en la nube pública de Microsoft Azure.
AKS características y beneficios
Los principales beneficios de AKS son la flexibilidad, la automatización y la reducción de los gastos generales de gestión para administradores y desarrolladores. Por ejemplo, AKS configura automáticamente todos los maestros y nodos de Kubernetes durante el proceso de implementación y maneja una variedad de otras tareas, incluida la integración de Azure Active Directory, las conexiones a los servicios de monitoreo y la configuración de funciones de red avanzadas, como el enrutamiento de aplicaciones HTTP.
Los nodos AKS pueden escalar hacia arriba o hacia abajo para adaptarse a las fluctuaciones en las demandas de recursos. Para poder de procesamiento adicional, AKS también admite grupos de nodos habilitados por unidades de procesamiento de gráficos ( GPU ) para cargas de trabajo intensivas en cómputo.
 AKS a través proporciona portal de administración de AKS, una interfaz de línea de comandos (CLI) de AKS o el uso de plantillas a través de herramientas como Azure Resource Manager. El servicio también se integra con el Registro de Contenedores de Azure (ACR) para el almacenamiento de imágenes de Docker, y admite el uso de datos persistentes con los Discos de Azure.
 Desventajas
 -Tiempos de aprovisionamiento elevados
 -Tiempos de montado elevados para volumenes (aprox 7 min)
 -Demoras al actualizar la duración del cluster
 -Restricciones al mismo tipo de nodo
 
 
 ### GOOGLE - GCE
 
Google Container Engine es un  administrador de clústeres y sistema de organización para ejecutar contenedores de Docker. Permite programar los contenedores en el clúster y los administra automáticamente según los requisitos que sean definidos (por ejemplo, de CPU y memoria). Se basa en el sistema de código abierto Kubernetes y uno de sus puntos duertes consiste en la flexibilidad de aprovechar una infraestructura de nube local, híbrida o pública.

Google GCE permite configurar un cluester en pocos minutos, ademas cuenta con la posibilidad de ejecutar administración declarativa de los requisitos de los contenedores tales como CPU,Memoria, replicas y politicas de persistencia haciendo uso de un archivo en formato JSON.

* Entre sus principales caracteristicas hace enfasis en: 
* Compatibilidad con Docker
* Registro de contenedores privado
* Escalabilidad
* Registro y supervisión con los servicios stackdriver logging y monitoring
* Completamente administrado
* Soporte de Redes híbridas
* Administración de identidades y accesos

Desventajas (reportadas por usuarios):
* Documentación mejorable
* poca observabilidad, plataforma de monitoreo mejorable
* errores ocasionales del balanceador de carga
* Algoritmo contra fraude poco flexible

La siguiente tabla proporciona una compartativa interesante entre las 3 tecnologias :) ( a febrero de 2019) :


![alt text](https://i1.wp.com/kubedex.com/wp-content/uploads/2018/10/Screen-Shot-2018-10-14-at-11.53.07.png?resize=1024%2C799&ssl=1 "Comparativa kuberntes in cloud providers")


## Soluciones Nativas vs Soluciones manuales

Para esto partiremos de un analisis que compare Kops con EKS en AWS, siendo la mayoria de los criterios aplicables a cualquier otro cloud provider.
Cuando se implementa una solución como kops, depende de nosotros configurar y administrar muchos de los servicios que comunmente serian administrados por nuestro proveedor de nube. Al desplegar un cluster de kubernetes con kops tendremos la instrumentacion por defecto, es decir, no contaremos con RBAC o administracion de usuarios, a diferencia de si utilizaremos el servicio de EKS, el cual brinde integracion con aws iam y el autenticador de codigo abierto de aws para control de acceso.
Al usar una solución como Kops deberemos encargarnos (para bien o para mal) de administrar una serie de aspectos como el estado del cluster, de los nodos y de nuestro almacenamiento etcd, a diferencia de lo que ocurriria en una solución como aws eks.
Por otro lado, con Kops tambien tendremos de encargarnos del networking con cualquiera de las opciones de CNI comptibles con amazon mientras que en una solución nativa tendremos integracion con la solucion de red nativa, vpc en el caso de eks.

El administrar todos los recursos que implica utilizar kops implicaa contar con una persona que cuente con los conocimientos necesarios para ello, ademas de necesitar mas tiempo a nivel de planeacion antes de desplegar un cluster. Dado que en las soluciones nativas muchos de los servicios son administrados por el proveedor del servicio se reducen considerablemente los tiempos de despliegue y se facilita la administracion a costa de un mayor valor economico. Por otro lado, la soluciones nativas al ser administradas en muchos aspectos son mucho menos persnoalizables y adaptables.


## Datadog Vs Prometheus

|        Caracteristica        |         Datadog                 |            Prometheus           |
|------------------------------|:-----------------------------------|:---------------------------------|
| Licencia | comercial | de codigo abierto |
| precio | planes de infraestructura desde $15 por host/mes y $31 incluyendo APM | gratuito |
| modelo de despliegue | servicio en la nube con agente local | contenedor, instalacion local |
| tipo de solución | completa (logs, monitoreo y alertas) | no incluye logs |
|dashboard | incluido | disponible mediante grafana |
| service discovery | disponible | disponible mediante modulos |
| Alertas | Prometheus alert manager | built in GUI |

### Referencias
https://www.bluematador.com/blog/how-to-monitor-your-kubernetes-cluster-prometheus-vs-datadog
https://docs.docker.com/engine/swarm/key-concepts/
https://www.trustradius.com/products/google-kubernetes-engine/reviews/pros-and-cons
https://www.g2.com/products/azure-kubernetes-service-aks/reviews
https://medium.com/faun/current-limitations-running-azure-aks-in-production-55b830e89f3c
https://azure.microsoft.com/en-us/services/kubernetes-service/
https://keplerworx.com/kops-vs-amazon-eks/
https://www.bluematador.com/blog/kubernetes-on-aws-eks-vs-kops
https://aws.amazon.com/es/eks/features/
https://kubedex.com/google-gke-vs-microsoft-aks-vs-amazon-eks/
