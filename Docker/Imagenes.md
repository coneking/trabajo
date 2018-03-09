# Imagen Docker

Una imagen es un conjunto de dependencias que se necesitan para correr una aplicación (binarios y librerías).
No se necesita kernel, ya que, se utiliza el del propio host.
La imagen contiene la información de cómo utilizar estas dependencias para que se puedan ejecutar como un contenedor.


## Repositorio de Imágenes

Las imágenes son almacenadas oficialmente en <a href="https://hub.docker.com" target="_blank">hub.docker.com</a>, donde existen tanto imágenes públicas como privadas. El registro en DockerHub es gratuito y es posible que una persona pueda crear su propia imagen mediante [Dockerfiles](./Dockerfile.md) y subirla para contribuir con la comunidad. Aquí más información sobre [Docker Hub](/DockerHub.md).

### Layers

La composición de una imagen docker está dado por *layers*. Layers son acciones descritas en un Dockerfile como pueden ser por ejemplo `FROM`, `COPY`, `RUN`, etc.
Ejemplo Dockerfile:

```
FROM alpine:latest

ENV SALUDO=Hola
RUN mkdir -p /opt/test
COPY ejemplo.sh /opt/test

CMD ["/opt/test/ejemplo.sh"]
```

En este ejemplo se crea un contenedor basado en 5 comandos (FROM, ENV, RUN, COPY y CMD), cada una de estas instrucciones es un *layer* y cada uno es dependiente del anterior, en este caso la base es la imagen `alpine` la cual ya posee su propio layer.

<br>

Para revisar los layers de una imagen se usa el comando `docker image history nombre_de_la_imagen`.

```
$ docker image history test
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
60498be00f2e        4 minutes ago       /bin/sh -c #(nop)  CMD ["/opt/test/ejemplo.s…   0B                  
ec8614e09d5e        4 minutes ago       /bin/sh -c #(nop) COPY file:dfb6a78f449a3737…   32B                 
fe5a9e4c91d1        4 minutes ago       /bin/sh -c mkdir -p /opt/test                   0B                  
a916517f8e9e        4 minutes ago       /bin/sh -c #(nop)  ENV SALUDO=Hola              0B                  
3fd9065eaf02        5 weeks ago         /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B                  
<missing>           5 weeks ago         /bin/sh -c #(nop) ADD file:093f0723fa46f6cdb…   4.14MB    
```
>**Nota**: En versiones de docker anteriores el comando a usar es `docker history nombre_de_la_imagen`.