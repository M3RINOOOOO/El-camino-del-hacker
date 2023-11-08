# Prototype Pollution

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #LFI

***

## Definición

> La **prototype pollution** es una vulnerabilidad de seguridad en la que los atacantes manipulan los prototipos de objetos en lenguajes como **JavaScript** para modificar o agregar propiedades de manera maliciosa. Esta técnica de ataque se utiliza para modificar la propiedad “**prototype**” de un objeto en una aplicación web, pudiendo llevar a comportamientos no deseados o a riesgos de seguridad al permitir la manipulación no autorizada de objetos y datos en una aplicación.
>
> En **JavaScript**, la propiedad **prototype** se utiliza para definir las propiedades y métodos de un objeto. Los atacantes pueden explotar esta característica de JavaScript para modificar las propiedades y métodos de un objeto y tomar el control de la aplicación.
>
> Para modificar la propiedad **protoype** de un objeto en una aplicación web, se pueden manipular los datos que se envían a través de formularios o solicitudes **AJAX**.

***

## Detección de vulnerabilidad

* **Manipulación de parámetros**: Experimenta con la manipulación de parámetros en las URL o en las solicitudes HTTP enviadas por la página web.
* **Pruebas de inyección de código**: En campos de entrada que reflejen datos en la página web, como campos de búsqueda o áreas de comentarios. Se puede intentar algo como `"><img src=x onerror=alert(1)>` para ver si se ejecuta código.

***

## Explotación

Un **prototipo** como tal nos define las propiedades de ciertos atributos en el caso de que no existan, por ejemplo podemos tener una variable en **JavaScript** de la siguiente manera, con un prototipo inyectado:

```JavaScript
var body = JSON.parse('{"name":"Pepito","__proto__":{"isAdmin": true}}'); 
```

De manera que si se intentase acceder a la propiedad _isAdmin_ y no existiese, nos devuelve true. También se puede hacer a la hora de asignar un nuevo atributo:

```JavaScript
var persona = {}
var admin = {}

persona.__proto__.edad = 18

console.log(admin.edad)   //nos devuelve 18, aunque admin no tiene el atributo edad
```

Traslado esto ahora a un caso práctico, suponiendo que tenemos campos de entrada vulnerables, interceptamos la petición con \[\[Burpsuite]], y podemos intentar añadir un prototipo al objeto enviado en formato _json_. En el siguiente ejemplo se establece una variable _admin_ a true, de manera que cuando se quiera acceder al atributo _admin_ de un objeto que no esté definido, tomará el valor true:

!\[\[Pasted image 20231009195728.png]]

Será mucho más fácil el hecho de tener el código fuente para examinarlo y ver los atributos que se están consultando y a los que le podamos encontrar esta vulnerabilidad, sino deberíamos ir un poco a ciegas, probando diferentes nombres para los atributos con un poco de intuición.

***

## Ejemplos prácticos

* **SKF-LABS**: [https://github.com/blabla1337/skf-labs](https://github.com/blabla1337/skf-labs)
