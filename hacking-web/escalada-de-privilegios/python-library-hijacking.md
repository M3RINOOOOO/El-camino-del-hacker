# Python Library Hijacking

Cuando hablamos de ‘**Python Library Hijacking**‘, a lo que nos referimos es a una técnica de ataque que aprovecha la forma en la que Python busca y carga bibliotecas para inyectar código malicioso en un script. El ataque se produce cuando un atacante crea o modifica una biblioteca en una ruta accesible por el script de Python, de tal manera que cuando el script la importa, se carga la versión maliciosa en lugar de la legítima.

La forma en que el ataque se lleva a cabo es la siguiente: el atacante busca una biblioteca utilizada por el script y la reemplaza por su propia versión maliciosa. Esta biblioteca puede ser una biblioteca estándar de Python o una biblioteca externa descargada e instalada por el usuario. El atacante coloca su versión maliciosa de la biblioteca en una ruta accesible antes de que la biblioteca legítima sea encontrada.

En general, Python comienza buscando estas bibliotecas en el directorio actual de trabajo y luego en las rutas definidas en la variable sys.path. Si el atacante tiene acceso de escritura en alguna de las rutas definidas en sys.path, puede colocar allí su propia versión maliciosa de la biblioteca y hacer que el script la cargue en lugar de la legítima.

Además, el atacante también puede crear su propia biblioteca en el directorio actual de trabajo, ya que Python comienza la búsqueda en este directorio por defecto. Si durante la carga de estas librerías desde el script legítimo, el atacante logra secuestrarlas, entonces conseguirá una ejecución alternativa del programa.
