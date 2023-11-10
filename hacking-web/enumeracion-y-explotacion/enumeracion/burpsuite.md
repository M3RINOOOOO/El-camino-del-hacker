# Burpsuite

***

* Tags: #herramientas #web #reconocimiento #enumeración

***

## Definición

> **BurpSuite** es una aplicación de seguridad de software utilizada para pruebas de penetración de aplicaciones web. Está diseñada para identificar vulnerabilidades en sitios web y aplicaciones mediante pruebas automatizadas y manuales.

***

## Uso

Para iniciar **BurpSuite** podemos ejecutar el siguiente comando con \[\[Control de flujo]] para que sea una tarea independiente:

```bash
burpsuite &> /dev/null & disown
```

Haciendo doble click en la pestaña superior izquierda podemos cambiar el nombre a la pestaña actual de trabajo.

***

## Herramientas

### Proxy

> Actúa como un **man-in-the-middle** entre el navegador web y el servidor web, de manera que podemos interceptar y modificar las solicitudes y respuestas HTTP y HTTPS enviadas entre el navegador y el servidor.

En la ventana de **Intercept** podemos (des)habilitar la interceptación de los mensajes, habiendo configurado el proxy previamente desde el navegador.

Para ver la respuesta del mensaje interceptado, sin necesidad del **repetidor**, pulsamos click derecho y _Do intercept/Response to this request_, de manera que podemos ver la respuesta, y si se nos está realizando un redirigimiento, podemos cambiar el estado de la respuesta y tensar alguna que otra cosa (mirar el apartado de _Options/Match and Replace_.

Teniendo un mensaje interceptado podemos pulsar _ctrl+r_ para redirigirlo al **repetidor**, que nos permite modificar el mensaje a nuestro gusto y poder enviarlo con facilidad, además de poder ir viendo interactivamente las respuestas del servidor. Pulsando _ctrl+espacio_ enviamos las solicitudes más rápido.

Teniendo un mensaje interceptado podemos pulsar _ctrl+i_ para redirigirlo al apartado de **intruder**, que nos permite realizar distintos ataques con el mensaje interceptado.

En la ventana de **HTTP history**, podemos ver el historial de las peticiones que hemos ido realizando. Si no queremos que nos guarde peticiones hacia diferentes dominios que estén fuera del alcance, en la parte de _Miscellaneous_, seleccionamos la última opción, y tenemos que definir cual va a ser el alcance del ataque (desde la pestaña de _Target/Scope_). En _Target/Site map_ podemos ver los diferentes dominios que se han ido visitando en la página web.

### Scanner

> Realiza diferentes pruebas de vulnerabilidades automatizada que se utiliza para identificar vulnerabilidades en aplicaciones web como \[\[SQL Injection]] y \[\[XSS (Cross-Site Scripting)]].

### Repeater

> Permite a los usuarios reenviar y repetir solicitudes HTTP y HTTPS, permitiendo probar diferentes entradas y verificar la respuesta del servidor. También es útil para la identificación de vulnerabilidades ya que podemos cambiar los valores y detectar respuestas inesperadas.

### Intruder

> Se utiliza para automatizar ataques de fuerza bruta. El usuario es capaz de definir diferentes \[\[Payload]] para diferentes partes de la solicitud, como la URL, cuerpo de solicitud y cabeceras, y ver examinar las respuestas para identificar vulnerabilidades.

Tiene diferentes tipos de ataques:

*   Sniper: Seleccionamos una cadena del mensaje, le damos al botón de la derecha que pone Add $, indicando ese campo como payload para ir modificandolo y ver las diferentes respuestas (Para poner más de uno, ver los demás ataques)

    En el apartado Payloads

    Payload Sets especificamos el número del payload con el que queramos trabajar. Payload Options (Simple list): Podemos ir añadiendo palabras a modo de diccionario para que sea lo que va probando el payload

    En el apartado Options

    Start attack en el botón de arriba a la derecha para que empiece el ataque Grep-Extract dandole a add podemos seleccionar toda la cadena que queremos ir analizando (crea una expresión regular)
* Battering ram: Es exactamente igual que el modo _sniper_, pero ahora todos los payloads seleccionados tendrán el mismo valor conínuamente.
* Pitchfork: Exactamente igual que el modo _sniper_, pero con múltiples payloads que van a ir seleccionando la i-ésima fila y juntandolo en la misma petición.
* Cluster bomb: Se trabaja igual que con el modo de _Sniper_, peropodemos añadir múltiples payloads (Se realiza combinación).

### Comparer

> Se utiliza para compara dos peticiones HTTP o HTTPS, permitiendo examinar las diferencias entre las solicitudes y respuestas.

Para mandar algo al comparador hacemos click derecho sobre la petición, y sale la opción de mandar la solicitud o la respuesta. Se puede utilizar una comparativa tanto por palabras como por bytes (por palabras es más recomendable).

### Extender

> En este apartado podemos instalar en **BurpSuite** distintos plugins que serán útiles en un futuro.
