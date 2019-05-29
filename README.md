# sd2019a-project
Proyecto Sistemas Distribuidos

**nombre:** edisson guerrero
**codigo:** A00328068
**carrera:** INGENIERÍA TELEMÁTICA


## 1) arquitectura de docker swarm y kubernetes

## 2) tabla comparativa entre Docker Swarm y Kubernetes

## 3) Tecnologías nativas en la nube con Kubernetes

**API declarativas :**  una API declarativa captura la intención del usuario, es decir, el estado deseado del sistema, sin preocuparse de cómo se espera que el sistema logre el estado deseado. En otras palabras, la interfaz de un sistema nativo en la nube debe proporcionar abstracciones que permitan a los usuarios especificar lo que quieren y cómo debe comportarse el sistema. En contraste, una interfaz imperativa permite a los usuarios especificar las instrucciones que sigue el sistema;

**Contenedores :** los contenedores han emergido rápidamente como la mejor manera de empaquetar, distribuir y operar componentes de software. Las aplicaciones nativas de la nube utilizan los contenedores (y Kubernetes como se describe a continuación) como un componente fundamental y como la unidad básica de creación y administración de componentes de aplicaciones.

**Microservicios :** una arquitectura de estilo de microservicios descompone un sistema en servicios independientes donde cada servicio es elástico, resistente, compostable, mínimo y completo. Los microservicios comparten una serie de atributos con los sistemas nativos de la nube, ya que las arquitecturas de estilo de los microservicios son el primer gran paradigma arquitectónico de software que surge en la era de la computación en la nube y DevOps.

**Mallas de servicio :** las arquitecturas de estilo microservicio eliminaron el middleware monolítico y la inteligencia centralizada de los intermediarios de servicio que se encuentran en los sistemas SOA al devolver la inteligencia a los servicios individuales. Sin embargo, varias funciones comunes para las comunicaciones de servicio a servicio, como administración, observabilidad y seguridad, ahora deben ser manejadas por un servicio. Una malla de servicios resuelve este problema y proporciona infraestructura distribuida para gestionar las comunicaciones entre servicios.

**Infraestructura inmutable :** el concepto básico de infraestructura inmutable es " reemplazar, no reparar ". La computación en nube permite este paradigma. Una excelente manera de entender esto es la descripción de Randy Bias de Pets vs Cattle. Lo importante para los sistemas nativos en la nube es separar las aplicaciones de la infraestructura, y tratar la infraestructura como algo inmutable es una excelente manera de lograrlo.

Kubernetes no solo habilita todas las tecnologías descritas anteriormente, sino que también actúa como el " plano de control " para los sistemas nativos de la nube. Todo se logra por medio de su infraestuctura en planos mencionados en el diagrama de la pregunta 1 : Plano de datos, Plano de control, Plano de administración

## 4) solución de kubernetes nativa vs scripts

Existen tres métodos populares para ejecutar Kubernetes en AWS: configurar manualmente todo en instancias de EC2, usar Kops para administrar su clúster o usar Amazon EKS para administrar su clúster. Administrar un clúster de Kubernetes en AWS sin ninguna herramienta es un proceso complicado que no se recomienda para la mayoría de los administradores, por lo que nos centraremos en el uso de EKS o Kops.

EKS Elastic Container Service para Kubernetes, se describe como un "Servicio de Kubernetes altamente disponible, escalable y seguro". Este tipo de descripción en el espacio de Kubernetes podría llevarlo a creer que puede tener una configuración de clúster de Kubernetes con un solo clic, y volvería a equivocarse. En su lugar, EKS administra completamente solo el plano de control de Kubernetes (nodos maestros, etcd, servidor api), por una tarifa de uso fija de $ 0.20 por hora o ~ $ 145 por mes. La desventaja de esto es que no tiene acceso a los nodos maestros y no puede realizar ninguna modificación en el plano de control.
 En cambio Kubernetes Operations (kops) es una herramienta CLI para “Instalación, actualizaciones y administración de K8 de grado de producción”. Kops ha existido desde finales de 2016, mucho antes de que existiera EKS.

Kops simplifica significativamente la configuración y administración del clúster Kubernetes en comparación con la configuración manual de los nodos maestro y de trabajo. Administra Route53, los grupos de AutoScaling, los ELB para el servidor de la API, los grupos de seguridad, el arranque maestro, el arranque del nodo y las actualizaciones continuas de su clúster. Dado que kops es una herramienta de código abierto, es de uso completamente gratuito, pero usted es responsable de pagar y mantener la infraestructura subyacente creada por kops para administrar su clúster Kubernetes.

hay diferentes aspectos que analizar por ejemplo:

**Configuración de clúster** Configurar un clúster con EKS es bastante complicado y tiene algunos requisitos previos, en cambio con Kops que es una herramienta CLI solo debe instalarse en su máquina local junto con kubectl para hacer que un clúster se ejecute tan simple como ejecutar el comando clúster kops create con todas las opciones necesarias.

**Gestión de clusters** La configuración del cluster es un evento raro. Al evaluar una solución que afectará a una parte crítica de su infraestructura como Kubernetes, también debe considerar cómo es escalar nodos, realizar actualizaciones de clústeres e integrarse con otros servicios en el futuro.

**Seguridad** La seguridad debe ser una de las principales preocupaciones de todos los administradores de Kubernetes. A medida que el ecosistema de Kubernetes madura, se encontrarán más vulnerabilidades. La velocidad a la que encontramos nuevos problemas de seguridad con Kubernetes está aumentando , y este problema no puede ser ignorado

es por ello que usar una solución nativa de kubernetes siempre sera una mejor opción.





### Referencias

* https://twitter.com/meinardi/status/707935835400884224
* https://thenewstack.io/kubernetes-steering-the-ship-with-cloud-native-management/
* https://kubernetes.io/docs/concepts/cluster-administration/networking/
* https://kubernetes.io/docs/concepts/architecture/cloud-controller/
* https://www.bluematador.com/blog/kubernetes-on-aws-eks-vs-kops


