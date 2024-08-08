# Git

> **Git** es una herramienta de control de versiones que permite gestionar y registrar los cambios en los archivos de un proyecto. Facilita la colaboración entre múltiples personas, permitiendo que trabajen en paralelo sin sobrescribir el trabajo de los demás.
>
> Cada cambio se guarda en un "commit", y se puede revertir o comparar con versiones anteriores. Git también soporta la creación de ramas, lo que permite experimentar y desarrollar nuevas características de manera aislada antes de integrarlas al proyecto principal.

Una web que funciona con **git** es [github](https://github.com), que es una plataforma en línea que aloja proyectos que utilizan Git para el control de versiones. Ofrece un espacio para almacenar repositorios de código, colaborar en ellos y compartirlos con otros usuarios.

A continuación comandos interesantes para el uso en terminal de esta utilidad:

```bash
git clone http:/........           # clona el repo
git log                            # muestra los commits existentes en un proyecto
git show id                        # muestra los cambios que se han aplicado para un punto dado del proyecto
git branch -a                      # muestra las ramas existentes en el proyecto
git checkout "rama"                # cambia a la rama "rama"
git tag                            # lista las tags del proyecto (una tag sirve como una rama firmada que no permuta, es un nombre que se puede utilizar para marcar un punto en el log del repo)
git show "tag"                     # muestra la tag 
git add -f file                    # añade al commit el archivo especifico file
git commit -m "comentario"         # commitea el commit actual con comentario "comentario"
git push -u origin master          # sube el archivo a la rama master

#Para descargar alguna carpeta de un repositorio pero no queremos el repositorio entero hacemos lo siguiente:
svn checkout https://github.com/directorio/trunk/a/descargar          #(donde hemos eliminado el /tree/master y lo hemos sustituido por trunk)
```
