# Inventario 

Como se indicó anteriormente para ejecutar tareas, ansible utiliza un inventario donde se define el listado de servidores. El archivo por defecto (texto plano) que usa ansible como inventario de servidores es `/etc/ansible/hosts`, este archivo puede ser modificado en `/etc/ansible/ansible.cfg` de forma global habilitando la opción `inventory`.<br>
El archivo puede contener la IP de los servidores o su host, dependiendo de la resolución de nombres.<br>

**Ejemplo de archivo hosts**

```
newhost.qa
10.100.101.11

[prod]
10.100.101.10
host01.prod
myhost.dev

```

Para utilizar otro archivo como inventario podemos llamarlo con el parámetro `-i` seguido de la ruta del archivo.

**Ejemplo**

```
$ ansible -i /tmp/inv_temp --list-hosts all
```

<br>

Otra forma de usar un archivo distinto del por defecto, es crear nuestro propio archivo `ansible.cfg` en el directorio actual y añadirle la opciones `[defaults]` y `inventory =` indicando la ruta del archivo con los servidores.

**Ejemplo**

```
$ mkdir /tmp/ansible; cd /tmp/ansible
$ echo -e "server1\nserver2" > inventario
$ echo -e "[defaults]\ninventory = ./inventario" > ansible.cfg
```
>En la primera línea se crea e ingresa al directorio `/tmp/ansible`.
>En la segunda línea se crea el archivo `inventario` con dos servidores.
>En la tercera línea se crea el archivo `ansible.cfg` y se le agrega la línea `inventory` con la ruta del archivo `inventario`.

<br>

De esta forma podremos utilizar el nuevo inventario sin la necesidad de indicarle la ruta en la que se encuentra.

```
$ ansible --list-hosts all
  hosts (2):
    server1
    server2
```
>La opción `all` se utiliza para listar todos los servidores del archivo de inventario.

<br>

## Listado de servidores

En los ejemplos anteriores para poder listar todos los servidores de un inventario se usó la opción `all`, esto es equivalente a usar `"*"`. De la misma manera se podrían listar grupos de servidores o servidores individuales y se podrían usar expresiones regulares para ampliar el método de búsqueda.


**Ejemplos**

Antes que todo ampliaremos nuestro inventario agregando los grupos *app*, *web* y *db*.

```
echo -e "\n[app]\nmy_app01\nmy_app02\n\n[web]\nmy_web01\nmy_web02\nmy_web03\n\n[db]\ndb01\ndb02" >> inventario
```
>**Nota:** Un host puede existir en más de un grupo.

<br>

Listar todos los servidores del inventario.
```
$ ansible --list-hosts all
  hosts (9):
    server1
    server2
    my_web01
    my_web02
    my_web03
    db01
    db02
    my_app01
    my_app02

```

<br>

Listar los servidores del grupo *web*.
```
$ ansible --list-hosts web
  hosts (3):
    my_web01
    my_web02
    my_web03
```

<br>

Listar los servidores *my_web02*, *db01* y *my_app01*.
```
$ ansible --list-hosts my_web02,db01,my_app01
o
$ ansible --list-hosts my_web02:db01:my_app01
o
$ ansible --list-hosts "my_web02 db01 my_app01"
  hosts (3):
    my_web02
    db01
    my_app01
```

<br>

Listar los servidores de los grupos *app* y *db*.
```
ansible --list-hosts app:db
  hosts (4):
    my_app01
    my_app02
    db01
    db02
```

<br>

Listar los servidores terminados en *01*.
```
$ ansible --list-hosts *01
  hosts (3):
    my_web01
    my_app01
    db01
```

<br>

Listar todos los servidores, excepto los del grupo *web*.
```
ansible --list-hosts \!web
  hosts (6):
    server1
    server2
    db01
    db02
    my_app01
    my_app02
```

Estos son algunos ejemplos con los que se puede filtrar una determinada cantidad de servidores para ejecutar tareas.

<br>

## Parámetros

En el archivo de inventario se pueden definir algunos parámetros para una mejor administración. Algunos parámetros son; el tipo de conexión, puerto, usuario de conexión, contraseña. En el siguiente link se muestra el listado de parámetros disponibles ![Ansible-parameters](http://docs.ansible.com/ansible/latest/intro_inventory.html#list-of-behavioral-inventory-parameters).