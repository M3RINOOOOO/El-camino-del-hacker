-----------------------
- Tags: #herramientas #forense #file_recovering
------------------
# Definición

> **Extundelete** es una utilidad de código abierto diseñada específicamente para la recuperación de archivos que hayan sido borrados accidentalmente en sistemas de archivos ext3 y ext4, comunes en sistemas Linux. Funciona examinando los metadatos del sistema de archivos, como la tabla de asignación de inodos, para localizar y restaurar archivos eliminados, siempre que el espacio de disco donde se encontraban no haya sido sobrescrito

# Uso

Para recuperar todos los archivos que han sido eliminados del disco, podemos ejecutar el siguiente comando:

```
extundelete disk-image --restore-all
```

Si tenemos la ruta del archivo que queremos recuperar, lo podemos utilizar:

```
extundelete disk-image --restore-file /path/al/archivo
```