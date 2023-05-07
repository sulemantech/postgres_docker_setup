# postgres_docker_setup
How to install Postgresql and PgAdmin on Docker and restore DB
CREATE VOLUME FOR THE POSTGRES
docker volume create --name postgres_volume_local -d local

CREATE THE DOCKER NETWORK FOR THE POSTGRES
docker network create pg-network

PULL & RUN THE POSTGRES CONTAINER
docker run -it --rm -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=shah@1234 -e POSTGRES_DB=erp_db -v postgres_volume_local:/var/lib/postgresql/data -p 5432:5432 --network=pg-network --name pg-database postgres

PULL & RUN THE PgAdmin
docker run -it --rm -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" -e PGADMIN_DEFAULT_PASSWORD="root" -p 8080:80 --network=pg-network --name pgadmin dpage/pgadmin4

COPY THE BACKUP TO THE Volume
docker cp dev-backup pg-database:/var/lib/postgresql/data

RUN THE COMMAND PROMPT
docker exec -it pg-database /bin/bash

RUN THE RESTORE
docker exec -it pg-database pg_restore -U postgres -d erp_db -c dev-backup
