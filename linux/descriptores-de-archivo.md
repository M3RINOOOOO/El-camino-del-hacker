# Descriptores de archivo

> Un **descriptor de archivo** en Linux es un identificador único que el sistema operativo asigna a cada archivo abierto, dispositivo, socket, o cualquier otro tipo de entrada/salida (I/O). Este identificador es un número entero no negativo que permite al sistema y a las aplicaciones acceder y manipular estos recursos de I/O de manera eficiente.

## Descriptores de archivo comunes

En linux, los descriptores de archivo más comunes son los tres primeros, que se asignan automáticamente cuando un proceso es iniciado:

* **Descriptor de archivo 0**, entrada del usuario (stdin): Se usa para leer la entrada del usuario de otro programa, que suele estar asociado con el teclado. Se redirige con `<`.
* **Descriptor de archivo 1**, salida estándar (stdout): Se utiliza para escribir la salida del programa, que suele estar asociado con la terminal. Se redirige con `>`.
* **Descriptor de archivo 2**, error estándar (stderr): Se utiliza para escribir mensajes de error, que seuele estar asociado también con la terminal. Se redirige utilizando `2>`.

A continuación un par de ejemplos de como se utilizaría la redirección de dichos descriptores:

```bash
comando > archivo.txt        #Redirige la salida estándar al archivo
comando < archivo.txt        #Redirige la entrada estándar desde el archivo
comadno 2> error.txt         #Redirige la salida de error al archivo
comando > todo.log 2>&1      #Redirige tanto la salida estándar como la de error al archivo
```

## Otros descriptores de archivos

También se pueden crear y utilizar otros descriptores de archivos, de manera que se permite un control avanzado sobre la redirección de entrada/salida en scripts de **Bash**, proporcionando flexibilidad para manejar múltiples flujos de datos de manera eficiente. A continuación vienen ejemplos de uso de los mismos:

```bash
exec 3<> file                   #crea un archivo file asociado al 3
whoami >&3                      #envía la salida al file creado anteriormente
exec 3>&-                       #cierra el descriptor (no borra el archivo creado)
exec 8>&5                       #asocia el descriptor 8 con el 5
exec 8>&5-                      #asocia el descriptor 8 con el 5 pero se cierra el 5
```
