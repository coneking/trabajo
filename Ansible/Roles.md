# ROLES

<br>

## Roles Ansible-Galaxy

Para utilizar roles con ansible-galaxy se debe definir lo siguiente:
<br>

En el archivo `ansible.cfg`, en el apartado `[default]` tiene que estar especificada la siguiente opción.
```
roles_path = /ruta/roles
```
<br>

Elegir el paquete a instalar desde la web de ansible-galaxy y ejecutar:
```
ansible-galaxy install "nombre_del_paquete"
```
<br>

Crear un archivo dentro del rol instalado (archivo.yaml).
Información del archivo archivo.yaml
```yaml
---
- hosts: listado_de_hosts
  become: true
  roles:
    - nombre_del_rol
...
```
>Esto hará que todos los archivo "main.yaml" que se crean por defecto en el role, se ejecutarán.

<br>

## Listar roles

```
ansible-galaxy list
```

<br>

## Crear un rol localmente

Al igual que como se instalan roles desde la web de Ansible-Galaxy, se pueden crear manualmente.

Para crear un role local ejecutamos:
```
ansible-galaxy init --offline nombre_del_rol
```
>luego mover el directorio creado al directorio roles configurado en el archivo ansible.cfg.

<br>


## Ejemplo definiendo variables.

Las variables se definen de forma separada del archivo de jugadas, y éstas se guardan en el archivo `ruta_del_role/vars/main.yaml`.
<br>

### Crear variables dentro del rol creado (usuarios)
Editar archivo `vars/main.yml`
```     
vi /home/sconejer/ansible/practicando_a_fondo/roles/usuarios/vars/main.yml
```
<br>

Info del archivo main.yml
```yaml
---     
grupoA:
  - usuarioA
  - usuarioB

grupoB:
  - usuarioC
  - usuarioD
```
<br>

## Ejemplo definiendo tareas.
Las variables se definen de forma separada del archivo de jugadas, y éstas se guardan en el archivo `ruta_del_role/tasks/main.yaml`.
<br>

### Crear tareas para el rol.

Editar el archivo `tasks/main.yml` 
```
vi /home/sconejer/ansible/practicando_a_fondo/roles/usuarios/tasks/main.yml
```
<br>

Info del archivo main.yml

```yaml
---
- name: Creando grupos
  group:
    name: "{{ item }}"
    state: present
  with_items:
    - grupoA
    - grupoB

- name: Creando usuarios con variable definida grupoA
  user:
    name: "{{ item }}"
    state: present
    group: grupoA
  with_items: 
    - "{{ grupoA }}"

- name: Creando usuarios con variable definida grupoB
  user:
    name: "{{ item }}"
    state: present
    group: grupoB
  with_items: 
    - "{{ grupoB }}"

```

## Crear archivo principal.
Para llamar a todos los archivos `main.yaml` de nuestra estrucutra de roles, se debe crear un archivo con la siguiente información.
<br>

Crear un archivo yaml
```    
vim creando_usuarios.yaml
```
<br>

Contenido del archivo creando_usuarios.yaml, la opción "roles" indica las tareas que se ejecutarán de dicho rol.

```yaml
---
- name: Playbook creando usuarios
  hosts: nodo2
  become: true
  roles:
    - usuarios
```    
>Esto hará que todos los archivo "main.yaml" que se crean por defecto en el role, se ejecutarán.

<br>

Finalmente Ejecutar comprobación del archivo creando_usuarios.yaml.
```
ansible-playbook --syntax-check creando_usuarios.yaml
```
