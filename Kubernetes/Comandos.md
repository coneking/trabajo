# Comandos Kubectl


Listar los diferentes contextos de kubernetes:
```
kubectl config get-contexts
CURRENT   NAME      CLUSTER   AUTHINFO    NAMESPACE
*         cl_prod   cl_prod   admin
          cl_lab    cl_lab    admin-lab
```
>**Nota:** El caracter `*` indica el cluster actual

<br>

Obtener solo los nombres de los clusters.
```
$ kubectl config get-clusters
NAME
cl_prod
cl_lab
```

<br>

Mostrar el actual contexto:
```
kubectl config current-context
```
<br>

Cambiar entre contextos:
```
kubectl config use-context "nombre_del_contexto"
```
<br>

Poner un nodo en mantenimiento:
```
kubectl drain my-node
```
>**Nota:** Si el nodo tiene Pods activos, se moverán a otro nodo del cluster.

<br>

Quitar un nodo del cluster (unschedulable):
```
kubectl cordon "nombre_del_nodo"
```
<br>

Reintegrar un nodo al cluster (schedulable):
```
kubectl uncordon "nombre_del_nodo"
```
<br>

Listar los nodos del cluster:
```
kubectl get nodes
NAME           STATUS                     ROLES     AGE       VERSION
nodo1   Ready,SchedulingDisabled   <none>    16h       v1.7.4
nodo2   Ready                      <none>    66d       v1.7.4
nodo3   Ready,SchedulingDisabled   master    100d      v1.7.4
nodo4   Ready,SchedulingDisabled   master    100d      v1.7.4
nodo5   Ready                      <none>    100d      v1.7.4
nodo6   Ready                      <none>    100d      v1.7.4
nodo7   Ready                      <none>    100d      v1.7.4
nodo8   Ready                      <none>    100d      v1.7.4
nodo9   Ready,SchedulingDisabled   <none>    100d      v1.7.4
nodo10  Ready,SchedulingDisabled   <none>    100d      v1.7.4
```
>**Nota:** "-o wide" para mostrar más información.

<br>

Mostrar el estado de los nodos:
```
kubectl top nodes
```
>**Nota:** "-o wide" para mostrar más información.

<br>

Mostrar información de un nodo específico:
```
kubectl describe node "nombre_del_nodo"
```
<br>

Listar namespace:
```
kubectl get namespace
```
>**Nota:** namespace = ns

<br>

Crear un namespace:
```
kubectl create namespace "nombre_para_namespace"
```
<br>

Listar recursos de un namespace:
Ejemplo `pod`.
```
kubectl -n "nombre_namespace" get pods
```
>**Nota:** "-o wide" para mostrar más información.

<br>

Listar más de un recurso de un namespace al mismo tiempo:
Ejemplo `service`, `pod`, `replicaset`, `deployment`
```
kubectl -n "nombre_namespace" get svc,po,rs,deploy
```

<br>

Listar todos los pods de todos los namespace:
```
kubectl get pod --all-namespaces
```

<br>

Mostrar un recurso en formato `yaml`:
Ejemplo `service`.
```
kubectl -n "nombre_namespace" get svc -o yaml
```
>**Nota:** También se puede mostrar en formato json.

<br>

Mostrar información de un recurso específico:
Ejemplo `ingress`.
```
kubectl -n "nombre_namespace" describe ingress
```

<br>

Eliminar un recurso específico.
Ejemplo `pod`.
```
kubectl -n "nombre_del_namespace" delete pod "nombre_del_pod"
```

<br>

Ver logs de un pod:
```
kubectl -n "nombre_namespace" logs "nombre_pod"
```
>**Nota:** Para ver logs en tiempo real `logs -f`. Ver las últimas 20 líneas `logs --tail=20`

<br>

Ver logs del contenedor de un pod (pods con más de un contenedor)
```
kubectl -n "nombre_namespace" logs "nombre_pod" -c "nombre_contenedor"
```
>**Nota:** Con el parámetro `-c` se indica el nombre del contenedor a revisar. 

<br>

Revisar el uso de los pods
```
$ kubectl -n "nombre_del_namespace" top pod
```

<br>

Ingresar a un contenedor de un pod (pods con más de un contenedor)
```
kubectl -n "nombre_namespace" logs "nombre_pod" -c "nombre_contenedor"
```
>**Nota:** Con el parámetro `-c` se indica el nombre del contenedor a ingresar.

<br>

Habilitar proxy del cluster Kubernetes
```
kubectl proxy
```

Crear despliegue mediante un archivo `yaml`.
```
kubectl create -f "/directorio/del/archivo.yaml"
```


