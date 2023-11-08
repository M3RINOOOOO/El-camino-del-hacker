# PadBuster

***

* Tags: #herramientas #web #Padding\_oracle #automático

***

## Definición

> **PadBuster** es una herramienta diseñada para automatizar el proceso de descifrado de mensajes cifrados en modo **CBC** que utilizan relleno **PKCS #7**. La herramienta permite a los atacantes enviar peticiones HTTP con **rellenos maliciosos** para determinar si el relleno es válido o no. De esta forma, los atacantes pueden adivinar el contenido cifrado y descifrar todo el mensaje.

***

## Uso

Para hacer uso de la herramienta, una vez descargada, para **descifrar** un texto, podemos ejecutar el siguiente comando en **bash**:

```bash
padbuster <URL> <TEXTO_ENCRIPTADO> <TAMAÑO_BLOQUE> -parametros
```

Donde en el **tamaño de bloques** tenemos que ir probando con números múltiplos de 8 hasta que funcione.

De manera contraria, para **cifrar** un texto de la misma manera que se está empleando en el servidor web, podemos hacer uso del siguiente comando:

```bash
padbuster <URL> <TEXTO_ENCRIPTADO> <TAMAÑO_BLOQUE> -parametros -plaintext '<TEXTO_A_CIFRAR>'
```

Y la herramienta nos pedirá que elijamos un ID que coincida con la **condición de error**, y debemos seleccionar la que incluya \*\* .

### Parámetros

En la parte de parámetros debemos introducir donde hemos encontrado dicho texto cifrado y queremos descifrar, así como distintos campos que sean necesarios para el acceso a la página web. Para ver todos ellos ver el manual de la herramienta.

Si estuviéramos trabajando con una **cookie**, podemos utilizar el siguiente comando:

```bash
padbuster <URL> <TEXTO_ENCRIPTADO> <TAMAÑO_BLOQUE> -cookies '<COOKIE>'
```
