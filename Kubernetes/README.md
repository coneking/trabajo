<p align="center"><img src="https://raw.githubusercontent.com/coneking/trabajo/desarrollo/Kubernetes/images/kubernetes.png" width="300" /></p>

<br>

## ¿Qué es Kubernetes?

Kubernetes es una herramienta open source que permite la orquestación de aplicaciones en contenedores [docker](https://github.com/coneking/trabajo/tree/master/Docker).<br>
Se encarga del despliegue, la mantención y la escalabilidad de las aplicaciones

<br>

## Ejemplo Arquitectura

<p align="center"><img src="https://raw.githubusercontent.com/coneking/trabajo/desarrollo/Kubernetes/images/arch.png" width="400" /></p>

<br>

## Conceptos

- **Nodo**: Servidor físico o virtual que contiene `pods`.
- **Pods**: Grupo de contenedores en los que se despliegan aplicaciones.
- **Replication Controller**: Administra pods y se asegura que las ***réplicas*** de los pods estén ejecutándose. Facilita el escalar sistemas y si existe algún error en un pod, lo puede crear nuevamente.
- **Service**: Define como aceder a un pod o a un grupo de pods.


