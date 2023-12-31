# Explotación del Kernel

El **kernel** es la parte central del sistema operativo Linux, que se encarga de administrar los recursos del sistema, como la memoria, los procesos, los archivos y los dispositivos. Debido a su papel crítico en el sistema, cualquier vulnerabilidad en el kernel puede tener graves consecuencias para la seguridad del sistema.

En versiones antiguas del kernel de Linux, se han descubierto vulnerabilidades que pueden ser explotadas para permitir a los atacantes obtener acceso de superusuario (**root**) en el sistema.

La elevación de privilegios se refiere a la técnica utilizada por los atacantes para obtener permisos elevados en el sistema, como superusuario (**root**), cuando solo tienen permisos limitados. Por ejemplo, un usuario con permisos limitados en el sistema podría utilizar una vulnerabilidad en el kernel para obtener acceso de superusuario y, posteriormente, comprometer el sistema.

Las vulnerabilidades del kernel pueden ser explotadas de varias maneras. Por ejemplo, un atacante podría aprovechar una vulnerabilidad en un controlador de dispositivo para obtener acceso al kernel y realizar operaciones maliciosas. Otra forma común en que se explotan las vulnerabilidades del kernel es mediante el uso de técnicas de desbordamiento de búfer, que permiten a los atacantes escribir código malicioso en áreas de memoria reservadas para el kernel.

Para mitigar el riesgo de vulnerabilidades del kernel, es importante mantener actualizado el sistema operativo y aplicar parches de seguridad tan pronto como estén disponibles.

A continuación, se os comparte el enlace a la máquina Sumo 1 de Vulnhub, la cual estaremos desplegando en esta clase para mostrar un ejemplo práctico de explotación del kernel:

* **Máquina Sumo 1**: [https://www.vulnhub.com/entry/sumo-1,480/](https://www.vulnhub.com/entry/sumo-1,480/)
