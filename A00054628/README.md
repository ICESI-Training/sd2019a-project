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

**Características** | Kubernetes | Docker Swarm
--------------- | ---------- | ------------
**Definición de la aplicación** | Las aplicaciones se pueden implementar usando una combinación de pods, implementaciones y servicios (o "microservicios"). | Las aplicaciones se pueden implementar como servicios (o "microservicios") en un clúster Swarm. Las aplicaciones de varios contenedores pueden especificarse utilizando archivos YAML.  
**Construcciones de escalabilidad de aplicaciones** | Cada nivel de aplicación se define como un pod y se puede escalar cuando se administra mediante una implementación, que se especifica en YAML, la escala puede ser manual o automatizada. | Los servicios se pueden escalar utilizando plantillas Docker Compose YAML.  
**Alta disponibilidad** | Las implementaciones permiten que los pods se distribuyan entre los nodos para proporcionar Alta Disponibilidad, tolerando así las fallas de la aplicación. | Los servicios se pueden replicar entre los nodos de Swarm. Los administradores de Swarm son responsables de todo el clúster y administran los recursos de los nodos de trabajadores.  
**Balanceo de carga** | Los pods se exponen a través de un servicio, que se puede usar como un equilibrador de carga dentro del clúster.  | El modo Swarm tiene un componente DNS que se puede usar para distribuir solicitudes entrantes a un nombre de servicio. Los servicios pueden ejecutarse en los puertos especificados por el usuario o pueden asignarse automáticamente.  
**Descubrimiento del servicio** | Los servicios se pueden encontrar utilizando variables de entorno o DNS. Kubelet agrega un conjunto de variables de entorno cuando se ejecuta un pod. | El nodo de Swarm Manager asigna a cada servicio un nombre DNS único y equilibrios de carga que ejecutan contenedores. Las solicitudes a los servicios se equilibran de carga a los contenedores individuales a través del servidor DNS integrado en el Enjambre.  

### 3 Mencione las tecnologías de orquestación de contenedores nativas con kubernetes para al menos tres proveedores de servicios en la nube. Describa sus principales características y limitaciones.  

1. **Pokemon GO:**  

