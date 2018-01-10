# Comandos Docker

## Imágenes

Ver las imágenes que se han descargado:

	docker images

<br>

Buscar imágenes de docker:
	
	docker search nombre_de_la_imagen

<br>

Descargar una imagen:
	
	docker pull nombre_de_la_imagen
***
<br>

## Inicio de contenedores.

Iniciar un contenedor:
	
	docker run nombre_de_la_imagen
(Se le asignará un ID al contenedor y un nombre, por defecto si la imagen no la tenemos, la descargará)

<br>

Iniciar un contenedor y dejarlo activo en background:
	
	docker run -d nombre_de_la_imagen

<br>

Iniciar un contenedor, dejarlo activo y darle un nombre:
	
	docker run -d --name "nombre" nombre_de_la_imagen

<br>

Inspeccionar un contenedor (configuración, Ip, etc):
	
	docker inspect nombre_del_contendor
***
<br>

## Logs

Ver el log de un contenedor:
	
	docker logs nombre_del_contenedor
***
<br>

## Mapear puertos

Iniciar un contenedor con un puerto específico, ejemplo sabiendo la imagen "redis" usa el puerto 6379, usarlo a través del puerto 6323
	
	docker run -d --name "nombre" -p 6323:6379 redis

<br>

Iniciar un contenedor con un puero aleatorio.
	
	docker run -d --name "nombre" -p 6379

<br>

Ver el puerto que tomó el contenedor:
	
	docker ps
o

	docker port "nombre" 6379
***
<br>

## Volúmenes

Al crear un contenedor la información por defecto se almacena dentro del mismo contenedor.
Por ende, si eliminamos el contenedor, la data guardada se eliminará y al tratar de levantar un nuevo contenedor, no tendrá la data anterior.
Para esto se puede mapear un volumen o directorio.
Por ejemplo, si en un contendor que guarda la información en el directorio "/opt/info", lo podemos mapear a un directorio local por ejemplo /var/container/info.
Con esto si eliminamos el contenedor, la data seguirá existiendo en el S.O en la ruta local /var/container/info y al crear un nuevo contenedor y mapearlo, seguirá ocupando la información de antes.

	docker run -d --name "nombre" -v "/var/container/info:/opt/info" nombre_del_contenedor

<br>

Los contenedores se ejecutan en background con la opción -d, si a un contenedor se le da un comando para ejecutar, ejecutará la acción y dejará de correr.

#### Ejemplo:

	docker run -d --name "nombre" ubuntu ps
	PID TTY          TIME CMD
    1 ?        00:00:00 ps
>En este ejemplo se creó un contenedor a partir de la imagen "ubuntu" y se le ordenó ejecutar el comando "ps", luego el contendor dejó de correr.

<br>

Para crear un contenedor e ingresar a él, se puede crear de forma interactiva con la opción "-i -t" o "-it". Ejemplo:
	
	docker run -it --name "nombre" ubuntu /bash
***
<br>

## Dockerfile

Dockerfile nos permite crear una imagen en base a una imagen base.
Dockerfile define varios pasos que se requieren para la creación de una imagen la cual será posteriormente usada para crear un contenedor.

#### Ejemplo dockerfile:

	FROM nginx:1.11-alpine
	COPY index.html /usr/share/nginx/html/index.html
	EXPOSE 80
	CMD ["nginx", "-g", "daemon off;"]

<br>

- **FROM:** Imagen base que se usará.
- **COPY:** Copia elementos "origen", "destino"... Si se trata de un archivo, se debe especificar el nombre del archivo destino.
- **EXPOSE:** Indica el puerto o puertos o rango de puertos que se expondrán en la imagen.
- **CMD:** Describe una secuencia de comandos que se ejecutarán al lanzar el contenedor que se cree con esta imagen.
- **RUN:** Pasos que se ejecutarán en la imagen antes de crearse.
- **WORKDIR:** Directorio en el que se ejecutarán los comandos.

<br>

Para crear la imagen se ejecuta el comando:

	docker build -t "nombre_image_nueva"
