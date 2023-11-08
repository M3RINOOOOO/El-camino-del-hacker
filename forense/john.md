-----------------------
- Tags: #herramientas #forense #decrypt
------------------
# Definición

> **John the Ripper** es una herramienta de código abierto que se utiliza para realizar ataques de fuerza bruta y diccionario en contraseñas con el objetivo de probar su seguridad y descifrarlas.

--------
# Uso 

## Crackear un hash con cualquier cifrado
### Listado de cifrados
Para visualizar los formatos y subformatos admitidos por nuestra versión de **jonh** utilizamos:

```
jonh --list=formats
jonh --list=subformats
```

### Cifrado md5
Si tenemos un hash en **md5**, podemos tratar de descifrarlo de la siguiente manera:

```
jonh --wordlist=rockyou.txt --format=Raw-MD5 hash.txt
```

### Cifrado SHA-1
Si tenemos un hash en **SHA-1**, podemos tratar de descifrarlo de la siguiente manera:
```
jonh --wordlist=rockyou.txt --format=Raw-SHA1 hash.txt
```
## Aplicar fuerza bruta a un zip

Primero comenzamos con traducir el zip a un hash legible por [[john]] y lo copiamos a un **hash.txt**:

```
zip2john arhivo.zip > hash.txt
```

Una vez hayamos conseguido el hash, se lo podemos proporcionar a **john** de la siguiente manera:

```
john --format=pkzip --wordlist=rockyou.txt hash.txt
```

Y si la contraseña está en el diccionario, se mostrará por consola, y ya podremos conseguir los archivos comprimidos:

```
7z x -p<PASSWORD> zip.zip
```

