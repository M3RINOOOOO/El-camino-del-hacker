# ExifTool

***

* Tags: #herramientas #web #file\_upload

***

## Definición

> **ExifTool** es una herramienta de línea de comandos y una biblioteca de Perl que permite la lectura, escritura y manipulación de metadatos en archivos multimedia, como imágenes, videos y archivos de audio.
>
> **ExifTool** es especialmente conocida por su capacidad para trabajar con los metadatos **EXIF** (Exchangeable Image File Format), que son datos incrustados en imágenes digitales y que pueden incluir información sobre la cámara, la configuración de la exposición, la ubicación geográfica y otros detalles relacionados con la captura de la imagen.

***

## Uso

Para visualizar los metadatos de un archivo, podemos utilizar el siguiente comando:

```
exiftool file
```

Para visualizar los metadatos de los archivos de un directorio recursivamente, utilizamos el siguiente:

```
exiftool nombre_del_archivo.extensión
```

Para escribir un comentario en un metadato:

```
exiftool -Comment'Comentario'
```
