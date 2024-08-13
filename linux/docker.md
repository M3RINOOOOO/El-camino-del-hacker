# Docker

> Docker es una herramienta de software que te permite empaquetar aplicaciones en un formato que sea fácilmente desplegado en cualquier máquina. Se crea un contenedor (como una caja) donde se instalan todas las dependencias necesarias para que la aplicación funcione correctamente. Algunas de las ventajas que aporta **docker** son el aislamiento que ofrece, la portabilidad y la reproducibilidad.

## Comandos Básicos de Docker

### Listado de Imágenes y Contenedores

* `docker images`: Lista las imágenes existentes en la máquina.
* `docker pull ubuntu`: Descarga la imagen de Ubuntu desde la base de datos de Docker.
* `docker ps -a`: Lista todos los contenedores que se están ejecutando y sus identificadores (`-q` para mostrar solo los identificadores).
* `docker logs contenedor`: Muestra un registro asociado al contenedor (`-f` para seguir el registro de forma interactiva).
* `docker inspect`: Muestra información detallada sobre un contenedor.
* `docker volume ls`: Lista los volúmenes en el sistema.

### Creación de Imágenes

Para crear una imagen de Docker, se necesita un archivo `Dockerfile` que defina la configuración de la imagen. Ejecuta el siguiente comando:

* `docker build -t nombre:v1 .`: Crea una imagen con el Dockerfile existente en el mismo directorio con el nombre `nombre` y etiqueta `v1`. (Es necesario hacer esto por cada cambio que se realice sobre el Dockerfile).

Un ejemplo de Dockerfile puede ser el siguiente:

```Dockerfile
#Para especificar la imagen base desde cual se creará la nueva.  ubuntu (última versión), si no se pone nada se sobreentiende que es la latest
FROM ubuntu:latest       

#Quien es el autor del docker y lo va a mantener
MAINTAINER Pepito Pamplona Perales "ppp@gmail.com"

#Para ajustar variables de entorno
ENV DEBIAN_FRONTEND noninteractive

#Se utiliza para ejecutar comandos en el interior del contenedor, como instalación de paquetes o configuación del entorno
RUN apt update && apt install -y net-tools \
  iputils-ping \
  curl \
  git \
  nano \
  apache2 \
  php

#Se utiliza para copiar archivos desde el sistema host al interior del contenedor
COPY archivo.txt /un/directorio/del/contenedor

#Se utiliza para especificar el comando que se ejecutará cuando se arranque el contenedor (en el ejemplo comienza el servicio web apache y necesariamente devolvemos una bash)
ENTRYPOINT service apache2 start && /bin/bash
#CMD

#Se utiliza para abrir un puerto
EXPOSE 80
```

### Creación de Contenedores

* `docker run -dit --name nombre imagen`: Crea un contenedor con el nombre `nombre` y utilizando la imagen con el nombre `imagen` previamente creada. Devuelve el identificador del contenedor creado.
* `docker exec -it nombre comando`: Ejecuta un comando sobre el contenedor `nombre`. Es útil ejecutar un `bash` para obtener una terminal interactiva.

#### Parámetros Importantes

* `-d`: Inicia el contenedor en segundo plano.
* `-i`: Permite la entrada interactiva al contenedor.
* `-t`: Asigna una seudoterminal al contenedor.

### Gestión de Contenedores

* `docker stop nombre`: Termina la ejecución del contenedor `nombre`.
* `docker rm nombre --force`: Termina la ejecución del contenedor y elimina su registro (el contenedor no podrá ser reiniciado).
* `docker rm $(docker ps -a -q)`: Elimina los registros de todos los contenedores abiertos en el sistema.

### Gestión de Imágenes

* `docker rmi nombre`: Elimina la imagen `nombre` del sistema.
* `docker rmi $(docker images -q)`: Elimina todas las imágenes del sistema.
* `docker rmi $(docker images -q "dangling=true")`: Elimina todas las imágenes con repositorio y etiqueta `<none>`.

### Gestión de Volúmenes

* `docker volume rm volumen`: Elimina el volumen del sistema.
* `docker volume rm $(docker volume ls -q)`: Elimina todos los volúmenes del sistema.

### Gestión de Redes

Para crear una nueva red en Docker, podemos utilizar el siguiente comando:

* `docker network create --subnet=<subnet> <nombre_de_red>`

#### Parámetros Importantes

* **subnet**: Es la dirección IP de la subred de la red que estamos creando. Esta dirección IP debe ser única y no debe entrar en conflicto con otras redes o subredes existentes en nuestro sistema.
* **nombre\_de\_red**: Es el nombre que le damos a la red que estamos creando.

Además de los campos mencionados anteriormente, también podemos utilizar la opción `–driver` en el comando `docker network create` para especificar el controlador de red que deseamos utilizar.

Por ejemplo, si queremos crear una red de tipo “bridge“, podemos utilizar el siguiente comando:

* `docker network create --subnet=<subnet> --driver=bridge <nombre_de_red>`

En este caso, estamos utilizando la opción `–driver=bridge` para indicar que deseamos crear una red de tipo “bridge“. La opción `–driver` nos permite especificar el controlador de red que deseamos utilizar, que puede ser “bridge“, “overlay“, “macvlan“, “ipvlan“ u otro controlador compatible con Docker.

### Port Forwarding

Nos permite redirigir el tráfico de red desde un puerto específico en el host a un puerto específico en el contenedor. Para utilizar port forwarding, se debe hacer uso del parámetro `-p` en el comando `docker run`:

* `docker run -p 80:8080 imagen`: Redirige el puerto 80 del host al puerto 8080 del contenedor.
* `docker run -p 53:53/udp imagen`: Redirige el puerto 53 del host al puerto 53 del contenedor utilizando el protocolo UDP.
* `docker port contenedor`: Muestra los port forwarding que se están realizando en la máquina.

### Monturas

Nos permite compartir un directorio o archivo entre el sistema host y el contenedor. Para utilizar las monturas, se debe hacer uso del parámetro `-v` en el comando `docker run`:

* `docker run -v /un/directorio:/directorio imagen`: Monta el directorio `/un/directorio` del host en el directorio `/directorio` del contenedor.
* `docker run -v /un/directorio:/directorio:ro imagen`: Monta el directorio `/un/directorio` del host en el directorio `/directorio` del contenedor en modo de solo lectura.

### Docker Compose

Docker Compose es una herramienta para gestionar aplicaciones de múltiples contenedores Docker con un archivo de configuración YAML. Facilita el despliegue y escalado de aplicaciones complejas.

* `docker-compose up -d`: Levanta el contenedor estando en el directorio donde se encuentra el `docker-compose.yaml`.

### Limpieza de todo

Para limpiar todas las imágenes, contenedores, redes y volúmenes, podemos crear un comando e introducirlo en la `zsh` que contenga lo siguiente:

```bash
cleanDocker(){
	docker rm $(docker ps -a -q) --force 2> /dev/null
	docker rmi $(docker images -q) 2> /dev/null
	docker network rm $(docker network ls -q) 2> /dev/null
	docker volume rm $(docker volume ls -q) 2> /dev/null
}
```
