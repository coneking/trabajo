# Comando útiles

### Obtener indices
```
curl -XGET 'localhost:9200/_cat/indices?v'
```
### Parseo para que solo se muestren los nombres importantes

``` 
curl -XGET 'localhost:9200/_cat/indices?v' | awk '{print $3}'|grep -v ^[.]
```

### Obtener status del cluster
```
curl -XGET 'http://localhost:9200/_cluster/health?pretty'

```

### Obtener mapping
```
curl -XGET 'http://localhost:9200/_all/mapping?pretty'
```
### Agrega indice
```
curl -XPOST 'http://localhost:9200/indice01'
```
### Elimina indice
```
curl -XDELETE 'http://localhost:9200/indice01'
```
### Escribir un registro
```
curl -XPOST 'http://localhost:9200/indice01/item01/gato01' -d '{"title" : "Gato gordo", "color" : "jaspeado", "pelo" : "largo y bota mucho" }'
```

### Traer los datos del registro
```
curl -XGET 'http://localhost:9200/indice01/item01/gato01'
```

### Traer solo los datos del registro, sin los default
```
curl -XGET 'http://localhost:9200/indice01/item01/gato01/_source?pretty'
```
### Buscar el registro
```
curl -XGET 'http://localhost:9200/indice01/_search?q=gato'
```
### Eliminar el registro
```
curl -XDELETE 'http://localhost:9200/indice01/item01/gato01'
```
### Validar si existe el registro
```
curl -I 'http://localhost:9200/indice01/item01/gato01'
```
### Ejemplo de llenado
```
curl -XPOST 'http://localhost:9200/indice01/item01/gato01' -d '{"title" : "Gato gordo", "color" : "jaspeado", "pelo" : "largo y bota mucho" }'

curl -XPOST 'http://localhost:9200/indice01/item01/gato02' -d '{"title" : "Gato flaco", "color" : "gris", "pelo" : "corto y casi no bota" }'
```

Indices  Esquemas
Tipos  Tablas

### Links utiles
https://www.elastic.co/guide/en/elasticsearch/reference/current/_list_all_indices.html