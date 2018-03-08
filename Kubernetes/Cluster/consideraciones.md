# Consideraciones

## Nodos

- Deben tener salida a internet, de lo contrario se le configurará un proxy con kismatic.
- El hostname debe ser el mismo que en la configuración de kismatic.
- No deben tener swap.
- Debe tener thinpool.
- Debe tener VG para /var/lib/docker
- En /etc/resolv.conf debe tener agregado la línea `search`.
