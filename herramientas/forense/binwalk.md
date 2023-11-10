# binwalk

***

* Tags: #herramientas #forense #file\_recovering

***

## Definición

> **Binwalk** es una herramienta de código abierto utilizada para el análisis y la extracción de información de archivos binarios, especialmente en el contexto de la seguridad informática y la investigación de dispositivos embebidos. Está diseñada para identificar y analizar estructuras de datos dentro de archivos binarios, como imágenes, firmware y otros datos, y puede ayudar a revelar información oculta, firmas digitales, contraseñas y otros elementos relevantes en la investigación de seguridad y la ingeniería inversa.

***

## Uso

Para mostrar los datos integrados en un archivo, podemos ejecutar el siguiente comando:

```
binwalk file
```

Para mostrar y extraer todos los archivos de un archivo:

```
binwalk --dd ".*" file
```
