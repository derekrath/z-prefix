# z_prefix

If you are using Docker to store your data, prior to running backend, open Docker app and run the container in which you will be storing your data. Will be utilizing the Postgres image in this app. Steps to set up and run a postgres container:

docker pull postgres
<!-- fails unless I upgrade to postgres 15.2 -->

docker pull postgres:14.1
<!-- success, installs v 14.1 -->

docker run --name (name of container) -e POSTGRES_PASSWORD=(password) -d -p 5432:5432 -v $HOME/docker/volumes/postgres:/var/lib/postgresql/data postgres

docker run --name postgres -e POSTGRES_PASSWORD=docker -d -p 5432:5432 -v $HOME/docker/volumes/postgres:/var/lib/postgresql/data postgres

docker run --name db -e POSTGRES_PASSWORD=docker -d -p 5432:5432 -v $HOME/docker/volumes/postgres:/var/lib/postgresql/data postgres:14.1
<!-- success -->

if docker container is already created: 

docker run (container name)

docker exec -it (container name) /bin/bash, you are now in the shell of your container

docker exec -it db /bin/bash, you are now in the shell of your container
<!-- success -->

psql -U postgres, running your image in your container
<!-- success -->

Now in your container you can CREATE DATABASE (database name) or \c into an already created database. To view tables when in a database: \d or to look at a specific table \dt (table name)
createdb -U db postgres

Now in your container you can 

CREATE DATABASE postgres
<!-- success -->

<!-- DROP TABLE users_table; -->
<!-- DROP DATABASE postgres; -->

 or \c into an already created database. To view tables when in a database: \d or to look at a specific table \dt (table name)
<!-- success -->

When done, ^C to escape out and \q or ctrl+d to exit

Make sure to configure knexfile.js with appropriate connection string with the following template: 

connection: '(image)://(image):(password)@localhost/(database name)'
connection: 'postgres://postgres:docker@localhost/postgres'

Once knex is configured and container has been started, 

npx knex migrate:latest and npx knex seed:run 
<!-- success, after I renamed 'users' to 'users_table' because its a reserved word -->

to populate database with character information

npm start 

to bring your backend up so frontend can connect and retrieve data from it.

you need to change the host 127.0.0.1/32 to "all"  in the database folder pg_hba.conf to grant access to the database

If you use the migrations key in docker-compose.yml then you might be migrating twice

Had to add to db docker-compose.yaml     command: postgres -c stats_temp_directory=/tmp

