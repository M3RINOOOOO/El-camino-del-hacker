# Búsqueda de OpCodes para entrar al ESP y cargar nuestro Shellcode

Una vez que se ha generado el shellcode malicioso y se han detectado los badchars, el siguiente paso es hacer que el flujo del programa entre en el shellcode para que sea interpretado. La idea es hacer que el registro EIP apunte a una dirección de memoria donde se aplique un **opcode** que realice un salto al registro **ESP** (**JMP ESP**), que es donde se encuentra el shellcode. Esto es así dado que de primeras no podemos hacer que el EIP apunte directamente a nuestro shellcode.

Para encontrar el opcode **JMP ESP**, se pueden utilizar diferentes herramientas, como **mona.py**, que permite buscar opcodes en módulos específicos de la memoria del programa objetivo. Una vez que se ha encontrado el opcode ‘**JMP ESP**‘, se puede sobrescribir el valor del registro EIP con la dirección de memoria donde se encuentra el opcode, lo que permitirá saltar al registro ESP y ejecutar el shellcode malicioso.

La búsqueda de opcodes para entrar al registro ESP y cargar el shellcode es una técnica utilizada para hacer que el flujo del programa entre en el shellcode para que sea interpretado. Se utiliza el opcode JMP ESP para saltar a la dirección de memoria del registro ESP, donde se encuentra el shellcode.
