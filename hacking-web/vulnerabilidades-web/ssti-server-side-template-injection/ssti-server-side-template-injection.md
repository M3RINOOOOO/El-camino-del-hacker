# SSTI (Server-Side Template Injection)

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #SSTI #exploiting

***

## Definición

> **SSTI (Server-Side Template Injection)** es una \[\[Vulnerabilidades web]] de seguridad en aplicaciones web donde un atacante puede insertar código malicioso en las plantillas del lado del servidor, lo que lleva a la ejecución de código en el servidor. Esto puede permitir que el atacante acceda a datos confidenciales, modifique la aplicación y cause daño.
>
> Las plantillas de servidor son archivos que contienen código que se utiliza para generar **contenido dinámico** en una aplicación web. Los atacantes pueden aprovechar una vulnerabilidad de SSTI para inyectar código malicioso en una plantilla de servidor, lo que les permite ejecutar comandos en el servidor y obtener acceso no autorizado tanto a la aplicación web como a posibles datos sensibles.

***

## Detección de vulnerabilidad

Para detectar la vulnerabilidad debemos realizar una \[\[identificación\_de\_tecnologías]] que utiliza la página web. Una vez conozcamos como está construía la página, podemos examinar las distintas tecnologías para ver si alguna es vulnerable. Por ejemplo si se detecta que **Flask** está en uso, como utiliza el motor de plantillas **Jinja2**, puede ser que sea vulnerable.

Si tenemos el control de algún dato que introduzcamos y sea mostrado por pantalla, podemos comprobar si es vulnerable a **SSTI** de distintas maneras, por ejemplo podemos introducir ${1+1}, y si nos devuelve el resultado de la operación, muy posiblemente sea vulnerable.

***

## Explotación

Una vez detectada una página web que es vulnerable a **SSTI**, podemos intentar distintos \[\[Payload]] para intentar provocar un \[\[RCE (Remote Command Execution)]], listar archivos del propio servidor o incluso más cosas. Una página web muy interesante donde podemos ver distintos **payloads** para distintas herramientas web que se estén utilizando es [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection).
