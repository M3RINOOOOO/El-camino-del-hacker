# Análisis de distintos tipos de archivos
## Comprimidos (ZIP, TGZ)
### Visualizar el contenido

Para visualizar el contenido de un arhcivo podemos ejecutar distintos comandos:

```
binwalk file
7z l file
```
### Aplicar fuerza bruta a un zip

Primero comenzamos con traducir el zip a un hash legible por [[john]] y lo copiamos a un **hash.txt**:

```
zip2john arhivo.zip > hash.txt
```

Una vez hayamos conseguido el hash, se lo podemos proporcionar a **john** de la siguiente manera:

```
john --format=pkzip --wordlist=rockyou.txt hash.txt
```

Y si la contraseña está en el diccionario, se mostrará por consola, y ya podremos conseguir los archivos comprimidos:

```
7z x -p<PASSWORD> zip.zip
```
## Formato de imágenes (JPG, GIF, BMP, PNG)

### Observar los metadatos de una imagen

Para poder visualizar los **metadatos** de una imagen, podemos hacer uso de la herramienta [[ExifTool]]:

```
exiftool file.png
```

En dichos metadatos, puede ser que encontremos información valiosa.

### Esteganografía

La **esteganografía** es una técnica de ocultar información dentro de otro tipo de contenido. El objetivo de la **esteganografía** es que terceros no puedan detectar fácilmente la existencia de la información oculta.

Una herramienta que podemos utilizar para tratar de desaplicar técnicas de esteganografía es [[stegsolve]]. Con dicha herramienta, podemos ir alternando entre las diferentes técnicas que se contemplan para intentar conseguir datos ocultos.

También podemos hacer uso de la herramienta [[zsteg]], que nos proporciona información de datos incrustados en la imagen.

Muchas imágenes que nos encontremos han sido alteradas (Hue/saturation/Luminance) para esconder un mensaje secreto. **Gimp** es una muy buena herramienta para comprobar si hay texto en la fotografía, experimentando con diferentes ajustes modificables de la herramienta.
## Imágenes del sistema de archivos 

A veces nos encontraremos con retos que consistan en una imagen de un disco, y necesitaremos una estrategia para encontrar datos interesantes, sin ella tendremos que mirar absolutamente todo, lo cual toma mucho tiempo además de ser agotador.

### Montar y desmontar una imagen

Para comenzar, debemos crear el directorio donde queremos montar la imagen:

```
mkdir ruta/a/directorio
```

Procedemos ahora con la montura de la imagen:

```
mount <IMAGEN> /ruta/a/directorio
```

Puede que sea necesario con el parámetro -t indicar el tipo de sistema de archivos con el que estamos trabajando, en el caso de que no se detecte automaticamente.

Para desmontar la imagen, basta con ejecutar el siguiente comando:

```
umount ruta/a/directorio
```

### Búsqueda de archivos

Una vez tengamos acceso a los archivos del sistema de archivos, un comando muy útil para visualizar los archivos existentes es ```tree```. Si lo que deseamos es realizar una mejor búsqueda en el sistema, podemos realizar una [[Búsqueda a nivel del sistema]] para filtrar por diferentes campos que consideremos interesantes.

### Recuperación de archivos eliminados

Normalmente, no conseguiremos mucha información en el sistema, sino que tenemos que buscar en un volumen oculto, en espacio no asignado (espacio del disco que no forma parte de ninguna partición), en archivos eliminados o incluso en sistema de archivos que no poseen una estructura de sistema de archivos. 

Para listar los archivos en una imagen descomprimida, podemos ejecutar el siguiente comando:

```bash
fls <IMAGEN>
```

cuyo output nos devolverá los archivos que contiene, y un * en caso de tratarse de un archivo eliminado, teniendo asociado un número, con el que podemos encontrar el **path** completo al archivo eliminado/perdido con el siguiente comando:

```
ffind <IMAGEN> <NÚMERO_ASOCIADO>
```

