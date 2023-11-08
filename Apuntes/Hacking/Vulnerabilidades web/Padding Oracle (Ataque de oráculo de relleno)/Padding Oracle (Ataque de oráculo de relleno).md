# Padding Oracle (Ataque de oráculo de relleno)

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #Padding\_oracle

***

## Definición

> El término **Padding Oracle** se refiere a una \[\[Vulnerabilidades web]] en sistemas criptográficos que emplean modos de cifrado en bloques como el modo CBC. Aquí, **padding** implica agregar bytes extras a un bloque antes de cifrarlo para ajustar su tamaño. En un ataque de **Padding Oracle**, un atacante manipula los **bytes de relleno** en un mensaje cifrado para obtener información sobre el mensaje original, potencialmente revelando datos confidenciales o incluso la clave de cifrado. De esta manera, se permite al atacante descifrar el contenido de los datos sin conocer la clave.
>
> Un **oráculo** se refiere a una indicación que ofrece al atacante información sobre si la acción que se se ejecuta es correcta o no. El **relleno** es un término criptográfico específico. Algunos **cifrados** funcionan en bloques de datos en los que cada bloque posee un tamaño fijo, de tal manera que si los datos que quieres cifrar no cumplen con ese tamaño adecuado para rellenar bloques, los datos se rellenan automáticamente hasta que lo hacen.
>
> Uniendo ambos conceptos, una implementación de software con **oráculo de relleno** revela si los datos descifrados tienen un relleno válido como no, por ejemplo, que te devuelva un mensaje indicando que el relleno no es válido, o también que pueda tomar un tiempo considerablemente diferente para procesar un bloque válido de uno que no lo es.
>
> Existe otra propiedad llamada **modo** existente en los cifrados basados en bloques, que determina la relación de los datos del un bloque con los datos del siguiente. Uno de los más utilizados es **CBC**, que presenta un bloque aleatorio inicial, y combina el bloque anterior con el resultado del cifrado estático para que cifrar el mismo mensaje con la misma clave no siempre genere la misma salida.
>
> Este tipo de ataque se basa en la capacidad de cambiar los datos cifrados y probar el resultado con el oráculo. Podemos utilizar un **oráculo de relleno**, en combinación de la manera de estructurar los datos de **CBC**, para enviar mensajes ligeramente modificados al código que es expuesto por el oráculo, y seguir enviando datos hasta que el oráculo indique que son correctos. Así, se podría descifrar el mensaje byte a byte.
>
> La única manera de erradicar este tipo de ataque es detectar los cambios en los datos cifrados y rechazar que se hagan acciones en ellos. La manera estándar de hacerlo es crear una **firma**, como un **HMAC** (**código de autenticación de mensajes hash con clave**), para los datos y validarla antes de realizar cualquier operación. La firma debe ser verificable y el atacante no debe poder crearla; de lo contrario, podría modificar los datos cifrados y calcular una firma nueva en función de esos datos cambiados.

***

## Detección de vulnerabilidad

Para detectar si la vulnerabilidad está presente en un servicio web, podemos hacer uso de las siguientes pruebas:

* **Análisis de errores o respuestas inusuales:** Si las respuestas o errores generados por el sistema al intentar descifrar un mensaje cifrado presentan errores como _padding incorrecto_, _padding válido pero datos erróneos_ o algo parecido, podría ser una señal de la vulnerabilidad.
* **Examinando los tiempos de respuesta:** Se podría analizar el tiempo de respuesta del sistema al enviar mensajes cifrados manipulados. Un tiempo de respuesta considerablemente más largo para mensajes con relleno válido en comparación con el relleno incorrecto, podría indicar la existencia de la vulnerabilidad.
* **Pruebas de cifrado y descifrado:** Intentando manipular los mensajes cifrados, podríamos examinar como reacciona el sistema. Introducir cambios en los bytes de relleno y bloques de cifrado, puede ser una buena técnica para ver si obtenemos respuestas interesantes por parte del servidor.

***

## Explotación

Supondremos que lo estamos tratando de descifrar es una **cookie** de sesión, (si estuviese incluido el usuario al que pertenece la cookie podríamos cambiarlo para estar identificados como otros usuarios). Cabe destacar que el tamaño de bloque en el cifrado **CBC** debe ser un múltiplo de 8 bits (1 byte).

Para tratar de descifrar dicha **cookie**, podemos hacer uso de la herramienta \[\[PadBuster]]:

```bash
padbuster <URL> <TEXTO_ENCRIPTADO> <TAMAÑO_BLOQUE> -cookies '<SESSION_COOKIE>'
```

Y nos debería devolver el texto descifrado, pero lo más interesante es lo mencionado anteriormente, suponiendo que en el texto claro salga información relativa al usuario (por ejemplo user=M3RINOOOOO), podríamos cambiar dicha cookie para contemplar la información que a nosotros nos interese:

```bash
padbuster <URL> <TEXTO_ENCRIPTADO> <TAMAÑO_BLOQUE> -cookies '<SESSION_COOKIE>' -plaintext '<TEXTO_A_CIFRAR>'
```

Cambiando por ejemplo el texto plano a user=admin, o cualquier tipo de información que queramos cifrar.

Si no queremos hacer uso de esta herramienta, podríamos utilizar la herramienta \[\[Burpsuite]], en el modo _inruder_. Podemos hacer uso de la cookie de sesión, que sabemos que tendrá forma "user=user", de manera que podemos crear un nuevo usuario llamado muy parecido a otro del que estemos buscando suplantar la identidad, por ejemplo de admin un usuario muy parecido podría ser bdmin.

Así podemos tratar de realizar un _Bit Flipper Attack_, de manera que cambiando algunos bits, podemos ir probando hasta tratar de encontrar la **cookie** correspondiente al admin. Dentro del modo intruder, realizaremos un ataque de tipo **Sniper**, y en la selección de payload, tendremos que seleccionar la opción de **Bit Flipper**.

***

## Ejemplos prácticos

* [**Pentester Lab– Padding Oracle**](https://www.vulnhub.com/?q=padding+oracle)
