Proyecto Sistemas Distribuidos

Jonathan Esteban Arias S.
A00315328

Pregunta 1.

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
