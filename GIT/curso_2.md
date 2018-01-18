# Segunda Parte

Hagamos un repaso... ¿Qué aprendimos en la primera parte de este curso?

## Contenido

<details>
<summary>Expandir</summary>

- Inicializar un repositorio.
- Los tres estados de git.
- Revisar los commits realizados.
- Ejemplos prácticos.
- Recuperar archivos.
- Eliminar y revertir commits.
- Inspeccionar commits

</details>

<br>

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

**Ejemplo Push:**

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

<br>

**Ejemplo Pull:**

Haremos un cambio al archivo README.md desde la web de GitHub y luego en nuestro repositorio local lo descargaremos mediante pull.

```sh
$ git pull origin master
	remote: Counting objects: 3, done.
	remote: Compressing objects: 100% (2/2), done.
	remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
	Unpacking objects: 100% (3/3), done.
	From https://github.com/coneking/test
	* branch            master     -> FETCH_HEAD
	ea849b9..2e35a24  master     -> origin/master
	Updating ea849b9..2e35a24
	Fast-forward
	README.md | 9 +++++++--
	1 file changed, 7 insertions(+), 2 deletions(-)
```

>**Nota**: Es recomendable hacer un pull del repositorio antes de enviar nuestros cambios, esto debido a que, si el repositorio externo está más actualizado que el nuestro (debido a cambios de otras personas) no dejará que se hagan "push".

<br>

## Ramas

El concepto de ramificación puede ser uno de los puntos más fuertes de git, y es que se basa en el trabajo colaborativo paralelo de un proyecto. Para explicarlo de una manera más sencilla diremos que tenemos una línea de tiempo con commits hechos por la rama ***master*** (rama que se crea por defecto).

<p align="center"><img src="https://raw.githubusercontent.com/coneking/trabajo/desarrollo/GIT/images/rama1.png" width="500" /></p>

Ahora crearemos una rama con el comando `git branch "nombre_de_la_rama"` y luego nos cambiaremos a esa rama con el comando `git checkout "nombre_de_la_imagen"` para trabajar en paralelo a la rama master.

```sh
$ git branch testing
```

> Para ver las ramas locales se ejecuta el comando `git branch` donde un "*" indicará en cual rama estamos actualmente.<br>
> Para listar todas las ramas (locales y remotas) ejecutamos el mando `git branch -a`.

<br>

Ahora tenemos dos ramas ***master*** y ***testing***, ambas apuntando al mismo commit *f30ab*. 

<p align="center"><img src="https://raw.githubusercontent.com/coneking/trabajo/desarrollo/GIT/images/rama2.png" width="500" /></p>

<br>

Nos cambiaremos a la rama "testing" con el comando `git checkout "nombre_de_la_rama"`.

```sh
$ git checkout testing
	Switched to branch 'testing'
```

Al cambiarnos a nuestra nueva rama, el **HEAD** se irá también a esa rama y podremos crear un nuevo commit para trabajar en paralelo a "master". 

<p align="center"><img src="https://raw.githubusercontent.com/coneking/trabajo/desarrollo/GIT/images/rama3.png" width="500" /></p>
>HEAD se mueve al cambiarse entre ramas, en este caso ambas ramas apuntan al mismo commit.

<br>

<p align="center"><img src="https://raw.githubusercontent.com/coneking/trabajo/desarrollo/GIT/images/rama4.png" width="500" /></p>

<br>

En este ejemplo la nueva rama creó el commit *c2b9e*. Los cambios agregados en este commit no los puede ver la rama "master" ya que, si nos cambiamos a la rama master, veremos que el HEAD sigue estando en el commit *f30ab*.<br>
Continuando con el ejemplo nos devolveremos a la rama "master" y al hacer un commit la línea de tiempo se bifurcará.

```sh
$ git checkout master
	Switched to branch 'master'
	Your branch is up to date with 'origin/master'.
```

<br>

## Conflictos

Al trabajar con ramas, se nos puede presentar el caso de tener *conflictos*.
Esto puede deberse a que el archivo que queremos subir, se encuentra desactualizado respecto al que existe en el repositorio.

Si tratamos de realizar un push, aparece un mensaje similar al siguiente:

```sh
Updates were rejected because the remote contains work that you do
not have locally. This is usually caused by another repository pushing to the same ref. You may want to first integrate the remote changes
```

En este caso, git avisa del conflicto en el o los archivos y propondrá una manera de solucionarlo.

Podemos ejecutar un *git pull* para traer los cambios mas actualizados. Si el sistema no es capaz de realizar el merge o fusion, de forma automatica, nos creara un escenario para que nosotros decidamos que cambio queda como definitivo.

```sh
git pull origin master
...
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Si se esta usando el cliente de GIT para Windows, se señalara de forma grafica, que pasamos a una rama temporal para resolver el conflicto. Se nos añadira la etiqueta *MERGING* al nombre de la rama que estemos trabajando.

El archivo con conflicto aparecera con etiquetas, donde indicaran los cambios actuales con la etiqueta *HEAD*.

```sh
# Ejemplo
<<<<<<< HEAD
Cambio dia 01 enero
=======
Cambios dia 02 enero
>>>>>>> 57894ff9df56e924521009fb95b17655c4598fe5
```
Aca podemos editar y dejar los cambios que correspondan. Tambien deberemos eliminar las lineas agregaras para señalar los cambios. Luego de guardar los cambios, ya podemos ejecutar *git commit* y *git push* para sincronizar los cambios.
