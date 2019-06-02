## Miniproyecto Sistemas Distribuidos

**Universidad ICESI**  
**Curso:** Sistemas Distribuidos  
**Alumno:** Joan Sebastián García Delgado  
**Tema:**  Orquestación de Contenedores  
**Correo:** joan.garcia1@correo.icesi.edu.co

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

**Características** | Kubernetes | Docker Swarm
--------------- | ---------- | ------------
**Definición de la aplicación** | Las aplicaciones se pueden implementar usando una combinación de pods, implementaciones y servicios (o "microservicios"). | Las aplicaciones se pueden implementar como servicios (o "microservicios") en un clúster Swarm. Las aplicaciones de varios contenedores pueden especificarse utilizando archivos YAML.  
**Construcciones de escalabilidad de aplicaciones** | Cada nivel de aplicación se define como un pod y se puede escalar cuando se administra mediante una implementación, que se especifica en YAML, la escala puede ser manual o automatizada. | Los servicios se pueden escalar utilizando plantillas Docker Compose YAML.  
**Alta disponibilidad** | Las implementaciones permiten que los pods se distribuyan entre los nodos para proporcionar Alta Disponibilidad, tolerando así las fallas de la aplicación. | Los servicios se pueden replicar entre los nodos de Swarm. Los administradores de Swarm son responsables de todo el clúster y administran los recursos de los nodos de trabajadores.  
**Balanceo de carga** | Los pods se exponen a través de un servicio, que se puede usar como un equilibrador de carga dentro del clúster.  | El modo Swarm tiene un componente DNS que se puede usar para distribuir solicitudes entrantes a un nombre de servicio. Los servicios pueden ejecutarse en los puertos especificados por el usuario o pueden asignarse automáticamente.  
**Descubrimiento del servicio** | Los servicios se pueden encontrar utilizando variables de entorno o DNS. Kubelet agrega un conjunto de variables de entorno cuando se ejecuta un pod. | El nodo de Swarm Manager asigna a cada servicio un nombre DNS único y equilibrios de carga que ejecutan contenedores. Las solicitudes a los servicios se equilibran de carga a los contenedores individuales a través del servidor DNS integrado en el Enjambre.  

### 3 Mencione las tecnologías de orquestación de contenedores nativas con kubernetes para al menos tres proveedores de servicios en la nube. Describa sus principales características y limitaciones.  

1. **Google Cloud GKE:**  

* Kubernetes Engine es un entorno gestionado y listo para la producción para desplegar aplicaciones en contenedores. Aporta nuestras últimas innovaciones en productividad de desarrolladores, eficiencia de recursos, operaciones automatizadas y flexibilidad de código abierto para acelerar el tiempo de lanzamiento al mercado.  

* Kubernetes Engine se basa en la experiencia de Google de ejecutar servicios como Gmail y YouTube en contenedores durante más de 12 años. El Motor de Kubernetes permite el despliegue de Kubernetes eliminando por completo la necesidad de instalar, administrar y operar nuestros propios clústeres de Kubernetes.  

* Kubernetes Engine permite un rápido desarrollo e iteracción de aplicaciones al facilitar la implementación, actualización y administración de nuestras aplicaciones y servicios.  

* Las organizaciones suelen utilizar Google Kubernetes Engine para:

  * Crear o cambiar el tamaño de los clústeres de contenedores Docker.
  * Crear módulos de contenedores, controladores de replicación, trabajos, servicios o balanceadores de carga.
  * Redimensionar controladores de aplicaciones.
  * Actualizar y actualizar clústeres de contenedores.
  * Grupos de contenedores de depuración.

* **Limitaciones:**  

  * GKE todavía se considera nuevo. Se podría mejorar la documentación y el apoyo. Tampoco está bien integrado en el resto de Google Cloud Platform.  

  * Desde un punto de vista más técnico, algunos se quejan de que la capacidad de configuración de GKE es limitada, incluido el control limitado de la autenticación, la autorización o el control de admisión.   

