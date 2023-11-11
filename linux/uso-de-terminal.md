# Uso de terminal

Como hemos mencionado anteriormente, una de las razones principales por la que utilizamos Linx es por su consola de comandos (terminal), que nos permite realizar operaciones de una manera muy cómoda y automatizable.&#x20;

## Ruta absoluta vs ruta relativa

Antes de ver los comandos es necesario conocer la diferencia entre ruta **absoluta** y ruta **relativa** de un archivo o directorio. La ruta **absoluta** de un archivo especifica la ubicación del archivo o directorio desde el directorio raíz, mientras que la ruta **relativa** especifica la ubicación de un archivo o directorio en relación con el directorio actual, es decir, en el que nos encontremos actualmente.&#x20;

Por ejemplo, si estamos en el directorio `/home/usuario` y suponemos el caso en el que dentro del directorio raíz hay un directorio home, dentro de él uno llamado ususario, dentro uno llamado documentos, y dentro se encuentra el archivo, entonces su ruta **absoluta** será `/home/usuario/documentos/archivo.txt` , mientras que la ruta **relativa** será `documentos/archivo.txt`.

## Comandos básicos

Muchos comandos por sí solos no realizan ninguna tarea o simplemente dan error, ya que necesitan más información, que llamaremos **argumentos**. Son como unas especificaciones que se proporcina al comando para que realice la tarea exactamente como se desea. Por ejemplo, hay un comando para copiar archivos, que si no le dices qué quieres copiar y donde lo quieres copiar dará error porque falta información.

El directorio actual de trabajo se puede llamar desde consola con `.` y el directorio padre (el directorio que lo contiene) con `..` .

### ls

Lista los archivos y directorios del directorio que se pase como argumento. Si no se pasa ningún argumento lista el contenido del directorio actual.

```bash
ls /              #Listará el contenido del directorio raíz 
ls /home/usuario  #Listará el contenido del directorio /home/usuario
```

### cd

Cambia el directorio actual, se le puede proporcionar la ruta relativa o la ruta absoluta. Si no se pasa como argumento ningún directorio, tomará el directorio personal (/home/user).

```bash
cd /home            #Cambiará el directorio actual a /home (ruta absoluta)
cd usuario          #Cambiará el directorio actual a usuario (ruta relativa)
cd ..               #Cambiará el directorio actual al directorio padre
cd .                #Cambiará el directorio actual por el directorio actual (no hará nada)
```

### pwd

Muestra la ruta absoluta del directorio actual.

```bash
pwd                     #Mostrará la ruta relativa del directorio actual
```

### mkdir

Crea un nuevo directorio. Si no le proporcionamos argumentos, creará el directorio en el directorio actual de trabajo.

{% code fullWidth="false" %}
```bash
mkdir carpeta                 #Creará el directorio carpeta en el directorio actual de trabajo
mkdir /home/user/carpeta      #Creará el directorio carpeta en el directorio /home/user
```
{% endcode %}

cat !$ un cat del ultimo argumento pasado al aterior comando echo $? devuelve el estado del ultimo comando ejecutado (0 si ha sido correcto, distinto en otro caso) $0 representa el nombre del script que se invocó desde la terminal $n representa el n-esimo argumento desde la línea de comandos $# contiene el número de argumentos que son recibidos desde la línea de comandos $\* contiene todos los argumentos que son recibidos desde la línea de comandos, guardados en una misma variable

pushd /directorio Es igual que un cd, nos lleva a /directorio, pero posteriormente utilizando un popd volvemos a la ruta anterior (guau)

watch -n 1 comando cada segundo ejecuta el comando (interesante un ls -l para monitorizar el directorio)

wc -l archivo te devuelve el número de lineas del archivo

diff arhcivo1 archivo2 devuelve los cambios realizados de arhcivo1 a arhcivo2

tail -n 1 me quedo con la última linea de lo devuelto head -n 2 me quedo con las dos primeras lineas de lo devuelto

awk '{print $2}' FS=":" devuelve el segundo elemento de la cadena tomando como delimitador ":" awk 'NF{print $NF}' devuelve el ultimo elemento de la cadena awk "/cadena1/,/cadena2/" devuelve todo el texto desde cadena1 hasta la primera ocurrencia de cadena2

cut -d ':' -f 2 devuelve el segundo elemento de la cadena tomando como delimitador ":"

rev da la vuelta a la cadena

grep "^1$" -n te devuelve las lineas que contienen empiezan (^) por el 1 y terminan ($) por el 1 y te diga en qué línea se encontraba grep -v cadena elimina las lineas del texto con cadena grep -vE "cadena1|cadena2" elimina las lineas del texto con cadena1 y cadena2 grep con expresiones regulares: grep -oP '\d{1,3}.\d{1,3}.\d{1,3}.\d{1,3}' para grepear por ips (digitos de longitud de 1 a 3) grep -oP 'plugins/._' para grepear por plugins/ y mostrar todo lo que habría después (incluido la cadena plugins/) grep -oP 'plugins/\K._' Lo mismo que el de arriba solo que sin incluir la cadena plugins/ grep -oP 'plugins/\K\[^/]+' Para lo mismo que el de arribo solo que ahora te muestra hasta que encuentra una /

sed 's/cadena1/cadena2/g'sustituye las ocurrencias de la cadena1 por la cadena2 (si no estuviera la g lo haría solo con la primera ocurrencia)

sort ordena alfabeticamente todas las lineas del texto

uniq -u representa las lineas unicas del texto (debe estar previamente ordenado con sort)

xargs "comando pe ls" toma lo anterior como argumentos para el comando

strings lista las cadenas imprimibles de un archivo

echo "tetas" | base64 codea tetas en base 64 (si son múltiples líneas si ponemos -w 0 lo hace en solo una linea) echo "tetas" | base64 -d decodea tetas en base 64

tr -d 'cadena' elimina las ocurrencias de cadena en un texto tr '\[A-Za-z]' '\[N-ZA-Mn-za-m]' transforma una cadena de texto en rot13 a normal (A+13=N)

xxd transforma texto a hexadecimal xxd -ps transforma texto a hexadecimal y solo muestra el hexadecimal xxd -ps -r transforma hexadecimal a texto

$(echo "obase=10; ibase=16; 12CA" | bc) muestra 12CA expresado en decimal (o(utput)base,i(nput)base)

tee prueba crea el arhivo prueba con el resultado de lo anterior

md5sum te devuelve el hash de lo pasado

sponge prueba Si estabamos trabajando con cadenas de un archivo y tratamos de modificar con > ese archivo peta, con sponge no

7z l comprimido muestra el contenido del comprimido 7z x comprimido extrae el contenido del comprimido

\| while read line; do echo "linea $line"; done para cada linea pasada, se ejecuta la instrucción de dentro for line in (loqsea); do echo "linea $line"; done lo mismo que lo de arriba basically

COSA PECULIAR DEL COMANDO MORE si se ejecuta el comando more, y se queda con el porcentaje de que queda más texto, podemos pulsar v para entrar en el modo visual, esc, : y entramos como en modo comando, donde podemos declarar una variable set shell=/bin/bash y conseguimos una bash (lol)
