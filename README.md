# Contador en flask con mysql

Aquí tenemos un ejemplo de contador en flask utilizando una base de datos mysql.

## Aplicación (descripción)

La app ofrece 2 endpoints:
- `/inicializa-contador`: para inicializarlo a 0
- `/`: para incrementar el contador y mostrar su valor.

## Base de datos

He tenido unos problemas a la hora de crear la tabla, asi que leyendo solucione sy documentacion, he utilizado adminer para crear la tabla directamente.

## Requisitos:

Python 3.9 o superior.
Biblioteca Flask 3.0.0 o superior.
Docker y Docker Compose (si se va a ejecutar con contenedores).

## Desplegando nuestra aplicacion en Docker

Dockerfile: Contiene script ara construir la imagen del contenedor de nuestra aplicacion.
    Hemos usado multistage para que la imagen sea mas pequeña
    Ejecutamos build -t seguido del nombre de nuestra imagen desde el directorio raiz.

    ![image](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/469861bb-24d4-410e-ad36-8e565399639b)

Docker Hub: Hemos subido la imagen a Docker Hub despues del paso anterior.

    Hemos usado el comando docker push con el nombre del usuario /nombre de la imagen:etiqueta.

![image](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/a4b1f8f7-010b-404b-a6bf-9635ae96ce20)

Docker Compose: Permite ejecutar nuestra aplicación localmente con persistencia de datos y comunicación entre contenedores.

  Hemos ejecutado el comando docker compose up.

  ![image](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/af966733-2fbc-48b0-8f18-aa35acd6f951)


Aqui nos hemos encontrado un pequeño bloqueo y es que no creaba el esquema de BBDD, asi que he utilizado Adminer para generarlo yo y asi no quedarme bloqueado.

  Hemos creado un manifest para el adminer.yaml y hemos ejecutado kubectl apply -f adminer.yaml y accedido al puerto para crearlo.

![adminer](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/43410e6d-a054-4d80-97fd-a5f8d7782003)

![adminer2](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/3903bad3-8e79-4265-98be-9f778a0799ec)

Logs: se pueden ver ejecutando docker logs <nombre_del_contenedor>.

![image](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/922dd536-ec70-41e7-8a17-98fa8e6968fa)

## Kubernetes

Como se puede ver en nuestro repsitorio hemos creado varios manifiestos para k8s:

- El app-congifmap.yaml: define un ConfigMap en Kubernetes. Los ConfigMaps se utilizan para almacenar datos de configuración no confidencial y parámetros utilizables por las aplicaciones en el cluster.
- El app-deployment.yaml: describe el estado deseado de tus instancias de aplicación. Kubernetes se asegura de que el estado actual de tu aplicación siempre coincida con el estado deseado.
- El app-ingress.yaml: administra el acceso externo a los servicios dentro de un cluster. Este manifiesto define las reglas de enrutamiento del tráfico que llega al cluster a los servicios correspondientes.
- El db-init.yaml: se ejecuta una vez para inicializar la base de datos.
- El db-secret.yaml: se utilizan para almacenar y gestionar información sensible, como contraseña. En este caso de la BBDD
- El db-service.yaml: es una abstracción que define un conjunto lógico de Pods y una política para acceder a ellos. Define cómo se puede acceder a la base de datos desde otros componentes del cluster.
- El service.yaml: este es otro manifiesto de Service, pero para la aplicación en lugar de la base de datos. Define cómo los Pods que ejecutan tu aplicación son accesibles dentro del cluster.
- El statefulset.yaml: usado para aplicaciones que requieren un estado persistente. Cada instancia tiene una identidad persistente y los Pods se recrean con el mismo nombre y estado.

![image](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/d759f66e-6d0c-43ca-af6c-7174141f854e)

Despliegue en Kubernetes:
  Lo hemos hecho a traves del comando kubectl apply -f .

![depliegue en kubernetes](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/5ad1706e-af27-4509-a1b5-25929a49a50b)

## Helm

Hemos creado unChart Helm haciendo los siguientes pasos:

  Es un paquete que contiene todos los archivos necesarios para definir un conjunto de recursos relacionados dentro de un clúster de Kubernetes.
  Hemos creado un directori chart/flas-app para la raiz y dentro otro directorio llamado templates van nuestros manifests.

![image](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/c6dea7b0-9da8-4c7e-be27-a96da31cea6a)

En este caso, desde charts, hacemos un helm package flask-app para empaquetar en ese directorio.

Despues hacemos una subida a nuestro repo de Github.

Ejecutamos el comando helm repo update para actualizarlo.

Y hacemos el install o el upgrade para ejecutarlo y lo probamos en el puerto:

helm upgrade flask flask-app/flask-app


![despliegue helm](https://github.com/jesusod/Contenedores_JesusOtero/assets/99189407/e71cba0c-482a-4b45-9c35-20f520cd856d)








