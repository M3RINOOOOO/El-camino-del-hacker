# Nmap

Nmap es una herramienta que nos permite identificar los hosts conectados a una red, los servicios que se están ejecutando en ellos y las vulnerabilidades que podrían ser explotadas. Es capaz de detectar una amplia gama de dispositivos conectados a una red y permite adaptar la seguridad según las necesidades.

### Técnicas

* **OS Fingerprinting**: Se utiliza para detectar sistemas operativos en una red.
* **ACK Scan**: Permite omitir el escaneo de puertos cerrados y enfocarse solo en los puertos abiertos.
* **XMAS Scan**: Envío de paquetes TCP con las banderas TCP URG y PUSH activadas para detectar hosts o puertos protegidos por firewalls que bloquean paquetes SYN.
* **ICMP Scan**: Utiliza paquetes ICMP Echo Request en lugar de TCP y UDP.

### Primeros ejemplos

```bash
nmap (parámetros) IP
```

Devuelve los puertos abiertos y filtrados. Nmap no puede determinar si están abiertos o cerrados.

#### Ejemplo de uso común

```bash
nmap -p- --open -sS --min-rate 5000 -v -n -Pn "ip"
```

#### Parámetros

* `-p`: Especifica los puertos a utilizar. `-p-` enumera todo el rango de puertos, `-p1-100` enumera los 100 primeros puertos.
* `--top-ports 500`: Escanea los 500 puertos más comunes.
* `--open`: Muestra solo los puertos abiertos.
* `-v`: (Verbose) Proporciona información en tiempo de ejecución.
* `-n`: Evita la resolución DNS para una ejecución más rápida.
* `-T5`: Ajusta la velocidad del escaneo de 0 a 5, siendo 5 la más rápida.
* `--min-rate 5000`: Ordena enviar un mínimo de 5000 paquetes por segundo.
* `-sT`: Opera por el protocolo TCP y realiza el three-way handshake.
* `-sU`: Opera por el protocolo UDP.
* `-Pn`: Asume que la máquina de destino está activa.
* `-p22 -sV ip`: Realiza fingerprinting sobre el puerto especificado para obtener información del servicio activo.
* `-sn 10.0.0.0/24`: Similar a arp-scan, escanea los hosts activos y proporciona una breve descripción.
* `-O ip`: OS Fingerprinting para detectar el sistema operativo de la máquina (No recomendado por ser agresivo).

### Evasión de Firewalls

Si no se detecta un puerto abierto que debería estarlo, es posible que un firewall esté bloqueando los paquetes. Para sortear estas restricciones:

* `-f`: Desfragmenta los paquetes para comprobar si un puerto está abierto.
* `--mtu 8*n`: Desfragmenta paquetes en tamaños múltiplos de 8.
* `-D ip1,ip2,ip3`: Simula que las peticiones provienen de varias IPs.
* `--spoof-mac Dell`: Utiliza OUI de las direcciones MAC de Dell.
* `--spoof-mac 00:AB:...`: Utiliza una dirección MAC específica.
* `--source-port x`: Establece un puerto de origen específico.
* `--data-length x`: Aumenta el tamaño del paquete para evadir detecciones.
* `-sS`: Omite el tercer paquete del three-way handshake para una conexión establecida, agilizando el proceso.

### Scripts

Nmap permite el uso de scripts en Lua (.nse). Los scripts por defecto abarcan varias categorías:

* **Default**: Incluye scripts básicos y útiles para la mayoría de los escaneos.
* **Discovery**: Se enfoca en descubrir información sobre la red, como detección de hosts activos y resolución de nombres de dominio.
* **Safe**: Incluye scripts seguros que no realizan actividades invasivas.
* **Intrusive**: Contiene scripts más invasivos, detectables por sistemas de seguridad pero útiles para descubrir vulnerabilidades.
* **Vuln**: Se centra en la detección de vulnerabilidades en sistemas y servicios de la red.

Un ejemplo de script en Lua, podría ser el siguiente:

```lua
-- HEAD --

description = [[
Script que me estoy inventando y esta sería su descripción
]]

-- RULE -- ##La regla que queremos que se aplique para que sea reportado en ACTION

##nos debería de dar tanto el host como el puerto
portrule = function(hots, port)
  return port.protocol == "tcp"    ##Que el protocolo usado sea tcp
    and port.state == "open"       ##y que esté abierto
end

-- ACTION -- ##La acción que queremos que se lleve a cabo cuando ha superado el filtro de RULE

action = function(host,port)
  return "This port is open!"
end
```

#### Uso de Scripts

* `-sC`: Ejecuta un conjunto de scripts básicos de reconocimiento. Puede combinarse con `-sV` quedando `-sCV`.
* `--script name`: Ejecuta un script específico o un conjunto de scripts de categorías deseadas (`--script="vuln and/or safe"`). Por ejemplo, `--script name=http-enum`realiza fuerza bruta sobre el servicio HTTP.
