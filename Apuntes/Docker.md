Para crear una nueva red en Docker, podemos utilizar el siguiente comando:

➜ `docker network create --subnet=<subnet> <nombre_de_red>`

Donde:

- **subnet**: es la dirección IP de la subred de la red que estamos creando. Es importante tener en cuenta que esta dirección IP debe ser única y no debe entrar en conflicto con otras redes o subredes existentes en nuestro sistema.
- **nombre_de_red**: es el nombre que le damos a la red que estamos creando.

Además de los campos mencionados anteriormente, también podemos utilizar la opción ‘**–driver**‘ en el comando ‘docker network create’ para especificar el controlador de red que deseamos utilizar.

Por ejemplo, si queremos crear una red de tipo “**bridge**“, podemos utilizar el siguiente comando:

➜ `docker network create --subnet=<subnet> --driver=bridge <nombre_de_red>`

En este caso, estamos utilizando la opción ‘**–driver=bridge**‘ para indicar que deseamos crear una red de tipo “**bridge**“. La opción –driver nos permite especificar el controlador de red que deseamos utilizar, que puede ser “**bridge**“, “**overlay**“, “**macvlan**“, “**ipvlan**” u otro controlador compatible con Docker.




PARA CLEAN DOCKER (Enumeración y explotación de WebDAV)