Para los sistemas de archivos **EXT3** y **EXT4** podemos intentar encontrar archivos eliminados con la herramienta [[extundelete]]. Para todo lo demás, podemos utilizar [[TestDisk]] para recuperar tablas de particiones perdidas, archivos corruptos y cd ..archivos borrados en **FAT** o en **NTFS**, entre otras funciones. Otra herramienta interesante es [[Autopsy]], que nos permite buscar por clave en una imagen, o incluso ver a través del espacio no asociado.

### Sistemas de archivos de dispositivos integrados

Un ejemplo de este tipo de sistemas es **Squashfs**. Para poder analizar este tipo de sistemas de archivos deberemos utilizar la herramienta [[binwalk]].

## Captura de tráfico (PCAP, PCAPNG)

El tráfico de paquetes se guarda en un archivo **PCAP** o **PCAPNG**, que es una nueva versión y no soportada por todas las herramientas que mencionaremos, por lo que es recomendable convertir este tipos de archivos a un **PCAP**. 

Un ejemplo típico es que nos proporcionan un **PCAP** y debemos recuperar información archivos transferidos o un mensaje secreto. Suele ser un trabajo muy tedioso, ya que deberemos filtrar los datos encontrados.

### Análisis inicial

Comenzaremos utilizando la herramienta [[capinfos]] para conseguir información de la captura:

```
capinfos archivo.pcap
```

Continuamos ahora con el uso de la herramienta [[Wireshark]], que podemos utilizar tanto su interfaz gráfica como su versión por comandos. Esta herramienta permite utilizar *filtros* que reducen considerablemente el análisis con un buen uso. Existe también la página [PacketTotal](https://lab.dynamite.ai/) en la que podemos subir el PCAP y nos mostrará información gráficamente de las conexiones y los metadatos de SSL, además remarcará los paquetes que parezcan más sospechosos. Además podemos utilizar la herramienta [[ngrep]], que es parecida a **grep** pero para paquetes.

### Extracción de archivos

Puede ser que que tengamos que extraer archivos de un PCAP, y lo podemos realizar con diferentes herramientas, como [[Wireshark]], [[Xplico framework]], [[Network Miner]], [[Foremost]] o [[Snort]].

### Reparación de un PCAP dañado

Puede ser que nos enfrentemos ante un PCAP que se encuentre dañado, en este caso podemos hacer uso de la herramienta online [PCAPfix](https://f00l.de/pcapfix/).
## Volcados de memoria

En este caso nos darán una volcado de memoria y tendremos que extraer del mismo información secreta o un archivo contenido. 

Una de la herramienta más destacadas es [[Volatility]], que nos permitirá identificar las estructuras de los datos, como los procesos corriendo, contraseñas y más. Otra herramienta muy útil para este tipo de desafíos es [[Ethscan]], que se utiliza para encontrar datos en un volcado de memoria que parecen paquetes de tráfico. 


## PDF

Este formato de archivo puede ser muy difícil de examinar, ya que se pueden utilizar muchos trucos y hay muchos sitios para guardar información. El repositorio [PDF Tricks](https://github.com/corkami/docs/blob/master/PDF/PDF.md) tiene una guía muy completa para el análisis de estos archivos. 

Un **PDF** tiene un formato parcialmente formado por texto plano y posee muchos *objetos* binarios en su contenido. Para tener una idea introductoria del formato podemos visitar la página [blog.didierstevens.com](https://blog.didierstevens.com/2008/04/09/quickpost-about-the-physical-and-logical-structure-of-pdf-files/). Los objetos binarios pueden estar comprimidos o encriptados, y pueden contener lenguajes de scripting como *JavaScript* o *Flash*. Para explorar la arquitectura de un *PDF* podemos utilizar la herramienta [[Origami]], [[qpdf]] o incluso cualquier editor de texto.


## Video (mp4) o Audio (WAV, MP3)
## Formato de Microsoft's office (RTF, OLE, OOXML)
