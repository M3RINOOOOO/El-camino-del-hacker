# Detección y explotación de Capabilities

En sistemas Linux, las capabilities son una funcionalidad de seguridad que permite a los usuarios realizar acciones que normalmente requieren privilegios de superusuario (**root**), sin tener que concederles acceso completo de superusuario. Esto se hace para mejorar la seguridad, ya que un proceso que solo necesita ciertos privilegios puede obtenerlos sin tener que ejecutarse como root.

Las capabilities se dividen en 3 tipos:

* **Permisos efectivos (effective capabilities)**: son los permisos que se aplican directamente al proceso que los posee. Estos permisos determinan las acciones que el proceso puede realizar. Por ejemplo, la capability “**CAP\_NET\_ADMIN**” permite al proceso modificar la configuración de red.
* **Permisos heredados (inheritable capabilities)**: son los permisos que se heredan por los procesos hijos que son creados. Estos permisos pueden ser adicionales a los permisos efectivos que ya posee el proceso padre. Por ejemplo, si un proceso padre tiene la capability “**CAP\_NET\_ADMIN**” y esta capability se configura como heredable, entonces los procesos hijos también tendrán la capability “**CAP\_NET\_ADMIN**“.
* **Permisos permitidos (permitted capabilities)**: son los permisos que un proceso tiene permitidos. Esto incluye tanto permisos efectivos como heredados. Un proceso solo puede ejecutar acciones para las que tiene permisos permitidos. Por ejemplo, si un proceso tiene la capability “**CAP\_NET\_ADMIN**” y la capability “**CAP\_SETUID**” configurada como permitida, entonces el proceso puede modificar la configuración de red y cambiar su **UID** (**User ID**).

Ahora bien, algunas capabilities pueden suponer un riesgo desde el punto de vista de la seguridad si se les asignan a determinados binarios. Por ejemplo, la capability **cap\_setuid** permite a un proceso establecer el UID (User ID) de un proceso a otro valor diferente al suyo, lo que puede permitir que un usuario malintencionado ejecute código malicioso con privilegios elevados.

Para **listar** las capabilities de un archivo binario en Linux, puedes usar el comando **getcap**. Este comando muestra las capabilities efectivas, heredables y permitidas del archivo. Por ejemplo, para ver las capabilities del archivo binario **/usr/bin/ping**, puedes ejecutar el siguiente comando en la terminal:

➜ `getcap /usr/bin/ping`

La salida del comando mostrará las capabilities asignadas al archivo:

➜ `/usr/bin/ping = cap_net_admin,cap_net_raw+ep`

En este caso, el binario ping tiene dos capabilities asignadas: **cap\_net\_admin** y **cap\_net\_raw+ep**. La última capability (**cap\_net\_raw+ep**) indica que el archivo tiene el bit de ejecución elevado (**ep**) y la capability **cap\_net\_raw** asignada.

Para **asignar** una capability a un archivo binario, puedes utilizar el comando **setcap**. Este comando establece las capabilities efectivas, heredables y permitidas para el archivo especificado.

Por ejemplo, para otorgar la capability **cap\_net\_admin** al archivo binario **/usr/bin/my\_program**, puedes ejecutar el siguiente comando en la terminal:

➜ `sudo setcap cap_net_admin+ep /usr/bin/my_program`

En este caso, el comando otorga la capability **cap\_net\_admin** al archivo **/usr/bin/my\_program**, y también establece el bit de ejecución elevado (**ep**). Ahora, el archivo my\_program tendrá permisos para administrar la configuración de red.

El **bit de ejecución elevado** (en inglés, elevated execution bit o “**ep**“) es un atributo especial que se puede establecer en un archivo binario en Linux. Este atributo se utiliza en conjunción con las capabilities para permitir que un archivo se ejecute con permisos especiales, incluso si el usuario que lo ejecuta no tiene privilegios de superusuario.

Cuando un archivo binario tiene el bit de ejecución elevado establecido, se puede ejecutar con las capabilities efectivas asignadas al archivo, en lugar de las capabilities del usuario que lo ejecuta. Esto significa que el archivo puede realizar acciones que normalmente solo están permitidas a los usuarios con privilegios elevados.

Es importante señalar que los permisos permitidos pueden ser limitados aún más mediante el uso de un mecanismo de control de acceso obligatorio (MAC, Mandatory Access Control), como **SELinux** o **AppArmor**, que restringen las acciones que los procesos pueden realizar en función de la política de seguridad del sistema.
