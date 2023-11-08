# Abuso de permisos incorrectamente implementados

En sistemas Linux, los archivos y directorios tienen permisos que se utilizan para controlar el acceso a ellos. Los permisos se dividen en tres categorías: propietario, grupo y otros. Cada categoría puede tener permisos de lectura, escritura y ejecución. Los permisos de un archivo pueden ser modificados por el propietario o por el superusuario del sistema.

El abuso de permisos incorrectamente implementados ocurre cuando los permisos de un archivo crítico son configurados incorrectamente, permitiendo a un usuario no autorizado acceder o modificar el archivo. Esto puede permitir a un atacante leer información confidencial, modificar archivos importantes, ejecutar comandos maliciosos o incluso obtener acceso de superusuario al sistema.

De esta forma, un atacante experimentado podría aprovecharse de esta falla para elevar sus privilegios en el mejor de los casos. Una de las herramientas encargadas de aplicar este reconocimiento en el sistema es ‘**lse**‘. Linux Smart Enumeration (LSE) es una herramienta de enumeración de seguridad para sistemas operativos basados en Linux, diseñada para ayudar a los administradores de sistemas y auditores de seguridad a identificar y evaluar vulnerabilidades y debilidades en la configuración del sistema.

LSE está diseñado para ser fácil de usar y proporciona una salida clara y legible para facilitar la identificación de problemas de seguridad. La herramienta utiliza comandos de Linux estándar y se ejecuta en la línea de comandos, lo que significa que no se requiere software adicional. Además, enumera una amplia gama de información del sistema, incluyendo usuarios, grupos, servicios, puertos abiertos, tareas programadas, permisos de archivos, variables de entorno y configuraciones del firewall, entre otros.

A continuación, se os proporciona el enlace directo al proyecto de Github correspondiente a esta herramienta:

* **Linux Smart Enumeration**: [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
