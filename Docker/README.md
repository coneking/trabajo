# Docker

## ¿Qué es Docker?

Docker es un proyecto Opensource que permite desplegar aplicaciones en contenedores. Gestiona la creación y administración de contenedores.

<p align="center"><img src="https://raw.githubusercontent.com/coneking/docker/desarrollo/images/docker.png" width="500" /></p>

<br>

## ¿Qué son los contenedores?

Los contenedores son una tecnología de virtualización que se basa en la ejecución de instancias de sistemas operativos desde un servidor (host) físico.
Los contenedores utilizan las librerías, binarios, dependencias y recursos del Host local, usan una imagen base para su creación.

<p align="center"><img src="https://raw.githubusercontent.com/coneking/docker/master/images/container.png" width="500" /></p>

<br>

>**Nota**: Las imagenes de los contenedores están disponibles oficialmente en https://hub.docker.com/

<br>

## Contenedores VS Máquinas virtuales.

Los contenedores a diferencia de las máquinas virtuales corren bajo un sólo kernel, el del host donde se están ejecutando, así como sus binarios y librerías.
Por otra parte, las máquinas virtuales necesitan la instalación de un Sistema Operativo por cada una de ellas, además de algún Hypervisor que soporte multiples kernel para cada máquina virtual.

<p align="center"><img src="https://raw.githubusercontent.com/coneking/docker/desarrollo/images/vm-vs-container.png" width="500" /></p>

<br>

## Componentes de Docker

* [Docker Daemon](#docker-daemon)
* [Docker Client](#docker-client)
* [Docker Image](#docker-image)
* [Docker Hub](#docker-hub)
* [Dockerfile](#dockerfile)
* [Docker Container](#docker-container)
* [Docker Registry](#docker-registry)
* [Docker Swarm](#docker-swarm)


<br>

### Docker Daemon

Es el principal proceso de Docker. Se ejecuta en el host principal y el usuario no tiene interacción directa con el demonio, sino a través del cliente.


### Docker Client

Acepta la ejecución de comandos provenientes del usuario y los comunica con el demonio de Docker mediante la interface de línea de comandos.


### Docker Image

Las imágenes son templates que contienen toda la data que se requiere para hacer correr un contenedor.
Las imágenes están disponibles en https://hub.docker.com/, a su vez, se pueden crear imágenes propias y publicarlas en la misma página o utilizarlas localmente.


### Docker Hub

Es la plataforma oficial, la cual ofrece un repositorio centralizado para compartir y administrar imágenes de Docker.


### Dockerfile

Es un archivo que contiene toda la información para crear una imagen. El archivo es de texto plano y sigue una secuencia de comandos declarados comenzando por la sentencia "FROM".


### Docker Container

Contiene todo lo necesario para que una aplicación pueda ejecutarse. Los contenedores se crean a partir de una imagen de Docker.


### Docker Registry

Sirve para almacenar las imágenes de los contenedores que hemos creado, ya sea, de forma pública o privada.


### Docker Swarm

Se utiliza para implementar un contenedor Docker en un entorno clusterizado.

***

<br>

**Comandos Docker** [<kbd>&#10154;</kbd>](/Comandos.md)