2. **Amazon EKS:**  

* El servicio Amazon Elastic Container Service for Kubernetes (EKS) es un servicio gestionado por Kubernetes que facilita la ejecución de Kubernetes en AWS sin necesidad de instalar, operar y mantener el plano de control de Kubernetes. Amazon EKS está certificado como Kubernetes, por lo que las aplicaciones existentes que se ejecutan en Kubernetes son compatibles con Amazon EKS.  

* Amazon EKS proporciona un plano de control escalable y altamente disponible que corre a través de múltiples zonas de disponibilidad de AWS. El servicio Amazon EKS gestiona automáticamente la disponibilidad y escalabilidad de los servidores de la API de Kubernetes y la capa de persistencia de etcd para cada clúster. Amazon EKS ejecuta el plano de control de Kubernetes en tres zonas de disponibilidad para asegurar una alta disponibilidad, y detecta y reemplaza automáticamente a los nodos maestros "unhealthy".  

* **Limitaciones:**  

   * No es dinámico: Uno mismo debe ocuparse de la administración. Esto puede suponer un reto para los modelos dinámicos multi-nube, en los que las aplicaciones deben moverse rápida y fácilmente entre diferentes proveedores de cloud computing.  

   * No hay integración: Amazon EKS no ofrece integraciones con otros servicios gestionados de Kubernetes.  

3. **IBM Cloud Kubernetes:**  

* Actualizaciones rápidas, altamente escalables.

* Los despliegues son rápidos. 

* Tolerante a fallos.

* **Limitaciones:**  

  * Es muy poco fiable. Los nodos y máquinas muchas veces se caen aleatoriamente sin previo aviso o mensajes de error útiles.  

  * La documentación no es buena.  

### 4 Describa las implicaciones a nivel de operaciones de emplear una solución de kubernetes nativa con respecto a desplegar kubernetes manualmente a través de scripts, por ejemplo: KOPS. Tenga en cuenta aspectos de costo y tolerancia a fallas.  

Para ello vamos a compara KOPS contra AWS EKS.  

**EKS**  

