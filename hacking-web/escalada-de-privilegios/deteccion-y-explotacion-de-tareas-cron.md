# Detección y explotación de tareas Cron

Una tarea **cron** es una tarea programada en sistemas Unix/Linux que se ejecuta en un momento determinado o en intervalos regulares de tiempo. Estas tareas se definen en un archivo **crontab** que especifica qué comandos deben ejecutarse y cuándo deben ejecutarse.

La detección y explotación de tareas cron es una técnica utilizada por los atacantes para elevar su nivel de acceso en un sistema comprometido. Por ejemplo, si un atacante detecta que un archivo está siendo ejecutado por el usuario “root” a través de una tarea cron que se ejecuta a intervalos regulares de tiempo, y se da cuenta de que los permisos definidos en el archivo están mal configurados, podría manipular el contenido del mismo para incluir instrucciones maliciosas las cuales serían ejecutadas de forma privilegiada como el usuario ‘root’, dado que corresponde al usuario que está ejecutando dicho archivo.

Ahora bien, para detectar tareas cron, los atacantes pueden utilizar herramientas como **Pspy**. Pspy es una herramienta de línea de comandos que monitorea las tareas que se ejecutan en segundo plano en un sistema Unix/Linux y muestra las nuevas tareas que se inician.

Con el objetivo de reducir las posibilidades de que un atacante lograra explotar las tareas cron en un sistema, se recomienda llevar a cabo alguno de los siguientes puntos:

* **Limitar el número de tareas cron**: es importante limitar el número de tareas cron que se ejecutan en el sistema y asegurarse de que solo se otorgan permisos a tareas que requieren permisos especiales para funcionar correctamente. Esto disminuye la superficie de ataque y reduce las posibilidades de que un atacante pueda encontrar una tarea cron vulnerable.
* **Verificar los permisos de las tareas cron**: es importante revisar los permisos de las tareas cron para asegurarse de que solo se otorgan permisos a usuarios y grupos autorizados. Además, se recomienda evitar otorgar permisos de superusuario a las tareas cron, a menos que sea estrictamente necesario.
* **Supervisar regularmente el sistema**: es importante monitorear regularmente el sistema para detectar cambios inesperados en las tareas cron y para buscar posibles brechas de seguridad. Además, se recomienda utilizar herramientas de monitoreo de seguridad para detectar actividades sospechosas en el sistema.
* **Configurar los registros de la tarea cron**: se recomienda habilitar la opción de registro para las tareas cron, para poder identificar cualquier actividad sospechosa en las tareas definidas y para poder llevar un registro de las actividades realizadas por cada una de estas.

A continuación, se comparte el enlace al proyecto de Github correspondiente a la herramienta Pspy:

* **Herramienta Pspy**: [https://github.com/DominicBreuker/pspy](https://github.com/DominicBreuker/pspy)
