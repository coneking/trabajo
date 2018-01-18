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
Aca podemos editar y dejar los cambios que correspondan. Tambien deberemos eliminar las lineas agregaras para señalar los cambios.



