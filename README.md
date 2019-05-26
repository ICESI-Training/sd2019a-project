# Proyecto Final Sistemas Distribuidos
### Este documento contiene la resolución del proyecto final del curso de Sistemas Distribuidos, para el periodo 2019-1.
#### Realizado por:
**Nombre:** Santiago  
**Apellidos:** Fajardo Sima  
**Correo:** santiago_fajardo96@hotmail.com  
**Tema:** Orquestación de Contenedores  


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
* Realice un gráfico donde muestre la arquitectura de docker swarm y kubernetes. Tenga en cuenta
Incluya aspectos como las tecnologías empleadas para la comunicación de sus elementos, descubrimiento de servicio, entre otros.  
* Crear una tabla comparativa con al menos 5 diferencias entre Docker Swarm y Kubernetes
* Mencione las tecnologías de orquestación de contenedores nativas con kubernetes para al menos tres proveedores de servicios en la nube. Describa sus principales características y limitaciones
* Describa las implicaciones a nivel de operaciones de emplear una solución de kubernetes nativa con respecto a desplegar kubernetes manualmente a través de scripts, por ejemplo: KOPS. Tenga en cuenta
aspectos de costo y tolerancia a fallas.
* Crear una tabla comparativa con al menos 5 diferencias entre el monitoreo con Datadog y el monitoreo con Prometheus. Incluya aspectos como costo, obtención de métricas, alertas, entre otros.  


### Solución
**Docker swarm architecture**  
![](Capturas/swarm.png)
1. Docker Daemon: es un servicio que se ejecuta en su sistema operativo host . 

2. Swarm Join: un proceso que controla el registro de un único host con un Service Discovery Manager y la exposición del Docker Daemon del host como un servicio disponible.


3. Swarm Node: máquinas físicas o virtuales que ejecutan Docker Engine 1.12 o posterior en modo de enjambre.

4. Swarm Manager: el servicio que un usuario usa para administrar contenedores en los nodos de Swarm registrados. Este es el punto final para interactuar con un entorno de Swarm.

5. Gestores de descubrimiento de servicios:Estos administradores rastrean los servicios, miembros y sesiones registrados entre réplicas de ellos mismos. 

  
6. Docker Compose: esta es una herramienta para definir y ejecutar aplicaciones Docker de múltiples contenedores. Esta herramienta toma un archivo yml de configuración simple y despliega los contenedores como un manifiesto. Es la herramienta de implementación para utilizar las nuevas funciones de red de host múltiple que permiten a los nodos de Swarm tener aplicaciones que se ejecutan en hosts distribuidos. Compose también maneja estrategias de colocación para asegurar que sus contenedores se distribuyan de manera uniforme (o no) a través de los Nodos de Swarm. Esto permite la redundancia de contenedores en el nivel de host, lo que es bueno para la capacidad de recuperación de la producción.  

**Kubernetes architecture**

