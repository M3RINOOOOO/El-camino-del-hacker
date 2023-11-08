-----------------------
- Tags: #herramientas #forense #memory_dump
------------------
# Definición

> **Volatility** es una herramienta de código abierto esencial en el campo de la informática forense y análisis de malware, diseñada para analizar la memoria volátil de sistemas informáticos, como la RAM. Permite a los investigadores examinar procesos en ejecución, conexiones de red, archivos abiertos y otros datos clave en la memoria.

------
# Instalación con docker

Es una herramienta escrita en *python2*, por lo que necesitará muchas dependencias. En mi caso, preferí no tener que instalar tantas cosas, por lo que hice uso de [[Docker]]. Para ello puedes descargar la imagen de *docker*:

```
docker pull phocean/volatility
```

Y una vez instalada, podemos declarar una función dentro del archivo de configuración de nuestra shell (```.bashrc``` o ```.zshrc```) para hacer un uso más sencillo de la herramienta:

```
function volatility() {
  docker run --rm --user=$(id -u):$(id -g) -v "$(pwd)":/dumps:ro,Z -ti phocean/volatility $@
}
```

Y para ver si se ha instalado correctamente, ejecutamos el siguiente comando, que devolverá la versión que se está usando:

```
volatility 
```

Para poder utilizar un volcado de memoria, debemos crear un directorio en la raíz llamado ```dumps``` y meter ahí los archivos deseados para el análisis:

```
mdkir /dumps
mv /ruta/al/volcado /dumps
volatility - f /dumps/volcado <OPCIONES>
```

---
# Uso

## Obtener información del volcado de memoria

Para conseguir información acerca del volcado de memoria ejecutaremos el siguiente comando:

```
volatility -f <MEMORIA> imageinfo
```

Del output nos interesará el campo de *Suggested Profiles*, que utilizaremos para posteriores comandos. Puede darse el caso que nos enfrentemos a una memoria de un sistema *windows* o *linux*.

Para mostrar aún mas información, que no es relevante a no ser que nos enfrentemos a un caso real, ejecutamos:

```
volatility -f <MEMORIA> kdbscan
```
## Volcado de memoria de Windows

### Listado de procesos activos

Nos será muy útil conseguir los procesos activos en el momento de la captura de memoria, ya que nos ayudará a identificar procesos maliciosos o inesperados. Para ver los procesos usuales que corren en *Windows*, podemos ver la página de [Hacktricks](https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/windows-forensics/windows-processes), por lo que estos procesos en principio no deberían de ser sospechosos, ya que a veces se puede ocultar.

Tendremos cuatro comandos para realizar el listado de procesos *pstree*, *pslist*, *psscan* y *psxview*, y hay diferencias entre cada uno de ellos.

- **pstree**: Este comando nos mostrará todos los procesos de memoria ram, en formato de árbol, por lo que será una opción muy visual:

```
volatility -f <MEMORIA> --profile=<PROFILE> pstree
```

- *pslist*: Exactamente igual que el anterior, solo que este nos muestra los *offset virtuales* y además sin la forma jerarquizada, y también puede mostrar más procesos:

```
volatility -f <MEMORIA> --profile=<PROFILE> pslist
```

- *psscan*: Se centra en los procesos de memoria que más recursos toman:

```
volatility -f <MEMORIA> --profile=<PROFILE> psscan
```

- *psxview*: Muestra si algunos de los métodos anteriores tenían un proceso oculto (debería de salir en su columna como *false*):

```
volatility -f <MEMORIA> --profile=<PROFILE> psxview
```

### Ver conexiones entrantes y salientes

Para ello haremos uso de los comandos *connscan* y *sockets*.

- *connscan*: Nos permite ver las conexiones de nuestra IP privada con otras IPs públicas o privadas por un servicio que está corriendo:

```
volatility -f <MEMORIA> --profile=<PROFILE> connscan
```

Esto nos devolverá los procesos que están utilizando dichas conexiones, que podemos observar con uno de los comandos mencionados anteriormente, lo que nos ayudará a identificar los procesos que están utilizando las conexiones observadas.

- *sockets*: Nos permitirá ver los procesos con los puertos que se han abierto y con las conexiones que se han empleado:

```
volatility -f <MEMORIA> --profile=<PROFILE> sockets
```

### Ver instrucciones que han sido utilizadas

Una vez hemos identificado las IPs maliciosas, el paso siguiente sería ver las instrucciones que han sido ejecutadas desde el dispositivo, es decir, ver si el proceso malicioso ha ejecutado comandos o abierto alguna ventana. Para ello utilizaremos tres comandos esenciales en la herramienta de **volatility**, que son *consoles*, *cmdline* y *cmdscan*.

- *consoles*: muestra las sesiones de consola activas en la memoria volátil:

```
volatility -f <MEMORIA> --profile=<PROFILE> consoles
```

- *cmdline*: extrae la línea de comandos asociada a procesos en ejecución

```
volatility -f <MEMORIA> --profile=<PROFILE> cmdline
```

- *cmdscan*: busca artefactos de líneas de comandos utilizados en el sistema

```
volatility -f <MEMORIA> --profile=<PROFILE> cmdscan
```

### Listado de DLLs

Los archivos **DLL (Dynamic Link Libraries)** son componentes de software en sistemas operativos Windows que contienen funciones y datos que múltiples programas pueden utilizar para realizar tareas comunes. Estos se cargan dinámicamente en la memoria cuando se necesita y se vinculan a programas en tiempo de ejecución, lo que les permite reutilizar código y recursos compartidos.

Para hacer un listado de los DLLs, ejecutaremos el siguiente comando:

```
volatility -f <MEMORIA> --profile=<PROFILE> dlllist
```

### Dumpeo de procesos

Para realizar un análisis de un proceso que nos parezca interesante, necesitamos dumpearlo:

```
volatility -f <MEMORIA> --profile=<PROFILE> memdum -p <ID_PROCESO> --dump-dir .
```

Una vez tengamos dumpeado el proceso, podemos leer las cadenas legibles del mismo con el comando ``strings``:

```
strings <ID_PROCESO>.dmp
```

Al resultado le tendremos que realizar distintos filtrados para conseguir la información que consideremos interesante.

### Dumpeo de hashes de usuarios

No siempre se podrá, ya que se tienen que dar ciertas condiciones para que se puedan conseguir. Haremos uso del comando ```hashdump```:

```
volatility -f <MEMORIA> --profile=<PROFILE> hashdump
```
## Volcado de memoria de Linux

