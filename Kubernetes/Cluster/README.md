# Cluster Kubernetes (Kismatic)

<br>

## Descargar y descomprimir Kismatic

```sh
$ mkdir /home/usuario/kismatic; cd /home/usuario/kismatic
$ wget https://github.com/apprenda/kismatic/releases/download/v1.7.0/kismatic-v1.7.0-linux-amd64.tar.gz -O | tar -xz
```
>**Nota:** Verificar la última versión de kismatic en https://github.com/apprenda/kismatic/releases

<br>

## Crear una llave RSA

La llave se crea para tener una relación de confianza hacia los nodos que compondrán el cluster.

```sh
$ ssh-keygen -f lab01
    Generating public/private rsa key pair.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in lab01.
    Your public key has been saved in lab01.pub.
    The key fingerprint is:
    xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx lab01k8s
    The key's randomart image is:
```

<br>

## Copiar llave RSA con Ansible

### Contenido del archivo hosts

Se debe configurar el archivo hosts de ansible con los nodos a los que se les copiará la llave creada.

```
[lab01] # Nombre del grupo de servidores
10.101.100.3 # IP nodo1
10.101.100.4 # IP nodo2
10.101.100.5 # IP nodo3


[lab01:vars] # Variables para el grupo lab01
ansible_connection=ssh # Tipo de conexión
ansible_ssh_user=root # Usuario de conexión
ansible_ssh_pass=MiPassword # Contraseña de conexión
ansible_host_key_checking=false # No verifique fingerprint al conectar
```

<br>

### Playbook ansible

El siguiente contenido es del playbook para copiar la llave a los tres nodos que conformarán el cluster kubernetes (copia_llave.yml).

```
---
- hosts: lab01
  vars:
    ssh_key: ssh-rsa AAAABBBBBBBBBBBCCCCCCCCCCCCCCCCCCCDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDEEEEEEEEEEEEEEEEEEEEEEEEEEFFFFFFFFFFFFFFFFFF+GGGGGGGGGGGGGGGGGGGGGGGGGGGHHHHHHHHHHHHHHHHHHHHHHHHHHHHIIIIIIIIIIIIIII+JJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKKLLLLLLLLLLLLLLLLLLLLLLLLLLLLLLLMMMMMMMMMMMMMMMMMMMMMMMMMNNNNNNNNNNNOOOOOOOOOOOOOOOOOOOPPPPPPPPPPPPPPPQQQQQQ== lab01k8s
  remote_user: root
  become: true
  tasks:
    - name: copiar llave publica de usuario
      authorized_key: user=root key={{ssh_key}} manage_dir=yes state=present
```

<br>

Ejecutar playbook.

```
$ ansible-playbook copia_llave.yml
```
***
<br>

## Instalación del cluster Kubernetes

### Ejecutar Kistmatic en modo plan

```sh
$ ./kismatic install plan
    Plan your Kubernetes cluster:
    => Number of etcd nodes [3]: 1
    => Number of master nodes [2]: 1
    => Number of worker nodes [3]: 2
    => Number of ingress nodes (optional, set to 0 if not required) [2]: 1
    => Number of storage nodes (optional, set to 0 if not required) [0]:
    => Number of existing NFS volumes to be attached [0]:

    Generating installation plan file template with:
    - 1 etcd nodes
    - 1 master nodes
    - 2 worker nodes
    - 1 ingress nodes
    - 0 storage nodes
    - 0 nfs volumes
```
>En el ejemplo se creó un Cluster con un nodo etcd, un nodo master, un nodo ingress y dos nodos worker. Para etcd, master e ingress el nodo01 cumplirá dicha función, el roll de worker será para el nodo02 y nodo03.<br>
**IMPORTANTE**: Toda la configuración queda almacenada en el archivo [kismatic-cluster.yaml](/Kubernetes/Cluster/Kismatic_Cluster_yaml.md). Antes de iniciar el instalador se deben modificar algunas opciones de este archivo.

<br>

### Ejecutar Kismatic en modo validación

```sh
$ ./kismatic install validate
    Validating==========================================================================
    Reading installation plan file "kismatic-cluster.yaml"                          [OK]
    Validating installation plan file                                               [OK]
    Validating SSH connectivity to nodes                                            [OK]
    Configure Cluster Prerequisites                                                 [OK]
    Gather Node Facts                                                               [OK]
    Update Hosts File                                                               [OK]
    Run Cluster Pre-Flight Checks                                                   [OK]
```

<br>

### Ejecutar Kismatic en modo instalación

Una vez que el plan y la validación de la instalación están listos, procedemos con la instalación del cluster.

```sh
$ ./kismatic install apply

Validating==========================================================================
Reading installation plan file "kismatic-cluster.yaml"                          [OK]
Validating installation plan file                                               [OK]
Validating SSH connectivity to nodes                                            [OK]
Configure Cluster Prerequisites                                                 [OK]
Gather Node Facts                                                               [OK]
Update Hosts File                                                               [OK]
Run Cluster Pre-Flight Checks                                                   [OK]

Configuring Certificates============================================================
Found valid certificate for node01 etcd server                                  [OK]
Found valid certificate for node01 API server                                   [OK]
...
Found valid certificate for node03 kubelet                                      [OK]
Found valid certificate for admin client                                        [OK]
Cluster certificates can be found in the "generated" directory                  [OK]

Generating Kubeconfig File==========================================================
Generated kubeconfig file in the "generated" directory                          [OK]

Installing Cluster==================================================================
Configure Cluster Prerequisites                                                 [OK]
Gather Node Facts                                                               [OK]
Update Hosts File                                                               [OK]
Deploy Cluster Certificates                                                     [OK]
Generate Kubectl Config File                                                    [OK]
...
Deploy Etcd Certificates                                                        [OK]
Update Kismatic Version File                                                    [OK]

Running Smoke Test==================================================================
Smoke Test Master Node                                                          [OK]

The cluster was installed successfully!

- To use the generated kubeconfig file with kubectl:
    * use "./kubectl --kubeconfig generated/kubeconfig"
    * or copy the config file "cp generated/kubeconfig ~/.kube/config"
- To view the Kubernetes dashboard: "./kismatic dashboard"
- To SSH into a cluster node: "./kismatic ssh etcd|master|worker|storage|$node.host"
```
