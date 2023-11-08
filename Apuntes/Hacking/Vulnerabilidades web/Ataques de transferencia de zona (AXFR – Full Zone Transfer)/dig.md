# dig

***

* Tags: #herramientas #web #AXFR

***

El comando **dig** es una herramienta de línea de comandos que se utiliza para realizar consultas DNS y obtener información sobre los registros DNS de un dominio específico.

La sintaxis para aplicar un \[\[Ataques de transferencia de zona (AXFR – Full Zone Transfer)]] en un servidor DNS es la siguiente:

➜ `dig @<DNS-server> <domain-name> <tipo-consulta>`

Donde:

* **DNS-server** es la dirección IP del servidor DNS que se desea consultar.
* **domain-name** es el nombre de dominio del cual se desea obtener la transferencia de zona.
* **tipo-consulta** es el tipo de consulta que se desea realizar. **AXFR** indica al servidor DNS que se desea una transferencia de zona completa, **ns** indica que queremos enumerar los name-servers, **mx** indica que queremos enumerar los servidores de correo.
