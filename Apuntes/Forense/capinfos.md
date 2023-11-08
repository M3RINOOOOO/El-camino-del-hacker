-----------------------
- Tags: #herramientas #forense #pcap
------------------
# Definición

> **Capinfos** es una herramienta que forma parte del conjunto de herramientas de [[Wireshark]], que se utiliza para proporcionar información sobre archivos de captura de paquetes en formato **PCAP**. Proporciona estadísticas y detalles sobre el archivo de captura, como la cantidad de paquetes, duración de captura y tipo de tráfico.

----
# Uso

Para obtener información básica del archivo como la cantidad total de paquetes, tamaño del archivo y duración de la captura, podemos ejecutar:

```
capinfos archivo.pcap
```

Para conseguir información más detallada:

```
capinfos -a -i -z archivo.pcap
```

Donde el parámetro ```-a``` se utiliza para mostrar información avanzada, ```-i``` para imprimir estadísticas de entrada/salida y ```-z``` para mostrar estadísticas de protocolo.

Para guardar información en un archivo:

```
capinfos -Tm archivo.pcap > informacion.txt
```