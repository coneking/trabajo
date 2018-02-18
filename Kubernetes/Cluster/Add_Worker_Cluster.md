# Adicionar worker a un cluster Kubernetes


## Agregar permisos de sudo a los servidores del archivo `new_nodes`

Contenido del archivo `new_nodes`:

```
[new]
10.10.10.10

[new:vars]
ansible_connection=ssh
ansible_ssh_user=root
ansible_ssh_pass=Mipassword
host_key_checking=false
```


Playboook para agregar sudo:

```
---
- hosts: all
  remote_user: root
  become: true
  tasks:
    - name: comentar permiso de wheel con passwd
      lineinfile:
        path: /etc/sudoers
        state: present
        regexp: '^%wheel\s'
        line: '#%wheel  ALL=(ALL)       ALL'
    - name: descomentar permiso de wheel sin passwd
      lineinfile:
        path: /etc/sudoers
        state: present
        regexp: '^# %wheel\s'
        line: '%wheel  ALL=(ALL)       NOPASSWD: ALL'
```

<br>

Ejecutar el playbook para agregar sudo:

```
ansible-playbook -i new_nodes sudowheel.yml
```
   
<br>

## Agregar llave ssh del usuario root.

Playbook para agregar llave:

```
---
- hosts: all
  vars:
    ssh_key: "llave_ssh-rsa_yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv"
    remote_user: root
  become: true
  tasks:
    - name: copiar llave publica de usuario
      authorized_key: user=root key={{ssh_key}} manage_dir=yes state=present
```
   
<br>

Ejecutar el playbook para copiar la llave:

```
ansible-playbook -i new_nodes labk8s.yml
```

>Con esto el servidor tiene lo necesario para poder ser agregado al cluster de kubernetes.


## Agregando el nodo

```
./kismatic install add-worker nombre_del_nodo ip_del_nodo

	Running Pre-Flight Checks On New Worker=============================================
	Configure Cluster Prerequisites                                                 [OK]
	Gather Node Facts                                                               [OK]
	Update Hosts File                                                               [SKIPPED]
	Run Cluster Pre-Flight Checks                                                   [OK]

	Generating Certificate For Worker Node==============================================
	Generated certificate for g500603svgdm kubelet                                  [OK]
	Found valid certificate for kube-proxy                                          [OK]
	Found valid certificate for etcd client                                         [OK]

	Adding Worker Node to Cluster=======================================================
	Configure Cluster Prerequisites                                                 [OK]
	Gather Node Facts                                                               [OK]
	Update Hosts File                                                               [SKIPPED]
	Deploy Cluster Certificates                                                     [OK]
	Generate Kubectl Config File                                                    [OK]
	Configure Kismatic Package Repos                                                [OK]
	Install Docker                                                                  [OK]
	Start Kubernetes Kubelet                                                        [OK]
	Start Kubernetes Proxy                                                          [OK]
	Configure Kubernetes CNI                                                        [OK]
	Start Calico Network Components                                                 [OK]
	Validate Calico Network Components                                              [OK]
	Start Weave Network Components                                                  [SKIPPED]
	Validate Weave Network Components                                               [SKIPPED]
	Start Contiv Network Components                                                 [SKIPPED]
	Update Kismatic Version File                                                    [OK]

	Running New Worker Smoke Test=======================================================
	Smoke Test New Worker                                                           [OK]
```