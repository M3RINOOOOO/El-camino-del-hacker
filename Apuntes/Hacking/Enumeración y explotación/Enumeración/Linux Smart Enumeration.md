--------------------------
- Tags: #hacking #enumeración #herramientas #automático #escalada_de_privilegios 
----------------------------------
# Definición

> **LSE (Linux Smart Enumeration)**[^1] es un herramienta de enumeración para sistemas **linux** con la que podemos obtener información detallada sobre la configuración del sistema, los servicios en ejecución y permisos de archivos. Con esta herramienta podemos detectar diversas vulnerabilidades y encontrar información valiosa. 

-----------

# Uso

Para hacer uso de **LSE**, podemos ejecutar el siguiente comando:

```bash
./lse.sh
```

Si queremos que nos muestre más información podemos ejecutar el siguiente comando:

```bash
./lse.sh -l <nivel>    #Donde el nivel va desde el 0 al 2 mostrando de menos a más información
```

Si encontrase alguna vía crítica para realizar una [[Escalada de privilegios]], nos lo representaría con color rojo.

---------
## Referencias

[^1]: Enlace al proyecto en Github: [Enlace](https://github.com/diego-treitos/linux-smart-enumeration)
