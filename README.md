# Proyecto Final Sistemas Distribuidos

**Nombre:** Marisol Giraldo Cobo

**Código:** A00246380

**Correo:** marisol.giraldo@correo.icesi.edu.co

**Curso:** Sistemas Distribuidos

**Tema:**  Orquestación de Contenedores

**Profesor:** Daniel Barragán

# Objetivos
- Realizar de forma autónoma el aprovisionamiento automático de infraestructura
- Diagnosticar y ejecutar de forma autónoma las acciones necesarias para lograr infraestructuras estables
- Integrar servicios ejecutándose en nodos distintos

# Prerrequisitos
- Conocimientos teórico-prácticos en orquestación de contenedores
- Conocimientos en monitoreo de aplicaciones

# Actividades
**1. Realice un gráfico donde muestre la arquitectura de docker swarm y kubernetes. Tenga en cuenta Incluya aspectos como las tecnologías empleadas para la comunicación de sus elementos, descubrimiento de servicio, entre otros.**

**R/.**

# DOCKER SWARM
Se basa en una arquitectura maestro-esclavo. Cada clúster de Docker está formado al menos por un nodo maestro (también llamado administrador o **manager**) y tantos nodos esclavos (llamados de trabajo o **workers**) como sea necesario. Mientras que el maestro de Swarm es responsable de la gestión del clúster y la delegación de tareas, el esclavo se encarga de ejecutar las unidades de trabajo (tasks o tareas). 

**COMPONENTES**

**-Docker Daemon** Un proceso que maneja la administración de contenedores en un solo host o vm.

**-Swarm Join** Un proceso que controla el registro de un solo host con un Service Discovery Manager y la exposición del Docker Daemon del host como un servicio disponible.

**-Swarm Node** Este no es un término oficial de Docker, sino una asociación lógica para una máquina host en el Enjambre que solo es responsable de ejecutar contenedores

**-Swarm Manager** El servicio que un usuario utiliza para administrar los contenedores a través de Nodos swarm registrados. Este es el punto final para interactuar con un entorno de Swarm.

**-Service Discovery** Estos administradores rastrean los servicios, miembros y sesiones registrados entre réplicas de ellos mismos.

**-Swarm Token** Discovery Service, se sirve de un token para reconocer todos nodos que forman parte de un clúster.

