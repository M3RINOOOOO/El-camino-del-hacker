# PATH Hijacking

**PATH Hijacking** es una técnica utilizada por los atacantes para **secuestrar** **comandos** de un sistema Unix/Linux mediante la manipulación del **PATH**. El PATH es una variable de entorno que define las rutas de búsqueda para los archivos ejecutables en el sistema.

En algunos binarios compilados, algunos de los comandos definidos internamente pueden ser indicados con una ruta relativa en lugar de una ruta absoluta. Esto significa que el binario busca los archivos ejecutables en las rutas especificadas en el PATH, en lugar de utilizar la ruta absoluta del archivo ejecutable.

Si un atacante es capaz de alterar el PATH y crear un nuevo archivo con el mismo nombre de uno de los comandos definidos internamente en el binario, puede lograr que el binario ejecute la versión maliciosa del comando en lugar de la versión legítima.

Por ejemplo, si un binario compilado utiliza el comando “**ls**” sin su ruta absoluta en su código y el atacante crea un archivo malicioso llamado “**ls**” en una de las rutas especificadas en el PATH, el binario ejecutará el archivo malicioso en lugar del comando legítimo “**ls**” cuando sea llamado.

Para prevenir el PATH Hijacking, se recomienda utilizar **rutas absolutas** en lugar de rutas relativas en los comandos definidos internamente en los binarios compilados. Además, es importante asegurarse de que las rutas en el PATH sean controladas y limitadas a las rutas necesarias para el sistema. También se recomienda utilizar la opción de permisos de ejecución para los archivos ejecutables solo para los usuarios y grupos autorizados.
