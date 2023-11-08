# Abuso de APIs

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #APIs #exploiting

***

## Definición

> La vulnerabilidad de **abuso de APIs** se refiere a la explotación indebida de interfaces de programación de aplicaciones (**APIs**) para llevar a cabo acciones maliciosas o no autorizadas en una aplicación o sistema. Esto puede incluir el uso excesivo de recursos, la obtención de información sensible o la manipulación de datos, lo que potencialmente puede comprometer la seguridad y la integridad de una aplicación o servicio en línea.
>
> Una **API** es un conjunto de reglas y protocolos que permite que diferentes aplicaciones o componentes de software se comuniquen entre sí. Una **API** define cómo un programa o sistema debe **interactuar** con otro, proporcionando un conjunto de **funciones**, **procedimientos** y **métodos** que los desarrolladores pueden utilizar para acceder y utilizar ciertas características o datos de una aplicación o servicio de software sin necesidad de conocer los detalles internos de su implementación.
>
> Algunos endpoints de una API pueden aceptar diferentes métodos de solicitud, como GET, POST, PUT, DELETE, etc. Los atacantes pueden utilizar herramientas de fuzzing para enviar una gran cantidad de solicitudes a un endpoint en busca de vulnerabilidades. Por ejemplo, un atacante podría enviar solicitudes GET a un endpoint para enumerar todos los recursos disponibles, o enviar solicitudes POST para agregar o modificar datos.
>
> Este tipo de ataques pueden desencadenar en otras vías de ataque, como por ejemplo \[\[SQL Injection]] o \[\[CSRF (Cross-Site Request Forgery)]].
>
> Para evitar el abuso de APIs, los desarrolladores deben asegurarse de que la API esté diseñada de manera segura y que se validen y autentiquen adecuadamente todas las solicitudes entrantes. También es importante utilizar cifrado y autenticación fuertes para proteger los datos que se transmiten a través de la API.

***

## Detección de vulnerabilidad

* **Cambiar la versión de la API**: Si nos encontramos con un endpoint, cuya url nos indica que se está utilizando una versión, podemos probar con versiones anteriores, por si estuvieran públicas. Dichas versiones podrían tener fallos de seguridad.
* **Cambiar el método de la petición**: Puede darse el caso que una petición se pueda realizar con distintos métodos, por ejemplo que nos hayamos encontrado una petición por GET, pero que también pueda aceptar un POST para poder enviar información o cualquier otro. Para ello podemos utilizar \[\[Ffuf]] para enumerar los métodos disponibles:

```
ffuf -u http://<IP>/ruta/api/ruta/endpoint -w /Seclists/Fuzzing/http-request-methods.txt -X FUZZ
```

***

## Explotación

Podemos hacer uso de \[\[Postman]] para almacenar todas las peticiones relacionadas con las **APIs** de una manera más cómoda y permitirnos probar y depurar las mismas. Las peticiones las podemos obtener de distintas formas, como con \[\[Burpsuite]] o con el propio **firefox** en el apartado de _Network_ y seleccionando únicamente las peticiones **XHR**. Como ejemplo se deja la siguiente foto:

!\[\[Ejemplo para obtener peticiones a la api.png]]

De aquí en adelante, se tendrá que encontrar posibles vulnerabilidades (como las mencionadas anteriormente) en las APIs que encontremos.

***

## Ejemplos prácticos

* **crAPI**: [https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)
