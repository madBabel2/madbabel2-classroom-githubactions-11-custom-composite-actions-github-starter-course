## Objetivo
Explorar la sintaxis de las composite custom actions  y cómo crearlas en GitHub Actions.
Para ello, crearemos una composite action capaz manejar la instalación de las dependencias de npm, y así abstraer todo esa lógica dentro de una acción reutilizable fácil de usar. 

## Tareas
Se ofrece un fichero con el workflow donde se maneja la instalación de las dependencias de npm y tests. Se pide ejecutar todo el Workflow como una 'composite action', para ello:
1. Crear un archivo llamado action.yaml en la carpeta .github/actions/composite-deps. Esta carpeta no existe aún, por lo que es posible que tengas que crearla.
2. En el archivo action.yaml, implmentar toda la lógica de los steps `Setup Node`,`Install Dependencies`,`Run Unit Tests` que contiene el fichero de workflow.yml, realizando las adaptaciones necesarias, teniendo en cuenta que será necesario añadir una key 'runs' a nivel de fichero. Esta es la clave principal para definir nuestra composite custom action. Para una composite action, la key 'runs' tiene la siguiente forma:
```yaml
runs:
  using: composite
  steps: [...] #Similar a los steps de un job
```
   donde:
        - 'using: composite' simplemente informa a GitHub Actions que esta es una composite custom action.
        - 'steps: [...]' contiene la matriz de steps que deseas ejecutar. Esto es muy similar a los steps que ya hemos definido para los jobs en  ejercicios anteriores.


3. Confirmar los cambios y hacer push del código. Tómate unos momentos para entender la sintaxis de la composite custom action y las similitudes y diferencias con los steps de job. Por ahora no pasará nada, ya que esta acción personalizada no se utiliza en ningún flujo de trabajo, así que pasemos al siguiente ejercicio donde crearemos nuestro flujo de trabajo!


4. Modificar el fichero workflow.yml:
   - Nombrar el flujo de trabajo 11 Custom Actions - Composite.
   - desencadenantes:
     - workflow_dispatch: el workflow_dispatch también debería recibir un input llamado target-env, de tipo choice y con las siguientes opciones: dev, prod.
   - Establecer la opción run-name del flujo de trabajo en 11 Custom Actions - Composite | env - 'recuperar el input target-env aquí'.   
   - Trabajos:
     - **build**:
       - Debería ejecutarse en ubuntu-latest.
       - Debería contener 2 steps:
         - El primer step debería hacer checkout del código.
         - El segundo step, llamado Build Node, debería usar la acción personalizada recientemente creada (Punto 2). Para hacer referencia a una acción personalizada creada en el mismo repositorio que el flujo de trabajo, simplemente puedes proporcionar la ruta del directorio donde se encuentra el archivo action.yaml. En este caso, esto sería ./.github/actions/composite-deps.
         - No olvides proporcionar los inputs necesarios a través de la clave 'with'!        

5. Confirmar los cambios y hacer push del código. Desencadenar el flujo de trabajo manualmente desde la interfaz de usuario y tómate unos momentos para inspeccionar el resultado de la ejecución del flujo de trabajo. El job debe acabar correctamente, construyendo la app y ejecutando los test


## Tips



Para ejecutar un step dentro de un directorio de trabajo proporcionado, puedes usar la siguiente sintaxis:
```yaml
working-directory: /the/directory/path
```

Para ejecutar un script bajo el shell bash, puedes usar la siguiente sintaxis:
```yaml
run: echo "hello"
shell: bash
```
