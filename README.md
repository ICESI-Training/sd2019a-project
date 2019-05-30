## Miniproyecto Sistemas Distribuidos

**Universidad ICESI**  
**Curso:** Sistemas Distribuidos  
**Nombre:** Julián Niño  
**Código:** A00328080  
**Tema:** Orquestación de Contenedores  
**Correo:** juliannino01@hotmail.com

### Objetivos
* Realizar de forma autónoma el aprovisionamiento automático de infraestructura
* Diagnosticar y ejecutar de forma autónoma las acciones necesarias para lograr infraestructuras estables
* Integrar servicios ejecutándose en nodos distintos


### Actividades

### Primer Punto

* Realice un gráfico donde muestre la arquitectura de docker swarm y kubernetes. Tenga en cuenta
Incluya aspectos como las tecnologías empleadas para la comunicación de sus elementos, descubrimiento de servicio, entre otros

![](Imagenes/Kubernetes.png) 


**API server:** Es el que expone la API de kubernetes, se utiliza como punto de entrada de los comandos REST para controlar el cluster. Además, procesa valida y ejecuta todas las solicitudes REST.

**Scheduler:** Observa los pods recién creados que no registran un nodo asignado y selecciona un nodo para que estos se puedan  ejecutar. Tambien tiene las siguientes funciones:

       -Tiene en cuenta los recursos disponibles en cada nodo del clúster.
       -Toma en cuenta los recursos necesarios para que se ejecute un determinado servicio.

Lo anterior sirve para saber dónde desplegar cada pod dentro del clúster.

**Controller-manager:** Se encarga de ejecutar los controladores, un ejemplo de esto es el controlador de replicación encargado de mantener la cantidad correcta de pods en el clúster. 

**etcd:** Es un almacén de datos clave de valor simple, distribuido y consistente. Sus principales funciones son:

          -Almacenar la configuración compartida del clúster y para el descubrimiento de servicios
          -Notificar al resto de nodos del clúster los cambios de configuración

**kubelet:** Es el servicio dentro de cada nodo worker que sirve para comunicarse con el nodo master. Tambien  garantiza que los contenedores descritos en dicha configuración estén arriba y funcionando correctamente.

**Pods:** Un pod es grupo de uno o más contenedores y una especificación sobre cómo ejecutar los contenedores.

**kube-proxy:** Es un proxy de red y balnaceador de carga enrutando el trafico hacia el contenedor correcto, teniendo en cuenta la dirección IP y el numero de puerto. 



![](Imagenes/DockerSwarm.png) 


**Swarm Master:** Es el responsable del todo el cluster y gestiona los recursos de los hosts.

**Swarm Nodes:** Estos nodos deben ser accesibles por el nodo  master e integran un agente de nodo que registra el demonio del Docker referenciado, supervisa y actualizan el backend con el estado del nodo.

**Swarm Discovery:** Un servicio de descubrimiento que se basa en Docker Hub, utulizando un token para descubrir los nodos que forman parte de un cluster, tambien soporta servicios como consul y etcd.

-----------------------------------------------------------------------------------------------------------------------------------
### Segundo  Punto

* Crear una tabla comparativa con al menos 5 diferencias entre Docker Swarm y Kubernetes


![](Imagenes/TabladeComparación.png) 




----------------------------------------------------------------------------------------------------------------------------
### Tercer Punto

* Mencione las tecnologías de orquestación de contenedores nativas con kubernetes para al menos tres proveedores de servicios en la nube. Describa sus principales características y limitaciones

**IBM Cloud Kubernetes Service**

Esta ofrece herramientas eficientes, una experiencia de usuario intuitiva y una seguridad incorporada para la rápida entrega de aplicaciones que se pueden enlazar a servicios de nube relacionados con IBM Watson®, IoT, DevOps y análisis de datos. Entre las más importantes caracteristicas se encuentran: 

       - Gestión simplificada de clústeres: Proporciona una experiencia de GUI intuitiva para los usuarios de primera vez
       - La seguridad y el aislamiento se incorporan, no se anexan:  Almacene imágenes de Docker en un registro cifrado y privado, e incluya la firma de imágenes con Docker Notary y la aplicación de seguridad de imagen.
       - Servicios de nube y Watson en sus manos: Cree experiencias de cliente enriquecidas, con más de 170 servicios de IBM y de terceros para enriquecer sus aplicaciones, incluyendo datos cognitivos y meteorológicos.

Entre las limitaciones se encuentran: 

       - Los entornos de programación (lenguajes, librerías, etc.) están límitados por el proveedor
       - Al cobrarse por tiempo de ejecución, y en algunos proveedores limitarse, se penaliza un consumo prologando en el tiempo
       
 **Cloud Code**
 Ayuda al usuario cuando comienza a trabajar con un conjunto actualizado de muestras de Kubernetes preconfiguradas para la depuración, la creación y la implementación.
 
