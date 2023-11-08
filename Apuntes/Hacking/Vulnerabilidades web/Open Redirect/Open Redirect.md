---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #Open_redirect
--------------
# Definición

> Un **open redirect** es una vulnerabilidad de seguridad en aplicaciones web que ocurre cuando un atacante explota un enlace o una URL para redirigir al usuario a una ubicación externa no confiable. Esto puede utilizarse para engañar a los usuarios y dirigirlos a sitios maliciosos, lo que potencialmente facilita ataques de [[Phishing]] o malware. 
> 
> La vulnerabilidad se produce cuando la aplicación no valida adecuadamente las URL de redirección, permitiendo que los atacantes manipulen la dirección de destino y realicen redirecciones no autorizadas.
> 
> Por ejemplo, supongamos que una aplicación web utiliza un parámetro de redireccionamiento en una URL para dirigir al usuario a una página externa después de que se haya autenticado. Si esta URL no valida adecuadamente el parámetro de redireccionamiento y permite a los atacantes modificarlo, los atacantes pueden dirigir al usuario a un sitio web malicioso, en lugar del sitio web legítimo.

-----------
# Detección de vulnerabilidad

- **Manipulación de parámetros de URL**: Intenta modificar manualmente los parámetros de URL que implican redirecciones, como `?redirect=URL`. Cambia la URL de destino por una no autorizada y observa si la aplicación web redirige sin validar correctamente la URL.
- **Pruebas de redirección cruzada**: Si la aplicación web permite redirecciones a dominios externos, intenta redireccionar a un dominio controlado por ti.
- **Exploración de URL de redirección**: Analiza todas las URLs de redirección utilizadas en la aplicación. Presta especial atención a aquellas que aceptan parámetros de usuario o entradas de datos.

---------
# Explotación

Suponiendo que tenemos el caso en el que hemos encontrado un parámetro que implica redirecciones, podemos interceptar la petición con [[Burpsuite]] y modificar la url a la que se redirige. 

Puede ser que la aplicación no permita ciertos caracteres en la dirección a la que redirige, pero podemos intentar url-encodear dichos caracteres, y volver a url-encodear los %. Si los caracteres no permitidos son "/", es importante saber que si es una URL https, no hacen falta las dobles barras.


Esta vulnerabilidad como tal no es muy crítica, pero si la concatenamos con otras vulnerabilidades, como [[XSS (Cross-Site Scripting)]] o incluso [[Phishing]], puede ser muy interesante. 

---------------
# Ejemplos prácticos

- **Open Redirect 1**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection)
- **Open Redirect 2**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder)
- **Open Redirect 3**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder2](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder2)

Un repositorio muy chulo que utiliza esta vulnerabilidad para crear una botnet es: [ufonet](https://github.com/epsylon/ufonet)