# Uso de terminal

Como hemos mencionado anteriormente, una de las razones principales por la que utilizamos Linx es por su consola de comandos (terminal), que nos permite realizar operaciones de una manera muy cómoda y automatizable.&#x20;

## Ruta absoluta vs ruta relativa

Antes de ver los comandos es necesario conocer la diferencia entre ruta **absoluta** y ruta **relativa** de un archivo o directorio. La ruta **absoluta** de un archivo especifica la ubicación del archivo o directorio desde el directorio raíz, mientras que la ruta **relativa** especifica la ubicación de un archivo o directorio en relación con el directorio actual, es decir, en el que nos encontremos actualmente.&#x20;

Por ejemplo, si estamos en el directorio `/home/usuario` y suponemos el caso en el que dentro del directorio raíz hay un directorio home, dentro de él uno llamado ususario, dentro uno llamado documentos, y dentro se encuentra el archivo, entonces su ruta **absoluta** será `/home/usuario/documentos/archivo.txt` , mientras que la ruta **relativa** será `documentos/archivo.txt`.

## Comandos básicos

A continuación se detalllan comandos esenciales para el uso y el entendimiento de la terminal.

### Comandos de navegación y manejo de archivos

#### cp

Copia archivos o directorios.

```bash
cp archivo destino      #Copiará archivo en la ruta destino
cp notas /home          #Copiará el archivo notas en la ruta /home
```

#### mv

Mueve archivos o directorios de directorio o cambia el nombre del archivo o directorio.

```bash
mv archivo destino       #Moverá el archivo a la ruta destino
mv notas /home           #Moverá el archivo notas a la ruta /home
mv notas notas_modified    #Cambiará el nombre del archivo notas a notas_modified
```

#### rm

Elimina archivos o directorios.

```bash
rm archivo            #Eliminará el archivo 
rm -rf directorio     #Eliminará el directorio y los archivos de su interior recursivamente
```

#### pushd/popd

El comando **pushd** es igual que un cd, nos lleva al directorio pasado como argumento, pero posteriormente utilizando un **popd** podemos volvemos a la ruta anterior (guau).

```bash
pwd          #/home/Desktop/carpeta
pushd /tmp   #nos vamos al directorio /tmp
...
popd         
pwd          #/home/Desktop/carpeta
```

#### tar

Archiva y descomprime archivos.

```bash
tar -czvf archivo.tar.gz /ruta/al/directorio  # Crea un archivo comprimido "archivo.tar.gz" del directorio especificado 
tar -xzvf archivo.tar.gz                      # Descomprime "archivo.tar.gz"
```

#### 7z

Se utiliza para la compresión y descompresión de archivos.

```bash
7z a archivo_comprimido.7z archivo.txt # Comprime "archivo.txt" en un archivo llamado "archivo_comprimido.7z".
7z l comprimido                        # muestra el contenido del comprimido
7z x comprimido                        # extrae el contenido del comprimido
```

#### echo

Muestra un mensaje en la consola.

```bash
echo "Hola, Mundo!"       # Imprime "Hola, Mundo!" 
echo $PATH                # Muestra el valor de la variable de entorno PATH
```

#### sponge

Se utiliza para leer la entrada estándar y escribirla en un arhicov después de haber leído toda la entrada. Si estabamos trabajando con cadenas de un archivo y tratamos de modificar con > ese archivo peta, con sponge no

```bash
sed 's/ejemplo/prueba/' archivo.txt | sponge archivo.txt # Reemplaza "ejemplo" por "prueba" en "archivo.txt".
```

#### tee

Se utiliza para leer de la entrada estándar y escribir el contenido simultáneamente en un archivo y en la salida estándar.

```bash
echo "Texto de ejemplo" | tee archivo.txt # Escribe "Texto de ejemplo" tanto en la consola como en "archivo.txt"
echo "Más texto" | tee -a archivo.txt     # Añade "Más texto" al final de "archivo.txt" y también muestra el texto en la consola
ls -l | tee listado.txt                   # Muestra la lista detallada de archivos y directorios en la consola y la guarda en "listado.txt"
```

#### xargs

