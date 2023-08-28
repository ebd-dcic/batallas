Esta configuración de docker compose permite correr un contenedor
mysql guardando de forma persistente los datos de la BD.

La carpeta data/db contiene toda la información necesaria para mysql

- src: Es una carpeta visible dentro del contenedor, en /app, utilizada para poner el codigo fuente de java y sql.

- docker exec -i mysql_container mysql -uroot -psecret mysql < db.sql

	docker exec -i mysql_prueba mysql -uroot -p1234 mysql < src/sql/batallas.sql
	docker exec -i mysql_prueba mysql -uroot -p1234 mysql < src/sql/borrar.sql