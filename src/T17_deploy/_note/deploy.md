# Deploy the application with Docker

> [!IMPORTANT]
> Replace values such as `gXX`, `40XX`, `50XX`, `60XX`, and `dockerhub_userXXX`
> with the values assigned to your group.

## 1. Create a Docker volume and network

The volume keeps database files when the database container is removed. The
network lets the containers find one another by container name.

```bash
DOCKER_VOLUME_NAME='gXX_volume'
DOCKER_NETWORK_NAME='gXX_network'

docker volume create ${DOCKER_VOLUME_NAME}
docker network create ${DOCKER_NETWORK_NAME}
```

## 2. Start PostgreSQL

```bash
DB_IMAGE_NAME='postgres:latest'
DB_CONTAINER_NAME='gXX_db'
DB_PASSWORD='6&zi9!%BYCUZo6B'
DB_PORT='40XX'

docker run -d --name ${DB_CONTAINER_NAME} \
	-e POSTGRES_PASSWORD=${DB_PASSWORD} \
	-v ${DOCKER_VOLUME_NAME}:/var/lib/postgresql \
	--network ${DOCKER_NETWORK_NAME} \
	-p ${DB_PORT}:5432 \
	${DB_IMAGE_NAME}
```

### Create the application database and table

Open a shell in the database container, then start PostgreSQL's interactive
client:

```bash
docker exec -it ${DB_CONTAINER_NAME} bash
psql -U postgres
```

Run these commands at the `psql` prompt:

```sql
CREATE DATABASE app_db;
\c app_db

CREATE TABLE IF NOT EXISTS todos (
	id serial PRIMARY KEY,
	title VARCHAR(50) NOT NULL,
	completed BOOLEAN NOT NULL DEFAULT FALSE,
	created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
```

Exit `psql` and the container shell:

```text
\q
exit
```

## 3. Start the backend

The backend connects to PostgreSQL using the database container's name as its
host.

```bash
BACKEND_CONTAINER_NAME='gXX_backend'
BACKEND_PORT='50XX'

PGUSER='postgres'
PGHOST=${DB_CONTAINER_NAME}
PGDATABASE='app_db'
PGPASSWORD=${DB_PASSWORD}
PGPORT='5432'

BACKEND_IMAGE_NAME_TAG='dockerhub_userXXX/gXX_backend_image:latest'

docker run -d --name ${BACKEND_CONTAINER_NAME} \
	-p ${BACKEND_PORT}:3000 \
	--network ${DOCKER_NETWORK_NAME} \
	-e PGUSER=${PGUSER} \
	-e PGHOST=${PGHOST} \
	-e PGDATABASE=${PGDATABASE} \
	-e PGPASSWORD=${PGPASSWORD} \
	-e PGPORT=${PGPORT} \
	${BACKEND_IMAGE_NAME_TAG}
```

## 4. Start the frontend

The frontend's Nginx proxy forwards API requests to the backend container.

```bash
FRONTEND_CONTAINER_NAME='gXX_frontend'
FRONTEND_PORT='60XX'
BACKEND_URL="http://${BACKEND_CONTAINER_NAME}:3000"
FRONTEND_IMAGE_NAME_TAG='dockerhub_userXXX/gXX_frontend_image:latest'

docker run -d --name ${FRONTEND_CONTAINER_NAME} \
	-p ${FRONTEND_PORT}:81 \
	--network ${DOCKER_NETWORK_NAME} \
	-e NGINX_PROXY_PASS=${BACKEND_URL} \
	${FRONTEND_IMAGE_NAME_TAG}
```