Se utiliza para construir y ejecutar comandos a partir de la entrada estándar.

```bash
find /path/to/directory -name '*.tmp' | xargs rm # Encuentra todos los archivos .tmp en el directorio especificado y los elimina
find /path/to/directory -name '*.txt' | xargs wc -w # Cuenta el número de palabras en todos los archivos .txt encontrados
```

### Comandos de visualización y búsqueda

#### cat

Muestra el contenido de un archivo.

```bash
cat archivo.txt                 # Muestra el contenido de "archivo.txt" 
cat archivo1.txt archivo2.txt   # Muestra el contenido de "archivo1.txt" y "archivo2.txt" secuencialmente
```

#### more

Muestra contenido de archivos de forma paginada.

```bash
more archivo.txt             # Muestra el contenido de "archivo.txt" de forma paginada 
```

_COSA PECULIAR DEL COMANDO MORE_ Si se ejecuta el comando more, y se queda con el porcentaje de que queda más texto, podemos pulsar v para entrar en el modo visual, esc, : y entramos como en modo comando, donde podemos declarar una variable set shell=/bin/bash y conseguimos una bash (lol)

#### grep

Busca cadenas en archivos. A continuación varios ejemplos de uso:

```bash
grep "término_busqueda" archivo.txt # Busca "término_busqueda" en "archivo.txt" 
grep -r "término_busqueda" /ruta    # Busca "término_busqueda" en todos los archivos del directorio especificado
grep "^1$" -n                       #te devuelve las lineas que contienen empiezan (^) por  el 1 y terminan ($) por el 1 y te diga en qué línea se encontraba
grep -v cadena                      #elimina las lineas del texto con cadena
grep -vE "cadena1|cadena2"          #elimina las lineas del texto con cadena1 y cadena2
```

_grep con expresiones regulares:_

```bash
grep -oP '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}' #grepea por ips (digitos de longitud de 1 a 3) 
grep -oP 'plugins/.*'                         #grepea por plugins/ y muestra todo lo que habría después (incluido la cadena plugins/)
grep -oP 'plugins/\K.*'                       #lo mismo que el de arriba solo que sin incluir la cadena plugins/
grep -oP 'plugins/\K[^/]+'                    #lo mismo que el de arriba solo que ahora te muestra hasta que encuentra una /  
```

#### find

Busca archivos y directorios.

```bash
find /ruta -name "nombre_archivo" # Busca archivos con nombre "nombre_archivo" en "/ruta" find . -type f -name "*.txt"      # Busca todos los archivos .txt en el directorio actual y sus subdirectorios
```

Para más info mirar \[\[Búsqueda a nivel del sistema]].

#### locate

Encuentra archivos rápidamente.

```bash
locate nombre_archivo         # Busca archivos con nombre "nombre_archivo" 
locate *.conf                 # Busca todos los archivos con extensión .conf
```

#### wc

Cuenta líneas, palabras y caracteres en archivos o entrada estándar.

```bash
wc archivo.txt                # Muestra el número de líneas, palabras y caracteres en "archivo.txt"
wc -l archivo.txt             # Muestra solo el número de líneas en "archivo.txt"
wc -w archivo.txt             # Muestra solo el número de palabras en "archivo.txt"
wc -c archivo.txt             # Muestra solo el número de caracteres en "archivo.txt"º
```

#### diff

Compara archivos línea por línea y muestra las diferencias.

```bash
diff archivo1.txt archivo2.txt          # Compara archivo1.txt y archivo2.txt y muestra las diferencias
```

#### tail

Muestra las últimas líneas de un archivo.

```bash
tail -n 10 archivo.txt           # Muestra las últimas 10 líneas de "archivo.txt"
```

#### head

Muestra las primeras líneas de un archivo.

```bash
head -n 10 archivo.txt           # Muestra las primeras 10 líneas de "archivo.txt"
```

#### awk

Se utiliza para la manipulación de datos y generación de informes.

