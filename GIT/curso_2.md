# Segunda Parte

Hagamos un repaso... ¿Qué aprendimos en la primera parte de este curso?

- Inicializar un repositorio.
- Los tres estados de git.
- Revisar los commits realizados.
- Ejemplos prácticos.
- Recuperar archivos.
- Eliminar y revertir commits.
- Inspeccionar commits.


Todo lo que vimos anteriormente se enfocaba en las principales funcionalidades de git orientado al uso local.
Lo que veremos ahora será el uso más potente de git, mediante repositorios externos y trabajo colaborativo.

<br>

## Repositorios externos

Otras de las cualidades de los controles de versiones como git es su uso externo, trabajar con repositorios que están en la nube (github, gitlab, bitbucket) o en un servidor remoto.
Para los siguientes ejemplos ustilizaremos :octocat: https://GitHub.com :octocat:

<br>

## ¿Qué es GitHub?

GitHub es un servicio en la nube que permite alojar nuestros proyectos usando el control de veriosnes git.
GitHub tiene dos versiones disponibles:

- Gratis: Los repositorios son públicos pero se permite seleccionar quien puede hacer commits.
- De pago: Los repositorios son privados e ilimitados, permite seleccionar quien puede ver y hacer commits.

>Para hacer un poco más interactivo el curso, solicito puedan crear una cuenta gratuita en [GitHub](https://www.github.com)

<br>

## Creando un repositorio remoto

Una vez que estamos registrados y logueados en GitHub, crearemos nuestro primer repositorio. Para esto tenemos que ir al botón <kbd>Start a project</kbd> y crearemos un repositorio gratuito, en este caso se llamará "test".<br>
Marcamos la casilla **Initialize this repository with a README** para crear un achivo readme que será el de presentación y vamos al botón <kbd>Create repository</kbd>.

<p align="center"><img src="https://raw.githubusercontent.com/coneking/trabajo/desarrollo/GIT/images/repo_test.png" width="600" /></p>

<br>

## Clonando un repositorio remoto

Clonar un repositorio basicamente es descagarlo desde la nube con toda su configuración, esto incluye el directorio ".git" que se crea al inicializarlo.<br>
Para clonar el repositorio existen dos opciones, una es clonarlo a través de su URL y la otra es clonarlo mediante llave SSH, para este caso usaremos la primera opción.<br>
Seleccionamos el botón <kbd>Clone or download</kbd>, copiamos la URL, ejemplo: https://github.com/coneking/test.git y en un directorio de nuestro equipo ejecutamos el comando `git clone "URL"`.

<p align="center"><img src="https://raw.githubusercontent.com/coneking/trabajo/desarrollo/GIT/images/clonar.png" width="400" /></p>

<br>

```sh
$ mkdir repo_web; cd repo_web
$ git clone https://github.com/coneking/test.git
	Cloning into 'test'...
	remote: Counting objects: 3, done.
	remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
	Unpacking objects: 100% (3/3), done.
``` 

>Al listar los directorios tendremos uno con el nombre del repositorio (test), dentro encontraremos la carpeta ".git" y el archivo README.md que se generaron al crear el repositorio en GitHub. Con esto ya podemos hacer nuestras modificaciones y commits.

<br>

## Agregando un repositorio remoto

Existe otra forma de trabajar con un repositorio remoto y es agregándolo (algo similar a clonar).<br>
Nuevamente copiamos la URL de nuestro repositorio y en un directorio nuevo ejecutamos `git init` y `git remote add origin URL`

```sh
$ mkdir repo_web; cd repo_web
$ git init
	Initialized empty Git repository in C:/Users/Git/Documents/repo_web/.git/
$ git remote add origin https://github.com/coneking/test.git
$ git pull origin master
	remote: Counting objects: 3, done.
	remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
	Unpacking objects: 100% (3/3), done.
	From https://github.com/coneking/test
	 * branch            master     -> FETCH_HEAD
	 * [new branch]      master     -> origin/master
```

>Ahora tenemos nuestro repositorio en el directorio creado y listo para hacer cambios.

<br>

## Push y Pull

Cuando tenemos nuestro repositorio agregado en nuestro equipo podemos hacer cambios a al proyecto y subirlos a GitHub mediante "push".<br>
**Push**: permite subir cambios al repositorio remoto una vez que creamos un commit, los cuales tamién son exportados.<br>
**Pull**: permite descargar cambios que se hayan generado en el repositorio remoto (commits, archivos, ramas) para mantener actualizado el repositorio local.<br>

**Ejemplo:**

Haremos un cambio en el achivo README.md y luego un commit.

```sh
$ echo -e "# Nuestro repositorio de prueba\n Este un repositorio de prueba y haremos nuestro primer **push**." > README.md 
$ git commit -am "Primer push"
	warning: LF will be replaced by CRLF in README.md.
	The file will have its original line endings in your working directory.
	[master ea849b9] Primer push
	 1 file changed, 2 insertions(+), 2 deletions(-)
```


Finalmente subiremos los cambios con push.

```sh
$ git push origin master
	Counting objects: 3, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (2/2), done.
	Writing objects: 100% (3/3), 312 bytes | 78.00 KiB/s, done.
	Total 3 (delta 0), reused 0 (delta 0)
	To https://github.com/coneking/test.git
	   3a6c6bf..ea849b9  master -> master
```
