# Enumeración de subdominios

Los subdominios son partes de un dominio más grande y suelen estar configurados para apuntar a diferentes recursos de red, como servidores web, servidores de correo electrónico, sistemas de bases de datos y sistemas de gestión de contenido. Identificar los subdominios vinculados a un dominio principal puede proporcionar información valiosa y posibles vectores de ataque.

### Herramientas para la enumeración de subdominios

#### Herramientas pasivas

Las herramientas pasivas obtienen información sobre los subdominios sin enviar solicitudes directas a los servidores identificados. Son útiles para recolectar datos sin interactuar directamente con los sistemas objetivos.

* **Phonebook.cz**: Permite buscar subdominios y otros datos de contacto relacionados con empresas y dominios.
* **Intelx.io**: Ofrece información sobre subdominios, direcciones de correo electrónico y otros detalles relacionados con dominios.
* **CTFR**: Utiliza registros de certificados SSL/TLS para encontrar subdominios asociados a un dominio. Es una herramienta de línea de comandos que realiza búsquedas en los registros de certificados.
* **Sublist3r**: Herramienta de línea de comandos que realiza una búsqueda pasiva de subdominios utilizando varias fuentes de datos y motores de búsqueda.

#### Herramientas activas

Las herramientas activas envían solicitudes a los servidores identificados para encontrar subdominios bajo el dominio principal. Estas herramientas pueden proporcionar información más precisa pero también pueden ser detectadas por los sistemas objetivo.

* **Gobuster**: Herramienta de fuzzing para realizar escaneos de subdominios. Es útil para realizar búsquedas exhaustivas de subdominios.
* **WFuzz**: Herramienta de fuzzing que puede ser utilizada para buscar subdominios utilizando un diccionario de palabras. Permite ajustar diferentes parámetros para afinar los resultados.
