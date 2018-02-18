<p align="center"><img src="https://raw.githubusercontent.com/coneking/trabajo/desarrollo/Ansible/images/ansible-logo.png" width="500" /></p>

<br>

# ¿Qué es Ansible?

Ansible es una herramienta que permite el despliegue automatizado de tareas. Es una aplicación open-source desarrolada en Python y se basa en la ejecución de módulos y comandos arbitrarios a nodos a través del protocolo SSH.<br>

Ansible tiene un diseño minimalista y a diferecia de otros programas como Puppet o Chef, no necesita la instalación de agentes en los nodos, sólo hace falta que estos tengan python 2.6, 2.7 o 3.5 o superior.<br>


## Método de instalación.

Como requisito Ansible necesita python 2.4 o posterior para su instalación.<br>

Instalación con Python:

```python
$ pip install ansible
```


Instalación con git:

```git
$ git clone https://github.com/ansible/ansible.git

$ cd ./ansible

$ make rpm

$ sudo rpm -Uvh ./rpm-build/ansible-*.noarch.rpm
```

>Para sistemas basados en Debian o Redhat utilizar su propio gestor de paquetes como `apt` o `yum`.


## Configuración

Ansible orquesta los distintos nodos mediante un archivo de inventario, la ruta por defecto de este archivo es `/etc/ansible/hosts` y puede ser modificado en el archivo de configuración de ansible ubicado en `/etc/ansible/ansible.conf` habilitando la línea "".<br>
El archivo hosts es de texto plano y contiene los nombres de los nodos o sus IPs y además pueden estar agrupados para despliegues controlados.<br>

### Ejemplo de archivo hosts.

```
[prod]
10.100.101.10
host01.prod
myhost.dev

newhost.qa
10.100.101.11
```
>En el ejemplo existe el grupo "prod" el cual contiene 3 servidores y otros dos servidores que no pertenecen a ningún grupo.


Con esta configuración básica ya es posible realizar automatizaciones. En la siguiente sección abordaremos la sintaxis para utilizar algunos módulos.