>La opción `-t` es para poder darle un nombre a la nueva imagen, a su vez, el nombre de la imagen se le puede agregar un "tag" para especificar su versión; mi_imagen:v1

<br>

Si se quiere agregar una variable al contenedor antes de ejecutarlo, se puede agregar la opción "-e" seguido de la variable y su contenido.

#### Ejemplo:

	docker run -d --name my-production-running-app -e NODE_ENV=production -p 3000:3000 my-image-nodejs-app
***
<br>

## Dockerignore

Para ignorar archivos o directorios que no se quieran incluir en la imagen a crear, se deben agregar al archivo `.dockerignore` (similar a .gitignore).

***
<br>

## Docker Network

Crear una red docker, una red sirve para generar un puente del cual varios otros contenedores se conectarán y obtendrán ip, además de resolver por el nombre con el que se crearon los contenedores.

#### Ejemplo:

Crear una red docker.
	
	docker network create backend-network

<br>

Ahora crearemos dos contenedores asociándolos a la red "backend-network" a partir de una imagen "redis".
	
	docker run -d --name=redis --net=backend-network redis
	docker run -d --name=test --net=backend-network redis

<br>

Ambos contenedores están corriendo en background y si creamos un nuevo contenedor que esté dentro del mismo puente y genere un ping a uno de los contenedores "redis" o "test" este deberá responder.

	docker run --net=backend-network alpine ping -c1 test
	PING test (172.19.0.3): 56 data bytes
	64 bytes from 172.19.0.3: seq=0 ttl=64 time=0.069 ms

	--- test ping statistics ---
	1 packets transmitted, 1 packets received, 0% packet loss
	round-trip min/avg/max = 0.069/0.069/0.069 ms

Esto sucede porque al crear un contenedor y asociarlo a un puente, se guardará en el resolv.conf de los contenedores un servidores DNS 127.0.0.11. Hacer un ping a "redis" es lo mismo que a "redis.backend-network".
Se pueden crear multiples redes e interconectar contenedores de distintas redes entre si.

#### Ejemplo:

Crearemos una red nueva:

	docker network create frontend-network

<br>

Ahora usaremos el comando "connect" para adjuntar un contenedor ya existente a la nueva red.

	docker network connect frontend-network redis

<br>

Si creamos un nuevo contenedor, web en este caso, y lo adjuntamos a la misma red que se podrá comunicar con la instancia "redis" que se creó anteriormente.

	docker run -d -p 3000:3000 --net=frontend-network katacoda/redis-node-docker-example

<br>

Consultamos con curl:
	
	curl docker:3000

<br>

Las redes se pueden listar y al igual que un contenedor se puede inspeccionar con los comandos:

	docker network list
	docker network inspect "nombre_de_la_red"

<br>

Con docker network se puede proporcionar un alias a los contenedores, para tener una entrada DNS extra.
Por ejemplo crearemos una nueva red y le conectaremos nuestro contenedor llamado "redis" con el alias "db".

	docker network create frontend-network2
	docker network connect --alias db frontend-network2 redis

<br>

Si inspeccionamos el contenedor redis veremos que aparte del id que le entrega docker, también tiene el alias "db" que le proporcionamos con docker network.

	docker inspect redis
	--> Salida
	"frontend-network2": {
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "db",
                        "b2e0316bee85"
                    ],

<br>

Ahora si creamos un contenedor alpine con la nueva red y hacemos un ping al alias responderá correctamente.

	$ docker run --net=frontend-network2 alpine ping -c1 db
	PING db (172.21.0.2): 56 data bytes
	64 bytes from 172.21.0.2: seq=0 ttl=64 time=0.084 ms

	--- db ping statistics ---
	1 packets transmitted, 1 packets received, 0% packet loss
	round-trip min/avg/max = 0.084/0.084/0.084 ms	

<br>

De la misma forma en que se conectan contenedores a una red, también se pueden desconectar.
El siguiente ejemplo muestra como desconectar el contenedor "redis" de la red fronend-network.

	docker network disconnect frontend-network redis
