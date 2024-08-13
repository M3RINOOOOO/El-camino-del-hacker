# Escaneo de puertos con descriptores de archivo

Podemos intentar abrir un descriptor de archivo para un host y puerto especificado. El uso de `/dev/tcp/host/port` es una característica especial de Bash que permite realizar conexiones TCP utilizando descriptores de archivo.&#x20;

Si el resultado de intentar ejecutar el descriptor de archivo es 0, significa que la conexión se ha realizado con éxito y el puerto está abierto. De esta manera, podríamos crear un scrip como el siguiente:

```bash
#!/bin/bash

function ctrl_c(){
  echo -e "\n\n [!] Saliendo...\n"
  tput cnorm;exit 1
}

# Ctrl+c
trap ctrl_c INT  

declare -a ports=( $(seq 1 65535))

function checkPort(){

  (exec 3<> /dev/tcp/$1/$2) 2>/dev/null

  if [ $? -eq 0 ];then
    echo "[+] Host $1 - Port $2 (OPEN)"
  fi

  exec 3<&-
  exec 3>&-
}

tput civis #Oculta el cursor 

if [ $1 ];then

  for port in ${ports[@]};do 
    checkPort $1 $port &
  done; wait #este wait se espera a que todos los hilos hayan acabado para seguir con el flujo del programa
  
else

  echo -e "\n\n[!] Uso: $0 <ip-addres>\n"
fi

# Recuperamos el cursor
tput cnorm
```