```bash
awk '{print $1}' archivo.txt          # Imprime la primera columna de cada línea 
awk '{print $1, $3}' archivo.txt      # Imprime la primera y tercera columna de cada línea 
awk '/patrón/ {print $0}' archivo.txt # Imprime las líneas que contienen "patrón" 
awk '{print $2}' FS=":" archivo.txt   # Devuelve el segundo elemento de la cadena tomando como delimitador ":"
awk 'NF{print $NF}'                   # Devuelve el ultimo elemento de la cadena 
awk "/cadena1/,/cadena2/"             # Devuelve todo el texto desde cadena1 hasta la primera ocurrencia de cadena2
```

#### cut

Extrae secciones de cada línea de un archivo o de la entrada estándar.

```bash
cut -d 'delimitador' -f campo archivo.txt   # Extrae el campo especificado usando el delimitador dado
cut -c rango archivo.txt                    # Extrae los caracteres en el rango especificado
cut -d ':' -f 2                             # Devuelve el segundo elemento de la cadena tomando como delimitador ":"
```

#### rev

Invierte el orden de los caracteres en cada línea de un archivo o entrada estándar.

```bash
rev archivo.txt # Invierte cada línea en "archivo.txt" 
echo "Hola Mundo" | rev # Invierte la cadena "Hola Mundo"
```

#### sed

Es un editor de texto de flujo que se utiliza para realizar transformaciones básicas en texto

```bash
sed 's/patrón/reemplazo/' archivo.txt # Reemplaza la primera aparición de "patrón" con "reemplazo" en cada línea de "archivo.txt" 
sed 's/patrón/reemplazo/g' archivo.txt # Reemplaza todas las apariciones de "patrón" con "reemplazo" en cada línea de "archivo.txt"
```

#### sort

ordena alfabeticamente todas las lineas del texto

```bash
sort archivo.txt # Ordena las líneas de "archivo.txt" en orden alfabético
```

#### uniq

Elimina las líneas duplicadas consecutivas de un archivo o entrada estándar.

```bash
uniq archivo.txt              # Elimina las líneas duplicadas consecutivas en "archivo.txt"
uniq -c archivo.txt           # Muestra el número de veces que aparece cada línea
uniq -d archivo.txt           # Muestra solo las líneas duplicadas
uniq -u archivo.txt           # Muestra solo las líneas únicas (no duplicadas)
```

#### strings

Lista las cadenas imprimibles de un archivo

```bash
strings archivo.txt          #lista las cadenas imprimibles del archivo.txt
```

#### base64

Se utiliza para codificar o decodificar datos en formato **Base64**.

```bash
base64 archivo.bin > archivo_encoded.txt # Codifica el contenido de "archivo.bin" 
base64 -d archivo_encoded.txt > archivo_decoded.bin # Decodifica el contenido de "archivo_encoded.txt" 
echo "Texto para codificar" | base64 # Codifica "Texto para codificar" en Base64 y muestra el resultado
echo "VGV4dG8gcGFyYSBjb2RpZmljYXI=" | base64 -d # Decodifica la cadena Base64 "VGV4dG8gcGFyYSBjb2RpZmljYXI=" y muestra el resultado
```

#### tr

Se utiliza para traducir o eliminar caracteres en la entrada estándar. Se suele utilizar para realizar transformaciones de texto simples.

```bash
echo "texto en minúsculas" | tr 'a-z' 'A-Z' # La salida será: TEXTO EN MINÚSCULAS
echo "texto con espacios" | tr -d ' '       # La salida será: textoconespacios
echo "valor1,valor2,valor3" | tr ',' ';'    # La salida será: valor1;valor2;valor3
tr -d 'cadena'                              #elimina las ocurrencias de cadena en un texto
tr '[A-Za-z]' '[N-ZA-Mn-za-m]'              #transforma una cadena de texto en rot13 a normal (A+13=N)
```

#### xxd

Se utiliza para crear una representación hexadecimal de un arhico binario o para revertir los cambios

```bash
xxd archivo.bin      # Muestra el contenido del archivo "archivo.bin" en formato hexadecimal
xxd -p archivo.bin   # Muestra el contenido del archivo "archivo.bin" en formato hexadecimal sin direcciones ni ASCII
echo -n "Texto de ejemplo" | xxd -p # La salida será: 546578746f206465206a656d706c6f
```

