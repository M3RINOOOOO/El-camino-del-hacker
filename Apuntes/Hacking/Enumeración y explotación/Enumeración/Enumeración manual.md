-------------
- Tags: #hacking #enumeración #bash #escalada_de_privilegios 
---------
Para realizar el proceso de [[Enumeración]], podemos hacer uso de los siguientes comandos en bash:

```bash
whoami    #Muestra el nombre del usuario que está actualmente conectado y ejecutando comandos
id        #Muestra los grupos al que pertenece el usuario actual
sudo -l   #Muestra si tenemos algún privilegio asignado a nivel de sudoers
getcap -r / 2>/dev/null   #Muestra las capabilities del usuario actual
```

Además podemos hacer uso de la [[Búsqueda a nivel del sistema]] para encontrar archivos interesantes, por ejemplo archivos con el permiso de SUID activado podemos ejecutar el siguiente comando en bash:

```bash
find / -perm -4000 2>/dev/null
```

Para posteriormente intentar inyectar algún comando, con la posibilidad de realizar una [[Escalada de privilegios]]. Una página muy interesante para este tipo de binarios es [GTFOBins](https://gtfobins.github.io/).

También podemos hacer un listado de las tareas de [[Cron]] para ver las tareas que se están ejecutando pueden ser modificados:

```bash
crontab -l         #Muestra si el usuario actual tiene tareas ejecutándose
cat /etc/crontab   #Muestra las tareas que se están ejecutando cada cierto periodo
systemctl list-timers #Muestra cuánto tiempo queda para que se ejecute determinada tarea
```