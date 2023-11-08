---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #WebDAV
--------------
# Definición

> **WebDAV** (**Web Distributed Authoring and Versioning**) es una extensión del protocolo HTTP que permite a los usuarios colaborar y gestionar archivos y carpetas directamente en servidores web, facilitando la edición y control de versiones de contenido en línea a través de una conexión segura.
> 
> Enumerar un servidor **WebDAV** implica buscar y listar los recursos disponibles en un servidor que utiliza el protocolo **WebDAV**. Esto se hace mediante la identificación y enumeración de directorios, archivos y otros recursos web disponibles en el servidor. Se pueden utilizar herramientas de enumeración de **WebDAV** para buscar recursos protegidos en el servidor, como archivos de configuración, contraseñas y otros datos confidenciales.
> 
> Esta parte de reconocimiento intentar identificar las **extensiones de archivo** permitidas en el servidor. Una vez detectadas las extensiones de archivo permitidas, los atacantes pueden aprovecharse de esto para cargar y ejecutar archivos que contengan código malicioso.

-----------
# Detección de uso

Para detectar el sudo del servicio de **WebDAV**, podemos utilizar la herramienta [[whatweb]]:

```bash
whatweb <URL>
```

Y sabremos que se está haciendo uso si en la respuesta observamos algo parecido a lo siguiente:

![[Pasted image 20231024202553.png]]

---------
# Enumeración y explotación

Para comenzar la enumeración, es necesario disponer de unas credenciales válidas, que inicialmente no las poseeremos. Podemos entonces hacer uso de la herramienta [[Davtest]] junto con fuerza bruta de la siguiente manera:

```bash
cat /ruta/a/diccionario | while read password; do response=$(davtest -url <URL> -auth <USER>:<PASSWORD> 2>&1 | grep -i succeed); if [ $response ]; then echo "[+] La contraseña es $password"; break; if; done
```

Hemos supuesto que el usuario es *admin*, pero también podemos hacerle fuerza bruta. Este comando nos mostrará por pantalla distintas extensiones permitidas para la subida y ejecución de archivos.

Una vez conozcamos las credenciales válidas, podemos hacer uso de la herramienta [[Cadaver]] para acceder al servicio:

```
cadaver <URL>
```

Y podremos ejecutar comandos como listado de archivo, creación de los mismos e incluso subir los nuestros propios con el comando ```put```. Por lo tanto si en el servidor se interpreta *php*, podemos subir un script malicioso para poder ganar acceso al sistema o lo que nos interese, terminando en un [[RFI (Remote File Inclusion)]].

---------------
# Ejemplos prácticos

- **WebDav**: [https://hub.docker.com/r/bytemark/webdav](https://hub.docker.com/r/bytemark/webdav)