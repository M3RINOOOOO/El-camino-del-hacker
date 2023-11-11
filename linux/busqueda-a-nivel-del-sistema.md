# Búsqueda a nivel del sistema

En este apartado veremos diferentes maneras de utilizar el comando find para hacer buscar en un sistema (por eso utilizamos continuamente **/** en los ejemplos, si quisiéramos buscar en un directorio en específico habría que poner el mismo) por diferentes criterios. Recordemos que todo lo que nos muestre como **stderr** no lo queremos mostrar por pantalla

Para filtrar por el nombre del archivo/directorio buscado:

```bash
find / -name passwd 2>/dev/null
find / -name pass\*               #Podemos utilizar expresiones regulares en la búsqueda
```

Para filtrar por los **permisos** del archivo/directorio buscado:

```bash
find / -perm -4000 2>/dev/null
```

Para filtrar por el **grupo** al que pertenece el archivo/directorio buscado:

```bash
find / -group wheel 2>/dev/null
```

Para filtrar por el **usuario** perteneciente al archivo/directorio:

```bash
find / -user root 2>/dev/null
```

Para filtrar por los archivos/directorios que son leíbles, editables o ejecutables/abiertos por **otros**:

```bash
find / -readable 2>/dev/null
find / -writable 2>/dev/null
find / -executable 2>/dev/null
```

Para filtrar por el tamaño del archivo/directorio:

```bash
find / -size 1234c             #Con este ejemplo buscamos archivos/directorios que tengan 1234 bytes (para las demás unidades mirar el manual)
```

Si queremos realizar la búsqueda con archivos que **no** cumplan lo que queremos bastaría con poner un ! antes del parámetro, si quisiéramos buscar los archivos/directorios que no tienen el **permiso suid** activado:

```bash
find / ! -perm 4000 2>/dev/null  
```

Si queremos lo que estamos buscando es un archivo/directorio, se puede distinguir de la siguiente manera:

```bash
find / -name archivo -type f           #Estamos buscando arhcivos
find / -name directorio -type d        #Estamos buscando directorios
```