![bluematador-aws-EKS](https://user-images.githubusercontent.com/23371077/58520877-8a24ad80-817f-11e9-8387-49d5dd4004e6.png)  

Amazon EKS se lanzó en junio de 2018, este se describe como un servicio de Kubernetes altamente disponible, escalable y seguro. EKS administra completamente sólo el plano de control de Kubernetes (nodos maestros, etcd, api server), por  $0.20 por hora o ~$145 por mes. La desventaja es que no tiene acceso a los nodos maestros y no puede hacer ninguna modificación en el plano de control.

**KOPS**  

![kops](https://user-images.githubusercontent.com/23371077/58520937-dd96fb80-817f-11e9-9f4b-28d396dd204b.jpg)  

Es una herramienta de CLI para "Instalación, Actualizaciones y Gestión de K8s de Grado de Producción". Kops ha existido desde finales de 2016, mucho antes de que existiera EKS.  

Kops simplifica de manera significativa la configuración y gestión de los clústeres de Kubernetes en comparación con la configuración manual de los nodos maestros y workers. Administra Route53, AutoScaling Groups, ELBs para el api server, grupos de seguridad, bootstrapping maestro, bootstrapping de nodos y actualizaciones de rodamiento para su cluster. Dado que kops es una herramienta de código abierto de uso completamente gratuito, uno mismo es responsable de pagar y mantener la infraestructura creada por kops para gestionar el clúster de Kubernetes.  

**KOPS vs EKS: Cluster Setup**  

*EKS:* Configurar un clúster con EKS es bastante complicado y tiene algunos requisitos previos. Se debe configurar y utilizar tanto el AWS CLI como el aws-iam-authenticator para autenticarse en el clúster, de modo que la configuración de los permisos y usuarios de IAM tiene cierta sobrecarga. Dado que EKS no crea nodos de trabajo automáticamente con nuestro clúster EKS, también se debe administrar ese proceso.

*KOPS:* Kops es una herramienta CLI y debe ser instalada en su máquina local junto con kubectl. Ejecutar un cluster es bastante simple, es sòlo ejecutar el comando ***kops create cluster con todas*** las opciones necesarias. Kops gestionará la mayor parte de los recursos de AWS necesarios para ejecutar un clúster de Kubernetes, y trabajará con un VPC nuevo o ya existente. A diferencia de EKS, kops también creará nodos maestros como instancias de EC2, y se puede acceder a esos nodos directamente y hacer modificaciones. Con acceso a los nodos maestros, se puede elegir qué capa de red utilizar, elegir el tamaño de las instancias maestras y supervisar directamente los nodos maestros. También està la opción de configurar un clúster con un solo maestro, lo que podría ser deseable para entornos de desarrollo y prueba donde la alta disponibilidad no es un requisito.  

**KOPS vs EKS: Cluster Management**  

*EKS:* EKS tiene una forma prescrita de actualizar la versión de Kubernetes del plano de control con una interrupción mínima. Sus nodos de trabajo pueden ser actualizados usando un nuevo AMI para la nueva versión de Kubernetes, creando un nuevo grupo de trabajo, y luego migrando la carga de trabajo a los nuevos nodos. La ampliación del clúster con EKS es simple, sólo es añadir más nodos de trabajo. Dado que el plano de control está totalmente gestionado, no hay necesidad de preocuparnos por añadir o actualizar los tamaños maestros cuando el clúster se agrande.  

*KOPS:*  KOPS es muy bueno para crear un clúster de manera ràpida, el problema es que hay que hacer mucho trabajo para actualizar y reemplazar los nodos maestros de las nuevas versiones de Kubernetes. Actualizar una cantidad de nodos de AWS con KOPS puede tomar menos de 1 día, pero si se quiere que la interrupción sea mínima, es mejor crear un nuevo cluster en una nueva VPC con KOPS.  

**Tabla comparativa:**  

Kubernetes | Amazon EKS
---------- | ----------
Soporte para varias versiones de Kubernetes | No soporta todas las versiones de Kubernetes  
La instrumentación inicial, por defecto, será no añadir ningún RBAC o gestión de usuarios | Estrechamente acoplado con Amazon IAM, y aprovechar el autenticador AWS de código abierto para el control de acceso  
Totalmente manejado por usted mismo, puede ser una ventaja o desventaja para el uso futuro | Gestionado por AWS  
Hay que asegurarse de que el cluster etcd estè levantado y en ejecución | Gestionado por AWS  
Asegurarse de que los servicios de los nodos maestro estén en ejecución (Kube api server, kube DNS, kube-scheduler etc) | Gestionado por AWS  

Como siempre, la implementaciñon de una de estas herramientas depende de las necesidades del usuario, en mi opinión considero que Amazon EKS es mucho mejor debido a que ya tiene automatizada varias tareas que depronto pueden ser un poco molestas para nosotros.  

### 5 Crear una tabla comparativa con al menos 5 diferencias entre el monitoreo con Datadog y el monitoreo con Prometheus. Incluya aspectos como costo, obtención de métricas, alertas, entre otros  

**Características**| Prometheus | Datadog | Conclusión    
------------------ | ---------- | ------- | ----------  
**Instalación** | Todo lo que necesita para hacerlo es crear un rol de clúster, un mapa de configuración y una implementación para Prometheus, la mayoría de los cuales se pueden copiar pegados de cualquier número de tutoriales en línea (aquí hay uno para empezar). Sólo se tarda un par de minutos en prepararse. | Simplemente configurará los permisos RBAC e instalará el agente Datadog como un DaemonSet. Si desea enviar métricas personalizadas a Datadog, necesitará hacer un poco más de configuración. | Datadog es un poco más fácil de instalar que Prometheus, como es de esperar cuando se compara un servicio gestionado con una solución de código abierto. Sin embargo, tampoco es demasiado difícil.    
**Visualización de métricas** | Para la visualización de métricas Prometheus depende de Grafana, esta facilita la creación de cuadros de mando y gráficos de métricas. Hay diferentes widgets y opciones que permiten adaptar la visualización de acuerdo a nuestras necesidades. | Datadog proporciona gráficos y cuadros de mando muy completos con un gran rendimiento. Al igual que Grafana, hay muchos widgets para crear cualquier tablero que se te ocurra. El sistema de etiquetado es excelente y el lenguaje de consulta es muy robusto. Datadog también viene con dashboards de Kubernetes preestablecidos. | Grafana y Datadog son muy similares en sus capacidades de visualización. Ambos tienen todas las funciones que necesitas para empezar y te permiten crear paneles que te ayudarán a controlar tu grupo de Kubernetes. Este es un empate.    
**Eventos de kubernetes** | Prometheus no expone automáticamente los eventos de Kubernetes, así que necesita una solución diferente para ello. Hay un proyecto en Github que exporta los eventos de Kubernetes como métricas de Prometheus. | El agente de Datadog recoge los eventos del clúster de Kubernetes y los pone en la sección "Eventos" de la aplicación. Esta opción se debe habilitar, y también hay que entender y configurar alertas de eventos para obtener valor de ellos. | Se puede configurar un sistema Prometheus para que exponga los eventos de Kubernetes, pero la solución no es óptima. Datadog ya que tiene un método oficialmente soportado para hacer esto. Sin embargo, la interfaz de usuario de Datadog para ver eventos no es muy amigable.  
**Alertas y Notificaciones** | Se necesita instalar AlartManager para obtener notificaciones de Prometheus. AlertManager tiene algunas funciones bastante buenas. **1.** Puede agrupar las alertas en un solo aviso o incluso silenciar alertas específicas durante un período de tiempo. **2.** Hay una función llamada inhibición que permite silenciar ciertas clases de alertas cuando otro tipo de evento está activo (considere el caso de que si la CPU está alta en un contenedor, puede que no le importe saber que la carga también lo está). | Aunque Datadog carece de algunas de las mejores reglas de notificación de AlertManager, como la inhibición, tiene un sistema increíblemente robusto de detección de estados de error. Datadog llama a estas alertas Monitores, y pueden hacer cualquier cosa, desde la detección de umbrales y anomalías hasta la comprobación de un evento para una cadena específica. | El servicio que se escoja dependerá de nuestras necesidades. Si se desea un control estricto sobre las alertas que se envían a su equipo, Prometheus y AlertManager son probablemente la mejor opción. Si se está buscando condiciones de alerta complicadas o si no nos gustan los archivos de configuración, probablemente será mejor Datadog.  
**Integración con otras herramientas** | StackStorm, Rancher, Thanos, M3. | Redis, TeamCity, Ruby, StatusPage.io, Lita, Ansible. |  Tienen sus pro y contras, la elección de alguna de estas herramientas depende de muchas cosas.  

## Bibiografía  

https://technologyconversations.com/2015/09/08/service-discovery-zookeeper-vs-etcd-vs-consul/  
https://www.levvel.io/blog/running-distributed-docker-swarm-on-aws  
https://www.learnitguide.net/2018/08/what-is-kubernetes-learn-kubernetes.html  
https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/  
https://www.nubersia.com/es/blog/kubernetes-vs-docker-swarm/  
https://www.bluematador.com/blog/how-to-monitor-your-kubernetes-cluster-prometheus-vs-datadog  
https://kubernetes.io/case-studies/ibm/  
https://kubernetes.io/case-studies/newyorktimes/  
https://cloud.google.com/blog/products/gcp/bringing-pokemon-go-to-life-on-google-cloud  
https://kubernetes.io/case-studies/  

