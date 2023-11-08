# Wireshark

***

* Tags: #herramientas #forense #pcap

***

## Definición

> **Wireshark** es una herramienta de software de código abierto que se utiliza para analizar el tráfico de red en tiempo real. Permite capturar y examinar los datos que fluyen a través de una red, lo que facilita la detección de problemas de red, el monitoreo de la actividad y la resolución de cuestiones de seguridad. Wireshark muestra información detallada sobre los paquetes de datos, incluyendo su origen, destino, contenido y protocolos utilizados.
>
> Esta herramienta tiene una interfaz gráfica de usuario (GUI) que permite a los usuarios capturar y analizar datos de red de manera intuitiva a través de menús y ventanas interactivas. También ofrece una interfaz de línea de comandos (tshark) (CLI) que permite a los usuarios avanzados realizar tareas específicas utilizando comandos de texto en lugar de la GUI, lo que brinda un mayor control y automatización.

***

## Uso

Para hacer uso de la herramienta con interfaz gráfica simplemente ejecutaremos el siguiente comando:

```bash
wireshark archivo.pcap
```

### Recuperación de archivos

Para recuperar archivos, suponiendo que se está utilizando el protocolo _HTTP_, debemos irnos a `File -> Export Objects -> HTTP -> Save all`, lo cual nos descargará los archivos en el directorio actual de trabajo para su posterior análisis.

Si queremos exportar archivos encontrados en paquetes específicos, podemos exportar dichos paquetes como otro PCAP, para convertirlo a un binario y terminar extrayendo el archivo incrustado:

* **Exportar paquetes específicos**: Comenzamos seleccionando los paquetes que deseemos exportar, pulsamos en `File -> Export Pacjer Dissections` y elegimos que se guarde como texto plano.
* **Convertir un paquete a binario**: Para convertir un PCAP a binario, podemos ejecutar la siguiente sentencia:

```
tshark -r archivo.pcap -w archivo.bin
```

* **Extraer archivo incrustado**: Normalmente debemos conocer el formato del archivo, y utilizar herramientas específicas para ese tipo de archivo, o utilizar \[\[binwalk]]:

```
foremost -i archivo.bin -o carpeta_salida
unzip archivo.bin carpeta_salida
binwalk --dd ".*" file
```

### Filtrar paquetes

Podemos utilizar el cuadro de filtro de la parte superior para aplicar filtros, y poder enfocarnos en paquetes que realmente nos interesen. Algunos de los filtros más comunes son:

* `<PROTOCOLO>`: Para filtrar por el protocolo utilizado (http, arp, dns...)
* `ip.src == <IP_ORIGEN>`: Para filtrar por la ip que envía el paquete
* `ip.dst == <IP_DESTINO>`: Para filtrar por la ip que recibe el paquete
* `tcp.srcport == <PUERTO_ORIGEN>`: Para filtrar por el puerto por el que se envía el paquete
* `tcp.dstport == <PUERTO_DESTINO>`: Para filtrar por el puerto por el que se recibe el paquete
* `frame.len > <LONGITUD_PAQUETE>`: Para filtrar por los paquetes que tengan una longitud mayor a la indicada

Además, podemos utilizar varios filtros a la vez con el operador `&&` o `||`, por ejemplo:

* `<PROTOCOLO> && ip.src == <IP_ORIGEN>`: Para filtrar por el protocolo indicado y la ip que envía el paquete.
* `ip.src == <IP_ORIGEN1> || ip.src == <IP_ORIGEN2>`: Para filtrar por la ip que envía el paquete, ya sea `IP_ORIGEN1` o `IP_ORIGEN2`

### Seguir flujo de comunicación

Wireshark permite seguir el flujo de la conversación, lo cual puede ser muy útil en algunos casos. Si queremos seguir el flujo de un paquete que nos interese, podemos hacer click derecho sobre dicho paquete y seleccionar la opción de _Follow_.

Si queremos seguir el flujo de un protocolo como tal, por ejemplo _TCP_, podemos pulsar en `Analyze -> Follow -> TCP Stream`, lo cual nos abrirá otra ventana mostrando cada paquete con la opción de ver el siguiente y el anterior.

### Mostrar las estadísticas de red

Para mostrar un resumen general de las estadísticas, podemos irnos a `Statistics -> Capture File Properties`.

Para analizar las conversaciones entre direcciones IP y puertos `Statistics -> Conversations`.

Para mostrar las estadísticas desglosadas por tipo de protocolo, permitiendo ver cuántos paquetes corresponden a cada protocolo: `Statistics -> Protocol Hierarchy`.
