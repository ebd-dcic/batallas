Esta configuración de docker compose permite correr un contenedor
mysql pero guarda de forma persistente los datos de la BD, es decir,
cada vez que se corra el contenedor la BD estará vacía.

- src: Es una carpeta visible dentro del contenedor, en /app, utilizada para poner el codigo fuente de java y sql.