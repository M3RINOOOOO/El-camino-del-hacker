# SSH

> **SSH (Secure Shell)** es un protocolo de red utilizado para acceder de manera segura a una máquina remota a través de una red no segura. Su principal función es permitir una comunicación segura entre dos sistemas a través de una red, típicamente para la administración remota de servidores y sistemas.

## Comandos para la conexión directa

**Conexión directa**: Se utiliza el comando `ssh` para conectar a un servidor **SSH**, y si fuera necesario se te pedirá la contraseña del usuario:

```bash
ssh user@server
```

**Conexión directa con contraseña**: Se utiliza `sshpass` para conectar a un servidor **SSH** proporcionando la contraseña directamente en la línea de comandos:

```bash
sshpass -p 'password' ssh user@server
```

## Conexión con llaves

Primero generamos las claves **SSH** en la máquina local:

```bash
ssh-keygen -t rsa -b 4096
```

Y nos pedirá una ruta para almacenar la clave, y si queremos establecer una contraseña para la llave privada.

A continuación transferimos la clave pública al servidor remoto:

```bash
ssh-copy-id usuario@server
```

Este comando guardará la clave pública generada en el archivo `~/.ssh/authorized_keys`.

Ahora al conectarnos al servidor no nos debería pedir contraseña con el siguiente comando:

```
ssh -i id_rsa usuario@server
```

## Ejecución de comandos sin conexión

```bash
sshpass -p 'password' ssh user@asda.com COMANDO 
#si nos echase debido al .bashrc o algo asi, podriamos forzar una shell con el comando bash
```
