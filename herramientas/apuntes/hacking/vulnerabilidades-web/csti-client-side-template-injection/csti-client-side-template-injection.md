---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #CSTI #exploiting 
--------------
# Definición

> La **CSTI (Client-Side Template Injection)** es una [[Vulnerabilidades web]] en aplicaciones web que emplean plantillas en el lado del cliente. Permite a atacantes insertar código malicioso en estas plantillas, que son luego interpretadas por el navegador del usuario, potencialmente resultando en ejecución de código no autorizado en el contexto del cliente. Esto puede llevar a divulgación de información, manipulación de contenido y otros riesgos. Para prevenir **CSTI**, es esencial aplicar prácticas seguras de desarrollo web y validar adecuadamente los datos de entrada.
> 
> Es muy parecido a [[SSTI (Server-Side Template Injection)]], pero se diferencia en que el **CSTI** trata de atentar contra el propio navegador del usuario para obtener d**atos privilegiados** como la cookie de sesión (pudiendo acabar en [[Cookie Hijacking]]) o cualquier cosa que controles desde el archivo en **javascript** teniendo mucha relación con [[XSS (Cross-Site Scripting)]], mientras que el **SSTI** es más contra el servidor que está alojando el servicio con el objetivo de que tú llegues a vulnerarlo, leer archivos de la máquina y poder ejecutar comandos a nivel del sistema.
-----------
# Detección de vulnerabilidad

Igualmente como hacíamos con la vulnerabilidad de **SSTI**, tenemos que realizar una [[identificación_de_tecnologías]] para saber a lo que nos enfrentamos. Si hay algún campo en el que podamos introducir algún tipo de texto, podemos probar también a ejecutar las típicas operatorias como {{1+1}}. 

---------
# Explotación

Al igual que con **SSTI** debemos inyectar diferentes [[Payload]] que nos ayuden a convertir el ataque en un **XSS**, podemos buscar algunos payloads por internet, o en la página de [hacktricks](https://book.hacktricks.xyz/welcome/readme). 

Explotando algunas herramientas como **AngularJS**, intentando realizar un *alert*, puede ser que nos falle a la hora de introducir texto, ya que no se interpretan bien las comillas, por lo que tenemos que hacer uso de la función *String.fromCharCode()* de **JavasScript** para transformar los códigos decimales de cada letra y separarlo por comas. Sería interesante crear algún script malicioso en **JavasScript** para conseguir información o incluso para conseguir una [[Reverse shell]].

--------
# Ejemplos prácticos

[blabla1337](https://github.com/blabla1337/skf-labs)