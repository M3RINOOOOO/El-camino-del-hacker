# Modificación del Shellcode para controlar el comando que se desea ejecutar

Además de los payloads que se han utilizado en las clases anteriores, también es posible utilizar payloads como “**windows/exec**” para cargar directamente el comando que se desea ejecutar en la variable **CMD** del payload. Esto permite crear un nuevo shellcode que, una vez interpretado, ejecutará directamente la instrucción deseada.

El payload “**windows/exec**” es un payload de Metasploit que permite ejecutar un comando arbitrario en la máquina objetivo. Este payload requiere que se especifique el comando a ejecutar a través de la variable CMD en el payload. Al generar el shellcode con msfvenom, se puede utilizar el parámetro “-p windows/exec CMD=” para especificar el comando que se desea ejecutar.

Una vez generado el shellcode con el comando deseado, se puede utilizar la técnica de buffer overflow para sobrescribir el registro EIP y hacer que el flujo del programa entre en el shellcode. Al interpretar el shellcode, se ejecutará directamente el comando especificado en la variable CMD.
