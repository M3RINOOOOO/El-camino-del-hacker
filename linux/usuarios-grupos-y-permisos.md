# Usuarios, grupos y permisos

## Usuarios

Un **usuario** en Linux es una identidad asociada a una cuenta, que puede ser una persona (como un administrador o un empleado) o un proceso del sistema. Cada usuario tiene un nombre de usuario (username) y un identificador de usuario (UID), que es un número único que el sistema usa internamente para referirse al usuario.

El **usuario root** en Linux es el superusuario con el nivel más alto de privilegios, que puede realizar cualquier acción en el sistema, incluyendo la instalación de software, la modificación de archivos de sistema, y la gestión de usuarios y permisos.

Para crear un nuevo usuario y establecerle una contraseña podemos ejecutar lo siguiente en la terminal:

```bash
useradd pepe -s /bin/bash -d /home/pepe # Asignamos una shell y un directorio personal al nuevo usuario
passwd pepe                             #creamos una contraseña para pepe
```

## Grupos

Un **grupo** en Linux es un conjunto de usuarios que se agrupan bajo un nombre común y comparten permisos sobre archivos y directorios. Cada grupo tiene un identificador de grupo (GID). Para crear un grupo y meter en el mismo a un usuario podemos ejecutar lo siguiente:

```bash
groupadd alumnos                       # crea el grupo alumnos	
usermod -a -G alumnos pepe 	           # mete en el grupo alumnos a pepe
```

## Permisos, bits y más cosas de archivos

### Tipos de usuarios para archivos

* **Propietario (Owner)**: El usuario que creó el archivo o directorio.
* **Grupo (Group)**: Un grupo de usuarios que comparten ciertos permisos sobre el archivo o directorio.
* **Otros (Others)**: Todos los demás usuarios del sistema.

### Tipos de permisos para archivos

* **Lectura (Read `-r`)**: Permite ver el contenido de un archivo o listar el contenido de un directorio.
* **Escritura (Write `-w`)**: Permite modificar el contenido de un archivo o cambiar el contenido de un directorio (crear, eliminar archivos).
* **Ejecución (Execute `-x`)**: Permite ejecutar un archivo (si es un programa o script) o acceder a un directorio.

### Representación de permisos de archivos

Los permisos en Linux controlan quién puede leer, escribir o ejecutar archivos y directorios. Cada archivo o directorio tiene tres conjuntos de permisos para tres tipos de usuarios, que son los mencionados anteriormentre.

Los permisos se representan con 10 caracteres en total, como se ve en la salida del comando `ls -l`. Nos centraremos en el siguiente ejemplo:

`-rwxr-xr--`

El primer carácter indica el tipo de archivo (`-` para archivos, `d` para directorios), mientras que los siguientes 9 caracteres se dividen en tres conjuntos de tres:

* **rwx**: Permisos del propietario.
* **r-x**: Permisos del grupo.
* **r--**: Permisos de otros.

### Modificación de permisos de archivos

Para ello haremos uso del comando `chmod`, que podremos utilizar de dos maneras:

* **Modo simbólico**: Se utiliza letras y símbolos para representar los permisos y las acciones que se desean realizar:

```bash
chmod o+w archivo           # a otros le añade escritura (w)
chmod g-r archivo           # a grupo le elimina lectura (r)
chmod u-x,g-rw,o-w archivo  # (mix de todo xd)
```

* **Modo numérico**: Utiliza un sistema octal para representar los permisos. Cada permiso tiene un valor numérico (Lectura:4;Escritura:2;Ejecución:1). Así los permisos para el propietario, grupo y otros se combinan en un número de tres dígitos:

```bash
chmod 755 archivo         #establece rwxr-xr-x
```

### Cambiar propietario y grupo de archivos

Para cambiar el propietario y el grupo del archivo en el caso de querer actualizar para los permisos podemos ejecutar los siguientes comandos:

```bash
chgrp grupo archivo 	         # cambia el grupo de chmod al archivo archivo
chown persona archivo 	         # cambia el propietario del archivo a persona
chown propietario:grupo archivo  # cambia el propietario y grupo a al archivo
```

### Sticky bit

El **Sticky Bit** es un permiso especial en Linux que se utiliza principalmente en directorios para controlar quién puede eliminar o renombrar archivos dentro de ese directorio. Por ejemplo, si creamos un directorio con permisos de escritura por otros, y dentro creamos un archivo en el que otros no tienen permiso de escritura, podrían eliminar este archivo, pero si le incrustamos el sticky bit, no. Para añadir este permiso, podemos ejecutar lo siguiente:

```bash
chmod +t archivo
```

### lsattr y chattr

**lsattr** se utiliza para listar los atributos especiales de archivos y directorios en un sistema de archivos Linux. Estos atributos determinan ciertas características adicionales del comportamiento del archivo, como si es inmutable o si se borrará al vaciar la papelera. La salida muestra una serie de letras que representan estos atributos. para mostrar como unas flags de permisos avanzados del archivo.

```bash
lsattr archivo.txt           # Mostará algo como ----i-------- archivo.txt
```

**chattr** permite cambiar los atributos especiales de archivos y directorios. Por ejemplo, puedes usar `chattr` para marcar un archivo como inmutable (`+i`), lo que impide que se modifique, elimine o renombre, incluso para el usuario root.

```bash
chattr +i             # añade la flag de immutabilidad
chattr -i             # elimina la flag de immutabilidad
```

### SUID Y SGID

Si el bit de **suid** está activado te permite ejecutar el binario de forma temporal como si fueras el propietario del archivo.

Para añadir el bit de **suid** a un binario se haría de la siguiente manera:

```bash
chmod 4755 usr/bin/python3.9      # manera octal (se añade un 4 al principio)
chmod u+s usr/bin/python3.9       
```

Ahora al hacer un `ls -l` aparecerá una `s` en la tercera posición de la tupla de user `-rwsr-xr-x`.

Para buscar los archivos que tengan este bit activado podemos utilizar el siguiente comando:

```bash
find / -type f -perm -4000 2>/dev/null  
```

Si el bit de **sgid** está activado pasa lo mismo que con el **suid**, pero en vez de a nivel de propietario de grupo.

Para añadir el bit de **guid** a un binario se haría de la siguiente manera:

```bash
chmod 2755 usr/bin/python3.9        # manera octal (se añade un 2 al principio)
chmod g+s usr/bin/python3.9
```

Ahora al hacer un `ls -l` aparecerá una `s` en la tercera posición de la tupla de group `-rwxr-sr-x`.

Para buscar los archivos que tengan este bit activado podemos utilizar el siguiente comando:

```bash
find / -type f -perm -2000 2>/dev/null para ver los archivos con el bit de sgid activado
```

### CAPABILITY

Una **capability** (capacidad) es una unidad de privilegio que permite a los procesos realizar acciones específicas sin necesidad de ser el usuario **root**. Las **capabilities** dividen los permisos tradicionales del superusuario en fragmentos más pequeños, otorgando permisos granulares para operaciones específicas.

Para manejar las capabilites de un archivo podemos ejecutar los siguientes comandos:

```bash
getcap -r / 2>/dev/null                 # muestra los archivos con alguna capability activada
getcap archivo                          # muestra las capabilities asignadas a un archivo
setcap cap_setuid+ep /usr/bin/python3.9 # establece la capability de setuid para ese binario, que permite cambiar el id del usuario (podriamos cambiar a root y la cosa se tensa)
setcap -r /usr/bin/python3.9            # elimina las capabilities de un binario
```
