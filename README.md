## Containerize an Application
- `docker build -t getting-started .`
- `docker run --name getting-started -dp 3000:3000 getting-started`
- `docker ps -a`
- Visit `localhost:3000` to verify that container is running.
- `docker rm -f getting-started`

## Persist the DB - Docker Volumes
- `docker volume create todo-db`
- `docker run --name getting-started -dp 3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started`
- Visit to `localhost:3000` and add some items
- `docker rm -f getting-started`
- `docker run --name getting-started -dp 3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started`
- Visit to `localhost:3000` & verify that data is still present
- `docker volume inspect todo-db`
- `docker volume rm todo-db`

### Bind Mounts
The following are examples of a named volume and a bind mount using `--mount`:
- **Named volume:** `type=volume,src=my-volume,target=/usr/local/data`
- **Bind mount:** `type=bind,src=/path/to/data,target=/usr/local/data`

The following table outlines the main differences between volume mounts and bind mounts.


| | Named volumes | Bind mounts |
| --- | --- | --- |
| Host location |	Docker chooses |	You decide |
| Populates new volume with container contents | Yes |	No |
| Supports Volume Drivers |	Yes |	No |

## Multi Container Apps
- `docker network create todo-app`
- `docker run -d --name todo-mysql --network todo-app --network-alias my-sql -v todo-mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=todos mysql:8.0`
- `docker run --name getting-started -dp 3000:3000 --network todo-app -e MYSQL_HOST=my-sql -e MYSQL_USER=root -e MYSQL_PASSWORD=secret -e MYSQL_DB=todos getting-started`
- `docker rm -f getting-started`
- `docker rm -f todo-mysql`