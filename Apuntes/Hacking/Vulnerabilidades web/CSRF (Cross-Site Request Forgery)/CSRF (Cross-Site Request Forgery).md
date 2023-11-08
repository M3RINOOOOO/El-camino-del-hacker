---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #CSRF #exploiting 
--------------
# Definición

> **CSRF (Cross-Site Request Forgery)**, también conocido como **XSRF**, es una [[Vulnerabilidades web]] en la que un atacante engaña a un usuario autenticado para realizar acciones no deseadas en un sitio sin su conocimiento, comprometiendo su seguridad y datos. En cierta parte, es una vulnerabilidad parecida a un [[XSS (Cross-Site Scripting)]], pero no llega a ser lo mismo.
> 
> En un ataque de este estilo, el atacante engaña a la víctima para que haga clic en un **enlace malicioso** o **visite una página web maliciosa**, que puede contener por ejemplo, un formulario que envía una solicitud HTTP al sitio web del banco para transferir fondos de la cuenta bancaria del usuario a la cuenta del atacante. Si el usuario hace clic en el botón de envío sin sabe que está ocurriendo, el ataque **CSRF** será exitoso.
>
> Aparte del ejemplo mencionado, puede haber una amplia variedad de acciones no deseadas, como la modificación de la información de la cuenta, elminación de datos y mucho más.
> 
> Esta vulnerabilidad se evita mediante medidas como tokens de solicitud y verificaciones de origen, aunque también se pueden hacer cosas con esto.

--------
# Detección de vulnerabilidad

Para la detección podemos seguir una especie de metodología, para intentar descubrir si una página web es vulnerable:

1.- **Identificar puntos de acción crítica**: Podemos examinar en la página web las distintas acciones que podrían tener un cambio significativo en la seguridad, como un cambio de contraseña, modificación de datos personales o realizar compras.

2.- **Analizar las solicitudes y respuestas**: Con [[Burpsuite]], podemos analizar el tráfico de dichas peticiones, de manera que si podemos transformar la petición de POST a GET y no cambia el resultado, puede ser que exista la vulnerabilidad. Todo el rato estaremos jugando con dicha petición, será el enlace malicioso que enviaremos a la víctima.

3.- **Modificar solicitudes**: Podemos intentar modificar dichas peticiones para cambiar parámetro, especialmente los que están relacionados con identificación y autorización, viendo si podemos influir en las acciones realizadas por la aplicación.

4.- **Crear solicitudes falsificadas**: Con dichas solicitudes, podemos intentar falsificarlas para que puedan ser ejecutadas por un atacante. Esto puede involucrar escribir código HTML y JavaScript para simular una solicitud que realice una acción maliciosa.

5.- **Observar el impacto**: Envía las solicitudes falsificadas o modificadas al servidor y observa si las acciones se realizan sin la interacción directa del usuario, como por ejemplo si el nombre de usuario ha sido cambiado.

--------
# Explotación

Suponiendo que para realizar el cambio de una nombre, contraseña o cualquier otro campo mandamos una petición GET, pero podemos transformarla a una petición POST sin que varíe la utilidad de la misma solicitud, podemos intentar a hacer cosas. 

Suponiendo que la petición haga alusión al id del usuario del que se va a cambiar el nombre, contraseña o cualquier otra cosa, podemos intentar simplemente cambiar el número de id a otro que sepamos que exista de otro usuario, para ver si funciona el cambio. Si no funciona, puede ser porque se esté utilizando una **cookie de sesión** por lo que necesitamos la misma para realizar [[Cookie Hijacking]], o intentar hacer con [[Ingeniería social]] o [[Phishing]] que la víctima visite el enlace que crearemos.

Si hay alguna manera que nos permita enviar a la víctima código html, ya sea por un servicio de mensajería, o simplemente el hecho de visualizar una biografía, podemos insertar la **URL maliciosa**, de manera que si la víctima lo ve, se tensa la cosa. Por ejemplo, podríamos intentar publicar o enviar algo parecido a lo siguiente:

```html
<h1>IMPORTANTE!!!!</h1> 
<img src="http://<PÁGINA_WEB>/<URL MALICIOSA>" alt="ES MUY IMPORTANTE" width="1" height="1"/>
```

Y cuando el otro usuario vea la "imagen" introducida, que realmente es la **URL maliciosa**, ya se habrá cambiado su contraseña, usuario, realizado cualquier compra... La anchura y altura de la imagen la establecemos a 1, para que la víctima no sospeche al no ver la foto.

--------
# Ejemplos prácticos

[Lab Setup]([https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip](https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip))