------------
- Tags: #enumeración #herramientas #scripting #bash #exploiting 
---------------------------
Es una herramienta manual programada en bash, en sustitución a [[Pspy]], es decir, nos va a ir 
Para ver todos los comandos que se están ejecutando, podemos ejecutar el siguiente comando:

```bash
ps -eo command
```

Así, haciendo uso de este comando podemos programar el siguiente script:

```bash
#!/bin/bash

function ctrl_c(){
	echo -e "\n\n[!] Saliendo...\n"
	tput cnorm;	exit 1
}

# Ctrl+C
trap ctrl_c SIGINT

old_process=$(ps -eo user,commmand)

tput civis # Ocultar el cursor

while true; do
	new_process=$(ps -eo user,command)
	diff <(echo "$old_process") <(echo "$new_process") | grep "[\>\<]" | grep -vE "command|kworker|procmon"
	old_process=$new_process
done

```