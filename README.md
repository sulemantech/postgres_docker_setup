# Postgresql and PgAdmin on Docker

This repository contains instructions on how to install and configure Postgresql and PgAdmin on Docker. 

## Installation

To get started, follow these steps:

1. Create a volume for Postgresql:

docker volume create --name postgres_volume_local -d local

2. Create a network for Postgresql:
docker network create pg-network


3. Pull and run the Postgresql container:

docker run -it --rm -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=shah@1234 -e POSTGRES_DB=erp_db -v postgres_volume_local:/var/lib/postgresql/data -p 5432:5432 --network=pg-network --name pg-database postgres

4. Pull and run the PgAdmin container:

docker run -it --rm -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" -e PGADMIN_DEFAULT_PASSWORD="root" -p 8080:80 --network=pg-network --name pgadmin dpage/pgadmin4

5. Copy the backup file to the volume:

docker cp dev-backup pg-database:/var/lib/postgresql/data

6. Open a command prompt in the Postgresql container:
docker exec -it pg-database /bin/bash

7. Restore the database:
pg_restore -U postgres -d erp_db -c dev-backup

## Support

If you have any questions or issues, please contact us at support@company.com.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
