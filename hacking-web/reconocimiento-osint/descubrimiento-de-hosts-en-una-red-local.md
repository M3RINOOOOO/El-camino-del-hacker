# Descubrimiento de hosts en una red local

En la gestión y análisis de redes, es fundamental realizar un descubrimiento de hosts para identificar todos los dispositivos conectados. Aquí se presentan varias herramientas y técnicas para llevar a cabo este proceso:

### Nmap

El siguiente comando se utiliza para realizar un escaneo rápido en una red local con [nmap](nmap.md):

```bash
nmap -sn 192.169.1.0/24
```

Este comando realiza un "ping scan" o escaneo de ping en la subred `192.169.1.0/24`, identificando los hosts activos sin realizar un escaneo de puertos.

### Masscan

**Masscan** es conocido por su velocidad de escaneo. Aunque es extremadamente rápido, se recomienda ajustar la velocidad de escaneo para mejorar la precisión. Aquí se muestra un ejemplo de cómo usarlo  para escanear puertos específicos en una red grande:

```bash
masscan -p21,22,139,445 -Pn 192.168.0.0/16 --rate=10000
```

En este ejemplo, se escanean los puertos 21, 22, 139 y 445 en la subred `192.168.0.0/16`. La opción `-Pn` indica que no se realizará un escaneo de ping previo, y `--rate=10000` establece la tasa de paquetes por segundo para el escaneo, lo que permite un balance entre velocidad y precisión.

### ARP-Scan

ARP-Scan es una herramienta diseñada para descubrir dispositivos en la red local utilizando el protocolo ARP:

```bash
arp-scan -I ens33 --localnet --ignoredups
```

Este comando escanea la red local conectada a la interfaz `ens33` y omite las duplicaciones en la salida. Es una herramienta rápida y efectiva para descubrir hosts en la red local.

### Netdiscover

Netdiscover es una herramienta que realiza un escaneo ARP pasivo o activo. Aunque no se recomienda su uso en todos los casos, puede ser útil en ciertos escenarios:

```bash
netdiscover -i ens33
```

Este comando realiza un descubrimiento de hosts en la red local utilizando la interfaz `ens33`. Aunque es fácil de usar, se recomienda cautela debido a su comportamiento intrusivo en algunos casos.

### Verificación con ping

Para verificar si un host está activo, se puede utilizar el comando `ping`. Un código de estado `0` indica que el host está activo, mientras que un código de estado `1` sugiere que no hay ningún dispositivo con esa IP.

```bash
timeout 1 bash -c "ping -c 1 192.168.1.1" &>/dev/null && echo "[+] El host está activo"
```

Este script envía un solo paquete de ping (`-c 1`) al host `192.168.1.1` y espera una respuesta dentro de 1 segundo (`timeout 1`). Si el host responde, se muestra un mensaje indicando que el host está activo. De esta manera, podríamos hacer uso del siguiente script:

```bash
#!/bin/bash

function ctrl_c(){
  echo -e "\n\n [!] Saliendo...\n"
  tput cnorm; exit 1
}

# Ctrl+c
trap ctrl_c INT  

tput civis # Oculta el cursor
for i in $(seq 1 254);do
  timeout 1 bash -c "ping -c 1 192.168.1.$i" &>/dev/null && echo "[+] Host 192.168.1.$i - ACTIVO" &
done; wait 

# Recuperamos el cursor
tput cnorm
```

### Comprobación con puertos

En algunos casos, un host puede no responder a los paquetes ICMP utilizados por `ping`. En tales situaciones, se puede intentar abrir una conexión TCP en puertos comunes:

```bash
timeout 1 bash -c "echo '' > /dev/tcp/192.168.1.1/$port" 2>/dev/null && echo "[+] El host está activo"
```

Este script intenta abrir una conexión TCP a un puerto específico en el host `192.168.1.1`. Si tiene éxito, indica que el host está activo. De esta manera, podríamos hacer uso del siguiente script:

```bash
#!/bin/bash

function ctrl_c(){
  echo -e "\n\n [!] Saliendo...\n"
  tput cnorm; exit 1
}

# Ctrl+c
trap ctrl_c INT  

tput civis # Oculta el cursor
for i in $(seq 1 254);do
  for port in 21 22 23 25 80 139 443 445 8080; do
    timeout 1 bash -c "echo '' > /dev/tcp/192.168.1.$i/$port" &>/dev/null && echo "[+] Host 192.168.1.$i - port $port - ACTIVO" &
  done
done; wait 

# Recuperamos el cursor
tput cnorm
```

### Consideraciones sobre la máscara de subred

Aunque un servicio pueda tener una máscara de subred `/24` (Clase C), es posible encontrar más dispositivos utilizando una máscara `/16` (Clase B). Esto permite escanear un rango de direcciones IP más amplio y puede descubrir dispositivos que de otro modo pasarían desapercibidos.
