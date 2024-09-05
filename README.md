Esta configuración de docker compose permite correr un contenedor mysql guardando de forma persistente los datos de la BD.

La carpeta `bd` contiene toda la información necesaria para mysql

- src: Es una carpeta visible dentro del contenedor en /app, utilizada para poner el codigo fuente de java y sql.

# Inicio

Para crear/correr el contenedor docker podemos utilizar docker compose con el siguiente comando: `docker compose up -d`

El comando levantará el contenedor si ya se encuentra creado, y si no lo encuentra, descargará la imagen y creará un nuevo contenedor.

# Errores

Si se produce algún error, se recomienda observar la salida que produjo el mismo para identificar el origen. 
Los errores más comunes se deben a no tener corriendo el servicio docker al momento de ejecutar el comando, o estar ubicado en una carpeta donde no se encuentra el archivo `docker-compose.yml`

Al intentar correr el servidor de mysql puede ser que ocurra un error porque el puerto ya se encuentra asignado a otro programa. 
En dicho caso, es suficiente con cambiar el puerto asignado en el archivo `docker-compose.yml` cambiando la sección de `ports:`. 
Por ejemplo, con la siguiente definición decimos que en la maquina local utilice el puerto 6603 y lo mapee al 3306 del contenedor. 

`
    ports:
      - "6603:3306"
`

Puede ser que se produzca una advertencia (aunque creemos que está solucionado en la versión actual) sobre los permisos que tiene el archivo `config-file.cnf`.

Desde la linea de comando se puede ejecutar
`docker exec -i mysql_batallas_2024 chmod 0400 /etc/mysql/conf.d/config-file.cnf`

o ingresar al contenedor
`docker exec -it mysql_batallas_2024 sh`
y luego dentro del mismo ejecutar
`chmod 0400 /etc/mysql/conf.d/config-file.cnf`

# Ingresar al contenedor desde una terminar en modo interactivo

Genéricamente se utiliza el siguiente template
`docker exec -it <Contenedor ID> sh`

Específicamente para este proyecto se puede utilizar
`docker exec -it mysql_batallas_2024 sh`

Una vez que se encuentra dentro del contenedor puede ejecutar cualquier comando que se encuentre instalado. En el caso de mysql, puede ejecutar `mysql -uroot -p1234` para ingresar a la consola de mysql.

# Procesamiento de Scripts .sql

Para procesar un script .sql en la base de datos, se puede utilizar el siguiente comando desde la terminal de la máquina local.

En Windows utilizando el powershell se puede utilizar el siguiente comando
`Get-Content src/sql/batallas.sql | docker exec -i mysql_batallas_2024 mysql -uroot -p1234 mysql`

ya que no funciona la redirección de archivos con `<` y `>`. Por ejemplo,
`docker exec -i mysql_batallas_2024 mysql -uroot -p1234 mysql < src/sql/batallas.sql`

aunque se recomienda ingresar al contenedor y ejecutar el script desde la consola de mysql.
`docker exec -it mysql_batallas_2024 sh`
`mysql -uroot -p1234 mysql < /app/src/sql/batallas.sql`
o bien
`mysql -uroot -p` y luego ingresar la contraseña `1234` y ejecutar `\. /app/src/sql/batallas.sql`