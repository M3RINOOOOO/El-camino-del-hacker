---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #SSRF #exploiting 
--------------
# Definición

> **SSRF (Server-Side Request Forgery)** es una [[Vulnerabilidades web]] en la que un atacante manipula un servidor para realizar solicitudes a otros sistemas, a menudo internos. Esto puede dar acceso no autorizado a datos o sistemas sensibles, como contraseñas, claves API y otros más.
> 
> En este tipo de ataques, el atacante utiliza una entrada del usuario, como un campo de formulario o una URL, para enviar una solicitud HTTP a un servidor web. El atacante puede manipular la solicitud para que se dirija a un servidor vulnerable o a una red interna a la que el servidor tiene acceso. Esto puede acabar en una [[RCE (Remote Command Execution)]] por el lado del servidor web o en otros servidores de la red interna.
> 
> Una de las **diferencias** clave entre el **SSRF** y el [[CSRF (Cross-Site Request Forgery)]] es que el **SSRF** se ejecuta en el servidor web en lugar del navegador del usuario. El atacante **no necesita engañar a un usuario legítimo** para hacer clic en un enlace malicioso, ya que puede enviar la solicitud HTTP directamente al servidor web desde una fuente externa.
> 
> Para evitar este tipo de ataques los desarrolladores de aplicaciones web deben validar y filtrar adecuadamente la entrada del usuario y limitar el acceso del servidor web a los recursos de la red interna. Además, los servidores web deben ser configurados para limitar el acceso a los recursos sensibles y restringir el acceso de los usuarios no autorizados.

-------------
# Detección de vulnerabilidad 

Para detectar este tipo de vulnerabilidad, es necesario que en algún lado de la página web nos permita introducir una **URL**, independientemente del uso al que se le esté dando. Podemos intentar trabajar también con **ips privadas** para ver si podemos acceder a sistemas de la misma **red interna**. Puede ser que para introducir estos datos, podamos introducirlos manualmente en la **URL** original como parámetro.

------------
# Explotación

Suponiendo el caso en el que podemos introducir la **URL** que queremos visitar dentro de la **URL original** como un parámetro, podemos intentar mostrar el contenido del servidor proporcionado al mismo como url, como **localhost**:

```
?url=http://localhost
?url=http://127.0.0.1
```

Y si muestra el contenido del mismo el ataque ha sido exitoso. 

Puede ser que el servidor tenga puertos internamente abiertos que no puedan ser escaneados ni accedidos desde el exterior, pero si es vulnerable a **SSRF**, podemos intentar listar los puertos que pueden ser accedidos desde el mismo servidor con alguna herramienta de **fuzzing** como [[Wfuzz]], [[Ffuf]] o [[Gobuster]]. Un ejemplo con **Wfuzz** podría ser el siguiente, suponiendo que podemos introducir la **URL** como un parámetro en la **URL original**:

```
wfuzz -c -t 200 -z range,1-65535 "http://<IP_VICTIMA>/?url=http://localhost:FUZZ"
#podemos hacer un filtrado de las salidas para conseguir las respuestas que nos interesan por ejemplo con --hl=3
```

Para posteriormente analizar las páginas y servicios alojados en dichos puertos encontrados.

Como hemos mencionado anteriormente, puede ser que nos estemos enfrentando a un conjunto de máquinas, en las que podemos solo tengamos acceso de una, pero podemos ir consiguiendo acceso a las demás ya que se encuentran en una misma **red interna**, utilizando [[Pivoting]]. Para ello podemos hacer uso en vez del **localhost**, de la ip de la máquina a la que queramos acceder, de manera que podríamos tener acceso a la misma, utilizando **SSRF**. Si no conociésemos la dirección ip de las demás máquinas, también podríamos intentar aplicar **fuzzing** para descubrir las ips internas de la red. 

------------
# Ejemplos prácticos

Para montarnos un ejemplo práctico con redes internas, podemos hacer uso de [[Docker]], para montar nuestras redes y más.
