# Metasploit

***

* Tags: #hacking #exploiting #shells #enumeración

***

## Definición

> Metasploit es un proyecto de código abierto para la seguridad informática, que proporciona información acerca de vulnerabilidades de seguridad y ayuda en tests de penetración "Pentesting" y el desarrollo de firmas para sistemas de detección de intrusos.

Para más información consultar [wikipedia](https://es.wikipedia.org/wiki/Metasploit)

***

## Uso

Para arrancar **Metasploit** de manera que se actualice la base de datos podemos ejecutar el siguiente comando en bash:

```bash
msfdb run
```

Para crear un \[\[Payload]] podemos hacer uso del siguiente comando:

```bash
msfvenom -p <tipo de payload> --platform <plataforma> -a <arquitectura> LHOST=<ip_local> LPORT=<puerto_local> -f <file> -o <output>
```
