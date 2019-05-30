Proyecto Sistemas Distribuidos

Jonathan Esteban Arias S.
A00315328

--------------------------------------------------------***Pregunta 1.***----------------------------------------------------------

![Untitled Diagram-Arquitectura Kubernetes](https://user-images.githubusercontent.com/46909824/58590946-d7118e00-822a-11e9-954a-7958b3e563ef.png)

***Descripción de tecnologías empleadas:***

Actualmente se definen las tecnologías por Maestro y por nodo.

-----------***Maestro***---------

Los componentes de maestro proporcionan el plano de control del cluster, es decir, que toman decisiones globales sobre el cluster como: crear nuevos PODS según se requiera

•	***Kube-scheduler:*** Se encarga de observar los PODs recién creados y que no tienen un nodo asignado y selecciona un nodo para que se ajusten

•	***Kube-apiserver:*** Se encarga de exponer la API de kubernetes. Es el extremo frontal del plano de control de kubernetes

•	***Kube-controller-manager:*** Es el encargado de ejecutar los controladores

       Estos controladores incluyen: 
       
             1.	Controlador de nodo: responsable de percatarse cuando un nodo se cae, y de responder por él

             2.	Controlador de replicación: responsable de mantener el numero correcto de PODs para cada objeto del controlador de replicación en el sistema.

             3.	Controlador de puntos finales

             4.	Cuenta de servicio y controladores de token

•	***etcd:*** Es el almacén de valores clave, coherente y de alta disponibilidad usado como almacén de kubernetes para todos los datos del cluster.

---------***Nodos***-------

Los componentes del nodo se ejecutan en cada nodo, garantizando la ejecución de los PODs y proporcionando el entorno de ejecución de Kubernetes

•	***Kubelet:*** Es un agente que se ejecuta en cada nodo del cluster y aún en el master y se asegura de que los contenedores se estén ejecutando en un POD.

•	***Kube-proxy:*** habilita la abstracción del servicio Kubernetes manteniendo las reglas de la red en el host y realizando el reenvió de la conexión.

•	***POD:*** Es un contenedor en el cual se presenta un conjunto de datos a un usuario


![Untitled Diagram-Arquitectura Docker swarm](https://user-images.githubusercontent.com/46909824/58596449-c3b9ef00-8239-11e9-9f32-27ded5285b17.png)

***La arquitectura Docker-swarm consta de managers y workers***

•	***Nodo:*** Es una instancia de un enjambre

•	***Swarm:***que traducido es enjambre, lo conforman un grupo de nodos (Managers y workers)

•	***Nodos Manager:***Reciben las definiciones de servicio del usuario y distribuyen el trabajo a los nodos workers, aunque 
también pueden realizar las tareas de los nodos workers

•	***Nodos Workers:*** recopilan y ejecutan tareas desde el nodo Manager


Tomado de https://www.nubersia.com/es/blog/kubernetes-vs-docker-swarm/

------------------------------------------------- ***Pregunta 2.*** ----------------------------------------------------------

Actualmente Kubernetes es el lider en todas las metricas cuando se compara con Docker swarm  tiene más del 80% del interés en artículos de noticias, popularidad en herramientas como Github y búsquedas en la web



 ***Características*** |![image](https://user-images.githubusercontent.com/46909824/58597845-6cb71880-823f-11e9-80bf-147b1e1b3072.png) | ![image](https://user-images.githubusercontent.com/46909824/58597992-126a8780-8240-11e9-8b96-25c2011c6918.png)
----------------------|------------------------ | ---------------------
***Definición de la aplicación*** |Las aplicaciones se pueden implementar usando una combinación de pods, implementaciones y servicios (o "microservicios").Una implementación puede tener réplicas en múltiples nodos.  | Las aplicaciones se pueden implementar como servicios (o "microservicios") en un clúster Swarm. Las aplicaciones de varios contenedores pueden especificarse utilizando archivos YAML. Docker Compose puede implementar la aplicación. Las tareas se pueden distribuir a través de centros de datos usando etiquetas. 
***Balanceo de carga***| Los pods se exponen a través de un servicio, que se puede usar como un equilibrador de carga dentro del clúster. Por lo general, una entrada se usa para equilibrar la carga. | El modo Swarm tiene un componente DNS que se puede usar para distribuir solicitudes entrantes a un nombre de servicio. Los servicios pueden ejecutarse en los puertos especificados por el usuario o pueden asignarse automáticamente.
***Almacenamiento***| Dos API de almacenamiento:El primero proporciona abstracciones para backends de almacenamiento individual.El segundo proporciona una abstracción para una solicitud de recursos de almacenamiento “Storage”, que puede cumplirse con diferentes backends de almacenamiento.La modificación del recurso de almacenamiento utilizado por el daemon de Docker en un nodo del clúster requiere la eliminación temporal del nodo del clúster.Kubernetes ofrece varios tipos de volúmenes persistentes con soporte de bloque o archivo. Los ejemplos incluyen iSCSI, NFS, FC, Amazon Web Services, Google Cloud Platform y Microsoft Azure.El volumen EmptyDir no es persistente y puede usarse para leer y escribir archivos con un contenedor.| Docker Engine y Swarm admiten volúmenes de montaje en un contenedor.Los sistemas de archivos compartidos, incluidos NFS, iSCSI y el canal de fibra, se pueden configurar nodos. Las opciones de complementos incluyen AWS, Azure, Google Cloud Platform, NetApp, Dell EMC y otros.
***Descubrimiento del servicio*** | Los servicios se pueden encontrar utilizando variables de entorno o DNS.Kubelet agrega un conjunto  de variables de entorno cuando se ejecuta un pod. Kubelet admite variables {SVCNAME_SERVICE_HOST} Y {SVCNAME_SERVICE_PORT} simples, así como también variables compatibles de Docker. El servidor DNS está disponible como un complemento. Para cada servicio de Kubernetes, el servidor DNS crea un conjunto de registros DNS. Si DNS está habilitado en todo el clúster, los pods podrán usar nombres de servicio que se resolverán automáticamente. |El nodo de Swarm Manager asigna a cada servicio un nombre DNS único y equilibrios de carga que ejecutan contenedores. Las solicitudes a los servicios se equilibran de carga a los contenedores individuales a través del servidor DNS integrado en el Enjambre. Docker Swarm viene con múltiples backends de descubrimiento: • Docker Hub como un servicio de descubrimiento alojado está destinado a ser utilizado para desarrollo / prueba. No recomendado para producción. • Se puede usar un archivo estático o una lista de nodos como back-end de descubrimiento. El archivo debe almacenarse en un host al que se pueda acceder desde Swarm Manager. También puede proporcionar una lista de nodos como opción cuando inicie Swarm.
***Rendimiento y escalabilidad*** |A partir de 1.6, Kubernetes escala a clúster de 5.000 nodos. La escalabilidad de Kubernetes se compara con los siguientes objetivos de nivel de servicio (SLO): • Capacidad de respuesta de la API: el 99% de todas las llamadas API devuelven menos de 1s. • Tiempo de inicio del Pod: el 99% de los pods y sus contenedores (con imágenes pre-tiradas) comienzan en 5 segundos.| Docker Swarm ha sido escalado y probado en hasta 30,000 contenedores y 1,000 nodos con 1 administrador de Swarm.
***Actualizaciones de aplicaciones continuas y reversión***| El controlador de implementación es compatible con estrategias de actualización gradual y recreación. Las actualizaciones continuas pueden especificar el número máximo de pods no disponibles o el número máximo que se ejecuta durante el proceso. | En el momento del despliegue, puede aplicar actualizaciones continuas a los servicios. El administrador de Swarm le permite controlar la demora entre la implementación del servicio a diferentes conjuntos de nodos, con lo cual solo se actualizan 1 tarea a la vez.

Tomado de https://www.nubersia.com/es/blog/kubernetes-vs-docker-swarm/

--------------------------------------------------------***Pregunta 3.***----------------------------------------------------------

La siguiente tabla contendrá aquellos servicios propios de algunos proveedores que basan sus servicios en Kubernetes


***Proveedores*** | ***Servicio*** | ***Caracteristicas*** |***Origen de la informacion***
------------------|----------------|------------------------|---------------------------
***Amazon*** | Amazon Elastic Container Service para Kubernetes (Amazon EKS) | •Sin planos de control para administrar •Seguridad por defecto •Creado con la comunidad •Conforme y compatible |https://aws.amazon.com/es/eks/
***Google*** | Google Cloud Kubernetes | •Despliega una amplia variedad de aplicaciones •Operaciones impecables con alta disponibilidad •Escala fácilmente para satisfacer la demanda •Permite disfrutar de una ejecución segura en la red de Google •Permite moverse con libertad entre los entornos in situ y las nubes |https://cloud.google.com/kubernetes-engine/
***Microsoft Azure*** |Azure Kubernetes Service (AKS) | •Protege los clústeres con Active Directory de Azure  •Proporciona su propio registro de contenedores y un portal de aprovisionamiento •Agiliza el desarrollo de aplicaciones de contenedor •Permite administrar Kubernetes fácilmente  •Permite crear sus soluciones sobre una base de nivel empresarial más segura  •Ejecuta cualquier carga de trabajo en la nube, en el perímetro o en un entorno híbrido |https://azure.microsoft.com/es-es/services/kubernetes-service/




--------------------------------------------------------***Pregunta 4.-----------------------------------------------------------

Al hablar de Kubernetes nativa y Kubernetes manualmente a través de scripts en la etapa de operación, Resulta en varias diferencias.
Esto debido a que, en el caso de Kubernetes nativo o conocido como KOPS, se trata de una herramienta de código abierto, por lo que cada usuario de la herramienta está en la libertad de decidir si pagar por mantenimiento de la infraestructura para administrar su clúster Kubernetes o encargarse por cuenta propia. Esto puede resultar en ahorro de costos de operación, pero también representa un arduo trabajo a la hora de garantizar por cuenta propia la operación.
Por otro lado, con Kubertenes manualmente a través de scripts o conocido también como EKS (Amazon Elastic Container Service para Kubernetes), la administración del plano de control ( nodos, maestros, etcd, servidor api) corre por cuenta del proveedor por una tarifa, en este caso de $0.20 dólares por hora o $ 145 dólares por mes. La desventaja de esto es que no tiene acceso a los nodos maestros y no puede realizar ninguna modificación en el plano de control.



--------------------------------------------------------***Pregunta 5.-----------------------------------------------------------


![Datadog_Logo](https://user-images.githubusercontent.com/46909824/58601965-9c225100-8250-11e9-9bae-de053cdfee30.png) | ![promethus](https://user-images.githubusercontent.com/46909824/58601979-ab090380-8250-11e9-9841-e4991d5ae0d5.png)
---------------------------------------|---------------------------------------------------
Monitoreo de muchas aplicaciones (bases de datos, servidores web, etc.)| Potente monitoreo fácil de usar
Configuración fácil| Lenguaje de consulta flexible
Poderosa interfaz de usuario | Modelo de datos dimensional
Potentes integraciones|Comunidad activa y receptiva.
Gran valor | Alertas
Gran visualizacion | Integraciones extensas 
Eventos + métricas = claridad|Fácil de instalar 
Notificaciones| Hermoso modelo y lenguaje de consulta
Metricas personalizadas|Fácil de extender
Flexibilidad|Agradable



Tomado de https://stackshare.io/stackups/datadog-vs-prometheus

