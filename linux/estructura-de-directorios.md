# Estructura de directorios

La familiaridad con la estructura de directorios de Linux proporciona una base sólida para realizar tareas de hacking ético de manera efectiva y segura. Facilita la orientación en el sistema, la ubicación de herramientas, la personalización, la automatización de tareas y la seguridad del sistema.

Un directorio es lo equivalente que conocemos como carpeta, es decir, puede tener dentro otros archivos, directorios inclusive.&#x20;

El **directorio raíz** `/` es el directorio padre de todos los demás directorios, es decir, contiene todos los directorios del sistema. Los demás directorios principales que tiene un sistema linux son los siguientes:

### /bin

&#x20;El directorio /bin es un directorio estático y compartible en el que se almacenan archivos binarios/ejecutables necesarios para el funcionamiento del sistema. Estos archivos binarios los pueden usar la totalidad de usuarios del sistema operativo.

### /boot

Es un directorio estático no compartible que contiene la totalidad de archivos necesarios para el arranque del ordenador excepto los archivos de configuración. Algunos de los archivos indispensables para el arranque del sistema que acostumbra a almacenar el directorio /boot son el kernel y el gestor de arranque Grub.

### /dev

El sistema operativo Gnu-Linux trata los dispositivos de hardware como si fueran un archivo. Estos archivos que representan nuestros dispositivos de hardware se hallan almacenados en el directorio /dev.

### /etc&#x20;

El directorio /etc es un directorio estático que contiene los archivos de configuración del sistema operativo. Este directorio también contiene archivos de configuración para controlar el funcionamiento de diversos programas.

Algunos de los archivos de configuración de la carpeta /etc pueden ser sustituidos o complementados por archivos de configuración ubicados en nuestra carpeta personal /home.

### /home&#x20;

El directorio /home se trata de un directorio variable y compartible. Este directorio está destinado a alojar la totalidad de archivos personales de los distintos usuarios del sistema operativo a excepción del usuario root. Algunos de los archivos personales almacenados en la carpeta /home son fotografías, documentos de ofimática, vídeos, etc.

### /lib&#x20;

El directorio /lib es un directorio estático y que puede ser compartible. Este directorio contiene bibliotecas compartidas que son necesarias para arrancar los ejecutables que se almacenan en los directorios /bin y /sbin.

### /mnt&#x20;

El directorio /mnt tiene la finalidad de albergar los puntos de montaje de los distintos dispositivos de almacenamiento como por ejemplo discos duros externos, particiones de unidades externas, etc.

### /media&#x20;

La función del directorio /media es similar a la del directorio /mnt. Este directorio contiene los puntos de montaje de los medios extraíbles de almacenamiento como por ejemplo memorias USB, lectores de CD-ROM, unidades de disquete, etc.

### /opt&#x20;

El contenido almacenado en el directorio /opt es estático y compartible. La función de este directorio es almacenar programas que no vienen con nuestro sistema operativo como por ejemplo Spotify, Google-earth, Google Chrome, Teamviewer, etc.

### /proc&#x20;

El directorio /proc se trata de un sistema de archivos virtual. Este sistema de archivos virtual nos proporciona información acerca de los distintos procesos y aplicaciones que se están ejecutando en nuestro sistema operativo.

### /root&#x20;

El directorio /root se trata de un directorio variable no compartible. El directorio /root es el directorio /home del administrador del sistema (usuario root).

### /sbin&#x20;

El directorio /sbin se trata de un directorio estático y compartible. Su función es similar al directorio /bin, pero a diferencia del directorio /bin, el directorio /sbin almacena archivos binarios/ejecutables que solo puede ejecutar el usuario root o administrador del sistema.

### /srv&#x20;

El directorio /srv se usa para almacenar directorios y datos que usan ciertos servidores que podamos tener instalados en nuestro ordenador.

### /tmp&#x20;

El directorio /tmp es donde se crean y se almacenan los archivos temporales y las variables para que los programas puedan funcionar de forma adecuada.

### /usr&#x20;

El directorio /usr es un directorio compartido y estático. Este directorio es el que contiene la gran mayoría de programas instalados en nuestro sistema operativo.

Todo el contenido almacenado en la carpeta /usr es accesible para todos los usuarios y su contenido es solo de lectura.

### /var&#x20;

El directorio /var contiene archivos de datos variables y temporales como por ejemplo los registros del sistema (logs), los registros de programas que tenemos instalados en el sistema operativo, archivos spool, etc.

La principal función del directorio /var es la detectar problemas y solucionarlos. Se recomienda ubicar el directorio /var en una partición propia, y en caso de no ser posible es recomendable ubicarlo fuera de la partición raíz.

### /sys&#x20;

Directorio que contiene información similar a la del directorio /proc. Dentro de esta carpeta podemos encontrar información estructurada y jerárquica acerca del kernel de nuestro equipo, de nuestras particiones y sistemas de archivo, de nuestros drivers, etc.

### /lost-found&#x20;

Directorio que se crea en las particiones de disco con un sistema de archivos ext después ejecutar herramientas para restaurar y recuperar el sistema operativo como por ejemplo fsch.

Si nuestro sistema no ha presentado problemas este directorio estará completamente vacío. En el caso que hayan habido problemas este directorio contendrá ficheros y directorios que han sido recuperados tras la caída del sistema operativo.