![Untitled Diagram-Page-1](https://user-images.githubusercontent.com/35766585/58603119-2a003b00-8255-11e9-8c0a-8b0473bdb135.png)


# KUBERNETES
Kubernetes es un sistema de código libre para la automatización del despliegue, ajuste de escala y manejo de aplicaciones en contenedores​ que fue originalmente diseñado por Google y donado a la Cloud Native Computing Foundation. Soporta diferentes ambientes para la ejecución de contenedores, incluido Docker.

**-Nodo** es un término común para representar a las máquinas virtuales y / o servidores bare-metal que gestiona Kubernetes.

**-Pod** que es una unidad básica de implementación en Kubernetes. Un pod una colección de contenedores Docker relacionados que se ejecutan en conjunto.

**-Master Node** el cual es corazón donde Kubernetes se encuentra instalado, este nodo controla los pods a través de los **Worker Nodes** el cual es el segundo tipo de nodo de la arquitectura.

En el Master Node encontramos los siguientes componentes:

**-Kube-API server** donde se controlan todas las operaciones de comunicación desde los Nodos Docker o Worker Nodes hasta el Master Node , en consecuencia, todo en la plataforma Kubernetes se trata como un objeto de API.

**-Kubi-Controller Manager** el cual es responsable de verificar el estado actual de los nodos y sus pods, ademas toma las decisiones para lograr un estado optimo del Cluster Kubernetes. Escucha y colecta la información que requiere desde el Kube-API Server para ejecutar sus funciones.

**-Kubi-Scheduler** el cual decide como se programan los eventos de balanceo de cargas según la disponibilidad de recursos del cluster y las políticas establecidas por los administradores. Al Igual que Kubi-Controller-Manager, escucha y colecta la información que requiere desde el Kube-API Server para sus funciones.
**etcd:** Kubernetes usa etcd para almacenar sus datos, configuraciones, estado y metadata. En otras palabras, etcd es la base de datos de Kubernetes.

En los Worker Nodes, tenemos los siguientes componentes:

**-kubelet** un agente que corre en cada nodo del cluster. Verifica que cada contenedor sea ejecutado en un pod, ademas asegura que los contenedores se ejecuten y mantengan en su estado esperado de funcionamiento. Kubelet no controla contenedores que no fueron creados por Kubernetes.

**-kube-proxy** permite que varios microservicios de las aplicaciones se comuniquen con otros que se están ejecutando en los diferentes nodos del Cluster Kubernetes, ademas habilita el acceso a las aplicaciones a los usuarios finales.

**-Container Runtime** es el software responsable de ejecutar los contenedores. Kubernetes soporta diferentes runtimes, tales como: Docker, containerd, rktlet.

![Untitled Diagram-Page-2 (1)](https://user-images.githubusercontent.com/35766585/58603158-4c925400-8255-11e9-8ebc-c3cc88ce490a.png)

**2. Crear una tabla comparativa con al menos 5 diferencias entre Docker Swarm y Kubernetes**

**R/.**

# TABLA COMPARATIVA DOCKER SWARM vs KUBERNETES
![Sin título](https://user-images.githubusercontent.com/35766585/58609054-dc42fd00-826b-11e9-9c3e-e46b98cdf743.png)
![Sin título](https://user-images.githubusercontent.com/35766585/58606067-ac8df800-825f-11e9-9482-fffa7543c728.png)


**3. Mencione las tecnologías de orquestación de contenedores nativas con kubernetes para al menos tres proveedores de servicios en la nube. Describa sus principales características y limitaciones**

**R/.**

# SUSE Cloud Application Platform

![Sin título](https://user-images.githubusercontent.com/35766585/58609140-26c47980-826c-11e9-80e4-d3ceca3a1502.png)

**Caracteristicas**

- SUSE ha anunciado una nueva implementación nativa de Kubernetes para la plataforma de desarrollo Cloud Foundry y se ha integrado en **SUSE Cloud Application Platform**, avanzando en el objetivo de la compañía de ayudar a las organizaciones empresariales a mejorar la innovación, acelerar el despliegue de aplicaciones y aumentar su agilidad de negocio. 

- SUSE Cloud Application Platform provee a los usuarios de Kubernetes la mejor experiencia DevOps cloud native, combinando las tecnologías de Kubernetes y Cloud Foundry.  

- SUSE Cloud Application Platform incrementa la productividad de los desarrolladores con una automatización que elimina la necesidad de crear y gestionar imágenes de contenedores.

- SUSE ha incluido tecnología del proyecto de código abierto Eirini, la cual permite que Kubernetes se utilice como motor de orquestación de contenedores para desplegar y gestionar aplicaciones a través de Cloud Foundry. Eirini permite elegir la funcionalidad de Kubernetes para orquestar instancias de contenedores de aplicaciones en SUSE Cloud Application Platform, conservando al mismo tiempo la experiencia de usuario de Cloud Foundry, tecnología muy familiar entre los desarrolladores de aplicaciones.

**Limitaciones**

- Más adelante planea incluir soporte para diferentes proveedores de la nube pública.

# EKS

![Sin título](https://user-images.githubusercontent.com/35766585/58609720-3e9cfd00-826e-11e9-90f7-cc7dcd1af307.png)


**Caracteristicas**

- AWS facilita la ejecución de Kubernetes en la nube mediante una infraestructura de máquinas virtuales escalable y de alta disponibilidad, integraciones en servicios respaldadas por la comunidad y **Amazon Elastic Container Service for Kubernetes (EKS)**, un servicio administrado de Kubernetes que cuenta con certificación de conformidad.

- AWS facilita la ejecución de Kubernetes. Puede optar por ocuparse de la administración de la infraestructura de Kubernetes con Amazon EC2 o adquirir un plano de control de Kubernetes aprovisionado y administrado de manera automática con Amazon EKS. Independientemente de la estrategia que elija, obtendrá integraciones eficientes y respaldadas por la comunidad con servicios de AWS como VPC, IAM y detección de servicios, además de la seguridad, escalabilidad y alta disponibilidad de AWS.

- Amazon EKS aprovisiona y escala el plano de control de Kubernetes, incluidos los servidores de las API y la capa de persistencia backend, en varias zonas de disponibilidad de AWS para lograr una disponibilidad alta y tolerancia a errores. 

- Las aplicaciones que se ejecutan en Amazon EKS son totalmente compatibles con las aplicaciones que se ejecutan en cualquier entorno de Kubernetes estándar, independientemente de si se ejecutan en centros de datos en las instalaciones o en nubes públicas. Esto significa que puede migrar fácilmente cualquier aplicación de Kubernetes estándar a Amazon EKS sin tener que modificar el código.

- Amazon EKS realiza actualizaciones de clúster administradas in situ para Kubernetes y las versiones de la plataforma de Amazon EKS. De este modo se simplifican las operaciones de clúster y le permite aprovechar las últimas características de Kubernetes, así como las actualizaciones de la configuración y los parches de seguridad de Amazon EKS.

**Limitaciones**

- Amazon EKS actualmente es compatible con la versión 1.10.11 y 1.11.5 de Kubernetes y seguirá añadiendo compatibilidad con versiones adicionales de Kubernetes en el futuro.

- Las nuevas versiones de Kubernetes incorporan cambios significativos en la API de Kubernetes y, como resultado, pueden provocar cambios en el comportamiento de la aplicación. Por ello, es necesario tener en cuenta el control manual de la versión de Kubernetes en el clúster para probar las aplicaciones en nuevas versiones de Kubernetes antes de actualizar los clústeres de producción. 

# OVH

![descarga](https://user-images.githubusercontent.com/35766585/58610976-1368dc80-8273-11e9-9872-2ab438a89548.png)

**Caracteristicas**

- El servicio Managed Kubernetes se basa en las instancias de Public Cloud de OVH. Gracias a los balanceadores de carga y a los discos adicionales integrados, se puede alojar todo tipo de cargas y garantizar una reversibilidad total.

- El equipo de OVH se encarga de desplegar, alojar, monitorizar y mantener operativos y actualizados todos los componentes internos del servicio de forma gratuita. Así usted el usuario podrá centrarse en gestionar sus contenedores y servicios mediante las herramientas estándar del ecosistema Kubernetes.

- Servicio Kubernetes gratuito, administrado y con alta disponibilidad para orquestar sus aplicaciones contenerizadas en el cloud de OVH

-  Balanceador de carga integrado en Kubernetes de manera nativa, la posibilidad de ajustar la configuración de la política de actualizaciones de seguridad y la opción de elegir entre las versiones más recientes del servicio que ofrecen otros proveedores de soluciones administradas: las versiones 1.11 y 1.12.

- OVH proporciona de manera gratuita el software de orquestación Kubernetes y la infraestructura en la que se ejecuta.

**Limitaciones**

-  Pagar por el almacenamiento (volúmenes persistentes) y la potencia de cálculo de la infraestructura en la que corren sus contenedores. 

- Managed Kubernetes está disponible actualmente solo en el centro de datos de OVH de Gravelines (Francia)

- Pronto será compatible con la tecnología vRack, lo que permitirá que los clientes de OVH puedan conectar y desarrollar infraestructuras privadas híbridas en un perímetro multidatacenter a nivel mundial. 

**4. Describa las implicaciones a nivel de operaciones de emplear una solución de kubernetes nativa con respecto a desplegar kubernetes manualmente a través de scripts, por ejemplo: KOPS. Tenga en cuenta aspectos de costo y tolerancia a fallas.**

**R/.**

Existen tres métodos populares para ejecutar Kubernetes en AWS: 
1. Configurar manualmente todo en instancias de EC2
2. Usar Kops para administrar su clúster 
3. Usar Amazon EKS para administrar su clúster. 

Administrar un clúster de Kubernetes en AWS sin ninguna herramienta es un proceso complicado que no se recomienda para la mayoría de los administradores, por lo que nos centraremos en el uso de EKS o Kops. A continuación, compararé las características de configuración, administración y seguridad del cluster tanto para Kops como para EKS para determinar qué solución debe usar.

# EKS

![Sin título](https://user-images.githubusercontent.com/35766585/58609720-3e9cfd00-826e-11e9-90f7-cc7dcd1af307.png)

Amazon EKS estuvo disponible en general en junio de 2018 y se describe como un "Servicio de Kubernetes altamente disponible, escalable y seguro". Este tipo de descripción en el espacio de Kubernetes nos podría hacer creer que puede tener una configuración de clúster de Kubernetes con un solo clic, lo cual no es cierto. EKS administra completamente solo el plano de control de Kubernetes (nodos maestros, etcd, servidor api), por una tarifa de uso fija de $ 0.20 por hora o ~ $ 145 por mes. La desventaja de esto es que no tiene acceso a los nodos maestros y no puede realizar ninguna modificación en el plano de control. 

**Caracteristicas**

- Configurar un clúster con EKS es bastante complicado y tiene algunos requisitos previos. Debe configurar y utilizar tanto la CLI de AWS como el aws-iam-authenticator para autenticarse en el clúster, por lo que existe cierta sobrecarga con la configuración de permisos y usuarios de IAM. Como EKS no crea nodos de trabajo automáticamente con elclúster de EKS, también debe administrar ese proceso.

- Amazon proporciona instrucciones y plantillas de CloudFormation que pueden lograr lo anterior, pero es posible que algunas de ellas deban modificarse para que funcionen con  requisitos específicos, como volúmenes raíz cifrados o nodos en ejecución en subredes privadas. 

- El equipo de Amazon EKS está buscando activamente problemas de seguridad y es probable que tenga la próxima versión de Kubernetes parcheada lista muy rápidamente cuando se descubra la próxima vulnerabilidad.

- Una vez que su cuenta de AWS no tiene acceso de root a los nodos maestros de su clúster, tiene una capa adicional de protección.


# KOPS

![descarga (1)](https://user-images.githubusercontent.com/35766585/58612209-65abfc80-8277-11e9-98ea-de517d87106a.png)

Kubernetes Operations (kops) es una herramienta CLI para “Instalación, actualizaciones y administración de K8 de grado de producción”. Kops ha existido desde finales de 2016, mucho antes de que existiera EKS.
Kops simplifica significativamente la configuración y administración del clúster Kubernetes en comparación con la configuración manual de los nodos maestro y de trabajo. Administra Route53, los grupos de AutoScaling, los ELB para el servidor de la API, los grupos de seguridad, el arranque maestro, el arranque del nodo y las actualizaciones continuas de su clúster. Dado que kops es una herramienta de código abierto, es de uso completamente gratuito, pero el usuario es responsable de pagar y mantener la infraestructura subyacente creada por kops para administrar su clúster Kubernetes.

**Caracteristicas**

- Kops es una herramienta CLI y debe instalarse en la máquina local junto con kubectl. Hacer que un clúster se ejecute es tan simple como ejecutar el comando kops create cluster con todas las opciones necesarias. 

- Kops administrará la mayoría de los recursos de AWS necesarios para ejecutar un clúster Kubernetes y trabajará con una VPC nueva o existente. 

- A diferencia de EKS, kops también creará los nodos maestros como instancias de EC2, y podrá acceder a esos nodos directamente y realizar modificaciones. Con acceso a los nodos maestros, puede elegir qué capa de red usar, elegir el tamaño de las instancias maestras y monitorear directamente los nodos maestros. 

- También tiene la opción de configurar un clúster con un solo maestro, lo que puede ser conveniente para entornos de prueba y desarrollo donde la alta disponibilidad no es un requisito. Kops también admite la configuración de terraform para sus recursos en lugar de crearlos directamente, lo que es una buena característica si utiliza terraform.

* Como Kops tiene un alcance limitado para administrar solo la infraestructura necesaria para ejecutar Kubernetes, la seguridad del clúster depende casi totalmente del usuario. Los clusters de Kops aún se benefician del Modelo de Responsabilidad Compartida de Amazon en lo que respecta a EC2 y otros servicios, pero sin el beneficio de la experiencia o el soporte de seguridad adicional con el propio plano de control de Kubernetes.

- La comunidad kops es muy activa en el canal # kops-users Slack y probablemente el usuario obtendra una respuesta adecuada en la mayoría de las preguntas sobre kops allí.

- Dado que kops es una herramienta de código abierto, es de uso completamente gratuito, pero el usuario es responsable de pagar y mantener la infraestructura subyacente creada por kops para administrar su clúster Kubernetes.


**5. Crear una tabla comparativa con al menos 5 diferencias entre el monitoreo con Datadog y el monitoreo con Prometheus. Incluya aspectos como costo, obtención de métricas, alertas, entre otros.**

**R/.**

![Sin título](https://user-images.githubusercontent.com/35766585/58608770-bcf7a000-826a-11e9-95f0-0c4e37121f7f.png)
![Sin título](https://user-images.githubusercontent.com/35766585/58608818-e9132100-826a-11e9-9ad3-ab50ee73e3be.png)



# BIBLIOGRAFÍA

Learn How To Use The Docker Api To Manage Containers And Images
https://blog.eduonix.com/software-development/learn-how-to-use-the-docker-api-to-manage-containers-and-images/

Using Docker Swarm
https://www.penflip.com/akira.ohio/appcatalyst-hands-on-lab-en/blob/master/docker-swarm.txt

Kubernetes
https://oscarzuniga.blog/2019/03/23/diferencias-entre-kubernetes-y-docker/#.XO8zshZKjIU

EKS
https://aws.amazon.com/es/eks/

OVH
https://www.ovh.es/kubernetes/

Kops vs EKS
https://keplerworx.com/kops-vs-amazon-eks/



