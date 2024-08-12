# Editores de texto

**Nano** y **Vim** son dos de los editores de texto más populares, cada uno con sus propias características y filosofías de uso.

## Nano

**Nano** es un editor de texto simple y fácil de usar, diseñado para ser intuitivo, especialmente para los usuarios que no tienen experiencia con editores de texto en la línea de comandos. Posee una interfaz amigable que muestra los atajos del teclado en la parte inferior de la pantalla, facilitando la navegación en el mismo. Además las funciones de copiar y pegar se mantienen como la edición de texto básica.

## Vim

**Vim** es un editor de texto altamente configurable, poderoso y eficiente, que es una versión mejorada del antiguo editor Vi. Tiene varios modos de operación, como el modo `normal` (para navegar y manipular texto), el modo `inserción` (para escribir texto) y el modo `comando` (para ejecutar comandos). Vim es un editor muy personalizable, a través de su archivo de configuración de `~/.vimrc`. Algunos de los comandos necesarios para poder utilizar **Vim** son los siguientes:

```bash
alt+u   deshacer 
ctrl+r  rehacer
n+w     moverte n palabras (n natural xd)
n+b     moverte n palabras atras
n+d+w   cortar n palabras desde la actual
n+d+b   cortar n palabras para atras
nd+d    cortar n lineas desde la actual
o       crear una nueva linea y empezar a escribir
p       pegar lo que hay en el buffer
.       hacer lo ultimo que has hecho lol
/       buscar lo q quieras
:%s/palabra/sustitute cambia todas las coincidencias de palabra en el texto por sustitute

v       entrar en el modo visual (puedes seleccionar cosas, bastante intuitivo)
y       copiar la selección 
d       cortar la selección 
>       tabular lo seleccionado
<       destabular lo seleccionado

  q+a/b/c/d....       entrar en el modo grabación de macros (bastante loco la vd)
q       terminamos de grabar la macro
n+@+a/b.. ejecuta n veces la macro que hemos creado anteriormente 
```
