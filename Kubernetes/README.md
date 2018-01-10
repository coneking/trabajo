# Kubernetes

## ¿Qué es Kubernetes?

Kubernetes es una herramienta open source que permite la orquestación de contenedores [docker](https://github.com/coneking/trabajo/tree/master/Docker).


## Conceptos

- **Nodo**: Servidor físico o virtual que contiene `pods`.
- **Pods**: Grupo de contenedores en los que se despliegan aplicaciones.
- **Replication Controller**: Administra pods y se asegura que las ***réplicas*** de los pods estén ejecutándose. Facilita el escalar sistemas y si existe algún error en un pod, lo puede crear nuevamente.
- **Service**: Define como aceder a un pod o a un grupo de pods.


# Comandos Kubernetes

Revisar el uso de los nodos
```
$ kubectl top nodes
```
<br>

Revisar el uso de los pods
```
$ kubectl top -n "nombre_del_namespace" pod
```

<br>

Ver los cluster configurados.
```
$ kubectl config get-contexts
CURRENT   NAME      CLUSTER   AUTHINFO    NAMESPACE
*         cl_prod   cl_prod   admin
          cl_lab    cl_lab    admin-lab
```
>El `*` indica el cluster en el que estamos actualmente.

<br>

Obtener solo los nombres de los clusters.
```
$ kubectl config get-clusters
NAME
cl_prod
cl_lab
```
<br>

Ver el cluster en el que nos encontramos.
```
$ kubectl config current-context
cl_prod
```

<br>

Cambiar entre clusters.
```
$ kubectl use-context cl_lab
Switched to context "cl_lab".
```

<br>

Consultar nodos de un cluster.
```
$ kubectl get nodes
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

<br>

Quitar un nodo del cluster.
```
$ kubectl drain "nombre_del_nodo" --ignore-daemonsets --force --delete-local-data
node "nombre_del_nodo" cordoned
WARNING: Ignoring DaemonSet-managed pods: tick-polling-telegraf-ds-6g3h3, calico
t or StatefulSet: kube-proxy-nombre_del_nodo
node "nombre_del_nodo" drained
```

<br>

Reintegrar un nodo al cluster.
```
$ kubectl uncordon "nombre_del_nodo"
```

<br>

Listar los namespaces.
```
$ kubectl get namespaces
NAME          STATUS    AGE
default       Active    150d 
My_ns         Active    40d
Other_ns      Active    32d
```

<br>

Listar todos los pods de todos los namespaces.
```
$ kubectl get pods --all-namespace
```

<br>

Listar los pods de un namespace
```
$ kubectl -n "nombre_del_namespace" get pods
```

<br>

Listar los pods con el nodo y la IP que los contiene.
```
$ kubectl -n "nombre_del_namespace" get pods -o wide
```

<br>

Revisar logs de un pod
```
$ kubectl -n "nombre_del_namespace" logs "nombre_del_pod"
```
>Si el pod está compuesto de más de un contenedor se debe elegir el contenedor (la opción `-f` es paracontinuar revisando el log)
`kubectl -n "nombre_del_namespace" logs "nombre_del_pod" -c "nombre_del_contenedor" -f`

<br>

Información detallada de un pod
```
$ kubectl -n "nombre_del_namespace" describe pods "nombre_del_pod"
```

<br>

Eliminar un pod.
```
$ kubectl -n "nombre_del_namespace" delete pods "nombre_del_pod"
```

