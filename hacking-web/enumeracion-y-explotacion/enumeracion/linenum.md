# Linenum

***

* Tags: #hacking #enumeración #herramientas #automático #escalada\_de\_privilegios

***

## Definición

> **Linenum** es una herramienta muy parecida a \[\[Linux Smart Enumeration]], solo que un poco más antigua, aunque también muy útil a la hora de realizar una \[\[Escalada de privilegios]].

***

## Uso

Para hacer uso de **Linenum**, podemos ejecutar el siguiente comando:

```bash
./LinEnum.sh
```

Si queremos que se realice un reporte de todo lo conseguido, podemos ejecutar lo siguiente:

```bash
./LinEnum.sh -r report -e /tmp/ -t
```

Donde cada parámetro significa lo siguiente:

* `-r`: Indica el nombre del reporte
* `-e`: Indica donde queremos almacenar dicho reporte
* `-t`: Indica que se incluyan pruebas exhaustivas (largas)

***

### Referencias
