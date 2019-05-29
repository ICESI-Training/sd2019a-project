## Miniproyecto Sistemas Distribuidos

**Universidad ICESI**  
**Curso:** Sistemas Distribuidos  
**Nombre:** Julián Niño 
**Tema:**  Orquestación de Contenedores  
**Correo:** juliannino01@hotmail.com

### Objetivos
* Realizar de forma autónoma el aprovisionamiento automático de infraestructura
* Diagnosticar y ejecutar de forma autónoma las acciones necesarias para lograr infraestructuras estables
* Integrar servicios ejecutándose en nodos distintos


### Actividades
* Realice un gráfico donde muestre la arquitectura de docker swarm y kubernetes. Tenga en cuenta
Incluya aspectos como las tecnologías empleadas para la comunicación de sus elementos, descubrimiento de servicio, entre otros

![](Imagenes/Kubernetes.png) 


**API server:** Es el que expone la API de kubernetes, se utiliza como punto de entrada de los comandos REST para controlar el cluster. Además, procesa valida y ejecuta todas las solicitudes REST.

**Scheduler:** Observa los pods recién creados que no registran un nodo asignado y selecciona un nodo para que estos se puedan  ejecutar. Tambien tiene las siguientes funciones:
          - Tiene en cuenta los recursos disponibles en cada nodo del clúster.
          - Toma en cuenta los recursos necesarios para que se ejecute un determinado servicio.

Lo anterior sirve para saber dónde desplegar cada pod dentro del clúster.






![](Imagenes/DockerSwarm.png) 


* Crear una tabla comparativa con al menos 5 diferencias entre Docker Swarm y Kubernetes
* Mencione las tecnologías de orquestación de contenedores nativas con kubernetes para al menos tres proveedores de servicios en la nube. Describa sus principales características y limitaciones
* Describa las implicaciones a nivel de operaciones de emplear una solución de kubernetes nativa con respecto a desplegar kubernetes manualmente a través de scripts, por ejemplo: KOPS. Tenga en cuenta
aspectos de costo y tolerancia a fallas.
* Crear una tabla comparativa con al menos 5 diferencias entre el monitoreo con Datadog y el monitoreo con Prometheus. Incluya aspectos como costo, obtención de métricas, alertas, entre otros

### Referencias
* https://cloud.google.com/
* https://azure.microsoft.com/en-us/
* https://aws.amazon.com/
* https://aprenderdevops.com/arquitectura-de-kubernetes/
* https://www.adictosaltrabajo.com/2015/12/03/docker-compose-machine-y-swarm/