Entre las caracteristicas se encuentran: 

       -Cloud Code está diseñado específicamente para trabajar con Kubernetes, independientemente de su proveedor.
       -Cloud Code proporciona plantillas y resaltado de errores para los archivos de Kubernetes yaml
       -Cloud Code se integra fácilmente con las herramientas y servicios de DevOps existentes, incluidos Cloud Build y Stackdriver

Entre las limitaciones se encuentran: 

       -los entornos están límitados por el proveedor
       - Poca documentación 

**Kubernetes Engine**

Kubernetes Engine se lanzó en 2015, con base en los más de 12 años de experiencia de Google en la ejecución de servicios como Gmail y YouTube en contenedores. Permite la puesta en marcha de Kubernetes sin demoras, ya que elimina por completo la necesidad de instalar, administrar y operar tus propios clústeres.


Entre las caracteristicas se encuentran: 

       - Implementa una gran variedad de aplicaciones: Kubernetes Engine permite el desarrollo y la iteración de aplicaciones con más rapidez, ya que facilita implementar, actualizar y administrar aplicaciones y servicios. 
       - Operación fluida con alta disponibilidad: Controla tu entorno desde el panel integrado de Kubernetes Engine en la consola de Google Cloud
      
       - Escalabilidad sin dificultades para satisfacer la demanda: El ajuste de escala automático de Kubernetes Engine te permite administrar la demanda creciente de tus servicios por parte de los usuarios y los mantiene disponibles cuando más se necesitan.

Entre las limitaciones se encuentran:
       
       - Un máximo de 50 clústeres por zona, más 50 clústeres regionales por región
       - Un máximo de 5,000 nodos por clúster
       - Un máximo de 1,000 nodos por clúster si usas GKE Ingress Controller
       - 100 pods por nodo

--------------------------------------------------------------------------------------------------------------------------------
### Cuarto Punto

* Describa las implicaciones a nivel de operaciones de emplear una solución de kubernetes nativa con respecto a desplegar kubernetes manualmente a través de scripts, por ejemplo: KOPS. Tenga en cuenta
aspectos de costo y tolerancia a fallas.


Kops: sirve para crear y gestionar clusters de Kubernetes , en producción y con alta disponibilidaddesde línea de comandos. 

Si se hace por KOPS, se deben hacer los siguientes pasos: 

        -Crear una cuenta de usuario especifica
        -Crear un grupo en AWS llamado kops
        -Se corre el debido vagrant 
        -Se instala un Dashboard
        
  Mientras que Amazon EKS ejecuta la infraestructura de administración de Kubernetes por usted en varias zonas de disponibilidad de AWS a los fines de eliminar un único punto de error y sus mas importantes caracteristicas son:
  
       -Amazon EKS ejecuta la infraestructura de administración de Kubernetes en varias zonas de disponibilidad de AWS
       -Se configuran automáticamente canales de comunicación seguros y cifrados entre sus nodos de trabajo y los planos de control administrados
       -Amazon EKS ejecuta Kubernetes ascendente y cuenta con una certificación de conformidad con Kubernetes
  
  
 Para compararlos tenemos que: 
 
       - En KOPS La instrumentación inicial, por defecto, no agregará ningún RBAC o administración de usuarios. Mientras que en Amazon EKS es combinado estrechamente con Amazon IAM, y aprovechar el autenticador de código abierto de AWS para el control de acceso.
 
       -  En KOPS Totalmente administrado por usted mismo Puede ser una ventaja o desventaja para el uso futuro. Mientras que en EKS es gestionado por AWS
       
       - En KOPS se asegura que el clúster de etcd está en funcionamiento. Mientras que en EKS es gestionado por AWS
  
        
        


----------------------------------------------------------------------------------------------------------------
### Quinto Punto
* Crear una tabla comparativa con al menos 5 diferencias entre el monitoreo con Datadog y el monitoreo con Prometheus. Incluya aspectos como costo, obtención de métricas, alertas, entre otros


![](Imagenes/DatadogyPrometheus.png) 


### Referencias
* https://cloud.google.com/
* https://azure.microsoft.com/en-us/
* https://aws.amazon.com/
* https://aprenderdevops.com/arquitectura-de-kubernetes/
* https://www.adictosaltrabajo.com/2015/12/03/docker-compose-machine-y-swarm/
* https://creadoresdigitales.com/monitoreo-de-sistemas-con-prometheus/
* https://www.datadoghq.com/
* https://hackernoon.com/kubernetes-vs-docker-swarm-a-complete-comparison-guide-15ba3ac6f750
* https://cloud.google.com/kubernetes-engine/?hl=Es-419
* https://www.ibm.com/co-es/cloud/container-service
* https://www.adictosaltrabajo.com/2018/09/05/kubernetes-en-aws-con-kops/
* https://aws.amazon.com/es/eks/
* https://keplerworx.com/kops-vs-amazon-eks/