### Comandos de Red y Seguridad

#### ifconfig o ip

Configura interfaces de red.

```bash
ifconfig                   # Muestra la configuración de las interfaces de red 
ip addr                    # Muestra la configuración de las interfaces de red con el comando ip
```

#### ping

Comprueba la conectividad de red.

```bash
ping google.com            # Envía paquetes ICMP a "google.com" 
ping -c 1 google.com       # Envía 1 paquetes ICMP a "google.com"
```

#### netstat

Muestra conexiones de red, tablas de enrutamiento, etc.

```bash
netstat -tuln              # Muestra puertos abiertos y servicios escuchando 
netstat -i                 # Muestra estadísticas de interfaces de red
```

#### curl

Realiza transferencias de datos desde o hacia un servidor.

```bash
curl http://ejemplo.com   # Descarga el contenido de "ejemplo.com" curl -O http://ejemplo.com/archivo.txt  # Descarga "archivo.txt" desde "ejemplo.com"
```

#### wget

Descarga archivos desde la web.

```bash
wget http://ejemplo.com/archivo.txt  # Descarga "archivo.txt" desde "ejemplo.com" wget -r http://ejemplo.com           # Descarga recursivamente el contenido de "ejemplo.com"
```

### Comandos de Supervisión del Sistema

#### top

Muestra los procesos en ejecución.

```bash
top                       # Muestra los procesos activos y el uso de recursos en tiempo real top -u usuario            # Muestra los procesos del "usuario" especificado
```

#### ps

Muestra información sobre los procesos en ejecución.

```bash
ps aux                    # Lista todos los procesos en ejecución 
ps -ef                    # Muestra una lista detallada de los procesos en ejecución
```

#### watch

Ejecuta un comando de forma periódica y muestra su salida.

```bash
watch -n 1 comando             # Ejecuta el comando cada 1 segundo y muestra su salida
watch -n 1 'ls -la'            # Lista los archivos en el directorio actual cada 1 segundo
```

### Otros

#### history

Muestra el historial de comandos ejecutados.

```bash
history                  # Lista los comandos ejecutados anteriormente 
history -c               # Borra el historial de comandos
```

#### md5sum

Se utiliza para calcular el hash **MD5** de archivos o datos

```bash
md5sum archivo.txt                   # La salida será algo como: e99a18c428cb38d5f260853678922e03  archivo.txt
echo -n "Texto de ejemplo" | md5sum  # La salida será algo como: f4dcc3b5aa765d61d8327deb882cf99 -
md5sum archivo1.txt archivo2.txt     # Muestra los hashes MD5 de "archivo1.txt" y "archivo2.txt". Comparando los dos hashes puedes verificar si los archivos son idénticos.
```

### Argumentos especiales

A continuación vienen detallados algunos argumentos especiales que pueden ser muy útiles a la hora de crear un script en **bash**.

#### !$

Es un atajo en la línea de comandos de Bash (y otros shells similares) que se refiere al último argumento del comando anterior.

```bash
ls /var/log 
cat !$             #Se realizará un cat de /var/log 
```

#### $?

Es una variable especial que almacena el estado de salida (o código de retorno) del último comando ejecutado.

```bash
ls /noexiste
echo $?             #Devolverá un valor de 0
```

#### $0

Representa el nombre del script que se invocó desde la terminal

```bash
# script.sh
echo "Este es el script: $0"   #El resultado será `Este es el script: ./script.sh`
```

#### $n

Representa el n-esimo argumento desde la línea de comandos

#### $\#

Contiene el número de argumentos que son recibidos desde la línea de comandos

```bash
# script.sh 
echo "Número de argumentos: $#"   #Si ejecutas `./script.sh arg1 arg2`, el resultado será `Número de argumentos: 2`
```

#### $\*

Contiene todos los argumentos que son recibidos desde la línea de comandos, guardados en una misma variable

```bash
# script.sh 
echo "Todos los argumentos: $*"    # Si ejecutas `./script.sh arg1 arg2`, el resultado será `Todos los argumentos: arg1 arg2`.
```
