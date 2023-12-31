# Generación de Bytearrays y detección de Badchars

En la generación de nuestro shellcode malicioso para la explotación del buffer overflow, es posible que algunos caracteres no sean interpretados correctamente por el programa objetivo. Estos caracteres se conocen como “**badchars**” y pueden causar que el shellcode falle o que el programa objetivo se cierre inesperadamente.

Para evitar esto, es importante identificar y eliminar los badchars del shellcode. En esta clase, veremos cómo desde Immunity Debugger podremos aprovechar la funcionalidad **Mona** para generar diferentes bytearrays con casi todos los caracteres representados, y luego identificar los caracteres que el programa objetivo no logra interpretar.

Una vez identificados los badchars, se pueden descartar del shellcode final y generar un nuevo shellcode que no contenga estos caracteres. Para identificar los badchars, se pueden utilizar diferentes técnicas, como la introducción de diferentes bytearrays con caracteres hexadecimales consecutivos, que permiten identificar los caracteres que el programa objetivo no logra interpretar.

Estos caracteres irán representados en la pila (**ESP**), que será donde veremos qué caracteres son los que no están siendo representados, identificando así los badchars.
