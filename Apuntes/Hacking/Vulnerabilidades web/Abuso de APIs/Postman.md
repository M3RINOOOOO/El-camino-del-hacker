----------------------
- Tags: #herramientas #APIs  #exploiting 
-----------------------
**Postman** es una herramienta muy popular utilizada para probar y depurar APIs. Con Postman, los desarrolladores pueden enviar solicitudes a diferentes endpoints y ver las respuestas para verificar que la API está funcionando correctamente. Sin embargo, los atacantes también pueden utilizar Postman para explorar los endpoints de una API en busca de vulnerabilidades y debilidades de seguridad. 

Para comenzar con un proyecto, pulsamos sobre *Create Collection*, y le asignamos el nombre que nos plazca. 

Para añadir las distintas peticiones que queremos probar sobre la página web, pulsamos en *New* -> *HTTP Request*. Dependiendo de qué tipo de petición estemos utilizando, habrá que indicarla (POST, GET...). Si se estuviera mandando información adicional en la petición, la debemos especificar en **Postman** para que funcione correctamente. Si por ejemplo se está mandado una petición de autentificación con un json con usuario y contraseña, debemos introducirlo como sigue: 

![[Pasted image 20231008174112.png]]

Es conveniente guardar las peticiones en la colección creada anteriormente, para poder hacer uso de variables que serán muy útiles. Por ejemplo, tras habernos logeado, la aplicación web nos habría devuelto un token necesario para poder realizar otras peticiones. Para ello, hacemos lo siguiente, dentro de la colección:

![[Pasted image 20231008174533.png]]

Como necesitamos el token para la autorización, debemos especificarlo, donde podemos hacer uso de la variable recién creada:

![[Pasted image 20231008174735.png]]

Y así en todas las solicitudes de esta colección, se creará una cabecera con dicha autorización necesaria.