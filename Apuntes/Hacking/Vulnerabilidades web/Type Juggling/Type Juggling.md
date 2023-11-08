# Type Juggling

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #Type\_Juggling

***

## Definición

> **Type juggling** es la manipulación de valores de diferentes **tipos de datos** como si fueran de otro tipo en programación. Puede llevar a resultados inesperados o ser explotado en ataques de seguridad al permitir operaciones entre tipos incompatibles.
>
> En los distintos lenguajes de programación se utilizan **tipos de datos** para clasificar la información almacenada en una variable, utilizando los mismos para realizar operaciones aritméticas, comparaciones y más. Si no se validan adecuadamente los tipos de datos que se proporciona al usuario, un atacante puede explotar vulnerabilidades en dichos programas, manipulando los datos de entrada para cambiar el tipo de dato de una variable.
>
> Por ejemplo, el atacante podría proporcionar una cadena de caracteres parecida a un número entero, pero en realidad no lo es, de manera que si no se comprueba correctamente el tipo de dato, se podrían realizar operaciones matemáticas con dicha variable y obtener resultados inesperados.
>
> Un ejemplo común de cómo se puede utilizar un ataque de **Type Juggling** para burlar la autenticación es en un sistema que utiliza comparaciones de cadena para verificar las contraseñas de los usuarios. En lugar de proporcionar una contraseña válida, el atacante podría proporcionar una cadena que se parece a una contraseña válida, pero que en realidad no lo es.
>
> En **PHP**, una cadena que comienza con un número se convierte automáticamente en un número si se utiliza en una comparación numérica. Por lo tanto, si el atacante proporciona una cadena que comienza con el número **cero** (**0**), como “**00123**“, el programa la convertirá en el número entero **123**.

***

## Detección de vulnerabilidad

Podríamos utilizar alguno de los siguientes métodos para detectar la \[\[Vulnerabilidades web]]:

* **Entradas de datos:** cuando veamos una entrada de datos como en formularios y campos de entrada, podemos tratar de introducir números en los campos de texto donde se esperan cadenas o cadenas con carácteres especiales. Si notamos resultados inesperados, errores o comportamientos extraños, podría ser indicar la existencia de la vulnerabilidad.
* **Observación de la URL:** Si los valores introducidos en los campos de entrada se reflejan en la URL, podemos observar cómo cambian cuando se manipulan los datos. Esto podría darnos pistas sobre cómo la página web maneja y convierte los tipos.
* **Valores extremos:** Podemos probar a enviar números muy grandes o muy pequeños, o incluso cadenas que contengan una combinación de números y letras.

***

## Explotación

Podemos intentar explotar esta vulnerabilidad de distintas maneras. Por ejemplo, si nos encontrásemos ante un panel de autentificación en el que se va a realizar por detrás una **comparación** para ver si dichas credenciales son válidas podríamos intentar **convertir el tipo** de la variable contraseña a un **string**, de manera que puede burlar la comparación que se realiza. Suponiendo que se está realizando una petición **POST**, con campos usuario y password, podemos interceptar la petición con \[\[Burpsuite]] e intentar enviar lo siguiente:

```
usuario=admin&password=asdasdasda     #Petición original
usuario=admin&password[]=             #Petición con type juggling
```

Otro caso que se puede ocasionar es que la contraseña del usuario empiece por _0e_, ya sea porque se está aplicando una encriptación en **md5** o porque simplemente es así la contraseña. **PHP** interpretará dicha contraseña como un 0, ya que se toma como 0 elevado a lo que sea que será 0, de manera que podemos tratar de enviar una contraseña que también empiece por _0e_, de manera que se realizará una operación `0==0` de manera que accederemos como dicho usuario. Si se está cifrando también nuestra contraseña en **md5**, podemos tratar de enviar una cadena que cifrada empiece por _0e_. Algunos de estas cadenas pueden ser encontradas en la siguiente página web: [hackplayers](https://www.hackplayers.com/2018/03/hashes-magicos-en-php-type-jugling.html)

Esto sucedería en el caso en el que se esté realizando la comprobación de contraseñas con un doble igual en vez de un triple, lo que puede generar este tipo de vulnerabilidades.