![SI_SmartDevice_PokemonGo_image1600w](https://user-images.githubusercontent.com/23371077/58519647-28ae1000-817a-11e9-86a9-31166067bee9.jpg)  

Pokemon Go es una apliación móvil que usa muchos servicios de Google cloud. Más allá de ser un fenómeno global, Pokémon GO es uno de los ejemplos más interesantes de desarrollo basado en contenedores "in the wild". La lógica de aplicación del juego se ejecuta en Google Container Engine (GKE), impulsado por el proyecto Kubernetes de código abierto. Niantic eligió a GKE por su capacidad de orquestar su grupo de contenedores a escala planetaria, liberando a su equipo para que se centre en implementar cambios en directo para sus jugadores. De esta forma, Niantic utilizó Google Cloud para convertir Pokémon GO en un servicio para millones de jugadores, adaptándose y mejorando continuamente.  

Una de las hazañas técnicas más sorprendentes realizadas por Niantic y el equipo de CRE de Google fue actualizar a una nueva versión de GKE que permitiría añadir más de mil nodos adicionales a su clúster de contenedores, en preparación para el tan esperado lanzamiento en Japón. Se tomaron medidas para no interrumpir a los jugadores existentes, pasando a la nueva versión mientras millones de nuevos jugadores se inscribían y se unían al mundo de los juegos Pokémon. Además de esta actualización, los ingenieros de Niantic y Google trabajaron en conjunto para reemplazar el Balanceador de Carga de Red, desplegando el nuevo y más sofisticado Balanceador de Carga HTTP/S en su lugar. El Load Balancer HTTP/S es un sistema global diseñado para el tráfico HTTPS, que ofrece mucho más control, conexiones más rápidas a los usuarios y un mayor rendimiento en general, lo que se adapta mejor a la cantidad y al tipo de tráfico que veía Pokémon GO.

2. **The New York Times:**  

![20190205120119 0](https://user-images.githubusercontent.com/23371077/58520045-a7577d00-817b-11e9-8533-2ba668b7e55f.jpg)  

***Problema**: Cuando la empresa decidió hace unos años abandonar sus centros de datos, sus primeras implementaciones en la nube pública fueron aplicaciones más pequeñas y menos críticas gestionadas en máquinas virtuales. "Comenzamos a construir más y más herramientas, y en algún momento nos dimos cuenta de que estábamos haciendo un mal servicio al tratar a Amazon como otro centro de datos", dice Deep Kapadia, Director Ejecutivo de Ingeniería del The New York Times. Kapadia fue contratada para liderar un equipo de ingeniería de entrega que se encargaría de "diseñar las abstracciones que nos ofrecen los proveedores de cloud computing".*  

***Solución:*** El equipo decidió utilizar Google Cloud Platform y su oferta de Kubernetes-as-a-service, GKE.  

**Impacto:** La velocidad de entrega aumentó al implementar Kubernetes. Las implementaciones que realizaban con máquinas virtuales tardaban aproximadamente 45 minutos para desplegar el servicio, con Kubernetes, ese tiempo fue "de unos pocos segundos a un par de minutos" (Gerente de Ingeniería Brian Balser). Además, los equipos que solían desplegar en horarios semanales o tenían que coordinar horarios con el equipo de infraestructura, ahora, cada equipo despliega sus actualizaciones de forma independiente, y pueden hacerlo diariamente cuando es necesario. La adopción de tecnologías Cloud Native Computing Foundation permite un enfoque más unificado para la implementación en todo el personal de ingeniería y la portabilidad para la empresa.

3. **IBM:**  

![1200px-IBM_logo svg](https://user-images.githubusercontent.com/23371077/58520279-9824ff00-817c-11e9-8e5b-0e2f8381fdd9.png)  

IBM Cloud ofrece funcionalidad de cloud pública, privada e híbrida a través de un diverso conjunto de tiempos de ejecución, desde su función basada en OpenWhisk como servicio (FaaS), Kubernetes y contenedores gestionados, hasta la plataforma Cloud Foundry como servicio (PaaS). Estos tiempos de ejecución se combinan con la potencia de las tecnologías empresariales de la empresa, como MQ y DB2, su moderna inteligencia artificial (IA) Watson y los servicios de análisis de datos. Los usuarios de IBM Cloud pueden explotar las capacidades de más de 170 servicios nativos de cloud computing diferentes de su catálogo, incluyendo capacidades como la API y los servicios de datos de Weather Company de IBM. A finales de 2017, el equipo de IBM Cloud Container Registry quería crear un servicio de confianza en la imagen.  

El trabajo en este nuevo servicio culminó con su disponibilidad pública en la nube de IBM en febrero de 2018. El servicio de confianza en la imagen, llamado Portieris, se basa completamente en el proyecto de código abierto de la Cloud Native Computing Foundation (CNCF), según Michael Hough, desarrollador de software del equipo de IBM Cloud Container Registry. Portieris es un controlador de admisión de Kubernetes para hacer cumplir la confianza en el contenido. Los usuarios pueden crear políticas de seguridad de imagen para cada espacio de nombres de Kubernetes, o a nivel de clúster, y aplicar diferentes niveles de confianza para diferentes imágenes. Portieris es una parte clave de la historia de confianza de IBM, ya que permite a los usuarios consumir la oferta notarial de la empresa desde sus clusters IKS. La oferta es que el servidor Notary se ejecuta en la nube de IBM, y Portieris se ejecuta dentro del clúster IKS. Esto permite a los usuarios hacer que su cluster IKS verifique que la imagen desde la que están cargando contenedores contiene exactamente lo que esperan que contenga, y Portieris es lo que permite a un cluster IKS aplicar esa verificación.  

La intención de IBM al ofrecer un servicio gestionado de contenedores y registro de imágenes de Kubernetes es proporcionar una plataforma de extremo a extremo totalmente segura para sus clientes empresariales. "La firma de imágenes es una parte clave de esa oferta, y nuestro equipo de registro de contenedores vio a Notary como la forma de facto de implementar esa capacidad en el ecosistema actual de muelles y contenedores", dice Hough. La compañía no había ofrecido antes la firma de imágenes, y Notary es la herramienta que utilizó para implementar esa capacidad. "Teníamos un Registro Docker multi-inquilino con alojamiento privado de imágenes", dice Hough. "El Registro Docker utiliza hashes para asegurar que el contenido de la imagen es correcto, y los datos están encriptados tanto en vuelo como en reposo. Pero no ofrece ninguna garantía de quién empujó una imagen. Usamos Notario para permitir a los usuarios firmar imágenes en sus espacios de nombres de registro privados si así lo desean".  

### 4 Describa las implicaciones a nivel de operaciones de emplear una solución de kubernetes nativa con respecto a desplegar kubernetes manualmente a través de scripts, por ejemplo: KOPS. Tenga en cuenta aspectos de costo y tolerancia a fallas.  
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
