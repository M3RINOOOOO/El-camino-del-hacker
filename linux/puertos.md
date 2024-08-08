# Puertos

> Los **puertos** en un ordenador son puntos de comunicación utilizados para gestionar la transmisión de datos entre aplicaciones y servicios a través de una red. Los puertos permiten que múltiples servicios y aplicaciones utilicen una única dirección IP para comunicarse simultáneamente sin interferir entre sí.
>
> Los **puertos lógicos** son identificadores numéricos que permiten a las aplicaciones y servicios en un ordenador comunicarse a través de una red. Estos puertos ayudan a dirigir el tráfico de datos hacia el programa adecuado en una dirección IP específica.

**Algunos ejemplos**:

| Puerto    | Uso                |
| --------- | ------------------ |
| 20        | Datos FTP          |
| 21        | FTP                |
| 22        | SSH                |
| 23        | TELNET             |
| 25        | SMTP               |
| 53        | DNS                |
| 67        | DHCP               |
| 80        | HTTP               |
| 110       | POP3               |
| 111       | MAPEADOR PUERTOS   |
| 113       | AUTH / IDENT       |
| 123       | NTP                |
| 139       | NETBIOS            |
| 143       | IMAP               |
| 161       | SNMP               |
| 177       | XDMCP              |
| 389       | LDAP               |
| 443       | HTTPS              |
| 445       | MS DIRECTORY       |
| 465       | SMTP SSL           |
| 631       | IMPRESIÓN          |
| 635       | LDAP SSL           |
| 993       | IMAP               |
| 995       | POP3 SSL           |
| 5900      | REMOTE FRAMEBUFFER |
| 6000-6007 | X WINDOW           |

Algunos comandos esenciales para el manejo de puertos son los siguientes:

```bash
nc localhost 12345            # se conecta al servicio alojado en el puerto 12345 de la misma máquina (no te pide contraseña pero puede ser que sea necesaria)

nc -nlvp 12345                # se pone en escucha por el puerto 12345

echo "te" | nc localhost 1234 # se conecta al servicio ..... y envia el texto "te". Se podría pasar un archivo con cat en vez del echo y sería una especie de "brute-forcing"

ncat --ssl localhost 12345    # se conecta al servicio alojado en el puerto 12345 de la misma máquina utilizando ssl

netstat -nat//ss -nltp//cat /proc/net/tcp # para ver los puertos abiertos de la máquina actual

lsof -i:22                               # te muestra que servicio está usando el puerto 22

echo '' > /dev/tcp/127.0.0.1/22  # DESDE UNA BASH si existe el puerto nos devuelve ok, y si no existe nos devuelve un error
```
