# Docker Cheat Sheet

A quick reference for common Docker commands.

---

## Containers

### `docker run` - Create and start a container

```
docker run [options] <image>
```

|Flag|Example|What it does|
|---|---|---|
|`-d`|`docker run -d nginx`|Run in background (detached)|
|`-it`|`docker run -it ubuntu bash`|Interactive terminal (attach a shell)|
|`-p`|`docker run -p 8080:80 nginx`|Map host port → container port|
|`--name`|`docker run --name web nginx`|Assign a name to the container|
|`-e`|`docker run -e DB_PASS=secret mysql`|Set an environment variable|
|`-v`|`docker run -v mydata:/app nginx`|Mount a volume|
|`--rm`|`docker run --rm alpine echo hi`|Auto-remove container when it stops|
|`--restart`|`docker run --restart always nginx`|Restart policy (`always`, `unless-stopped`)|

### Managing containers

|Command|What it does|
|---|---|
|`docker ps`|List running containers|
|`docker ps -a`|List all containers, including stopped|
|`docker stop <name>`|Gracefully stop a container|
|`docker start <name>`|Start a stopped container|
|`docker rm <name>`|Remove a stopped container|
|`docker rm -f <name>`|Force-remove a running container|
|`docker logs <name>`|Print container output|
|`docker logs -f <name>`|Tail container output in real time|
|`docker exec -it <name> bash`|Open a shell inside a running container|

---

## Images

|Command|What it does|
|---|---|
|`docker pull nginx`|Download an image from Docker Hub|
|`docker pull nginx:1.25`|Pull a specific version (tag)|
|`docker images`|List locally available images|
|`docker rmi nginx`|Remove a local image|
|`docker build -t myapp .`|Build an image from a `Dockerfile` in the current directory|
|`docker build -t myapp:v2 .`|Build and tag with a version|

---

## Volumes

Named volumes persist data beyond the container lifecycle.

|Command|What it does|
|---|---|
|`docker volume create mydata`|Create a named volume|
|`docker volume ls`|List all volumes|
|`docker volume rm mydata`|Remove a volume|
|`docker run -v mydata:/app nginx`|Mount named volume into a container|
|`docker run -v /host/path:/app nginx`|Bind-mount a host directory instead|

---

## Docker Compose

Compose manages multi-container apps defined in `docker-compose.yml`.

|Command|What it does|
|---|---|
|`docker compose up -d`|Start all services in the background|
|`docker compose down`|Stop and remove containers|
|`docker compose ps`|Show service status|
|`docker compose logs -f`|Tail logs for all services|
|`docker compose exec web bash`|Open a shell in a running service|
|`docker compose down -v`|Tear down and also remove volumes|

---

## Cleanup

|Command|What it does|
|---|---|
|`docker system df`|Show disk usage by Docker|
|`docker system prune`|Remove all stopped containers and unused resources|
|`docker system prune -a`|Also remove unused images|

---

## Common Patterns

**Run a temporary container and discard it:**

```bash
docker run --rm -it ubuntu bash
```

**Run a web server and expose it locally:**

```bash
docker run -d --name web -p 8080:80 nginx
```

**Pass environment variables and mount a volume:**

```bash
docker run -d -e DB_PASS=secret -v mydata:/var/lib/mysql mysql
```

**Build and immediately run your app:**

```bash
docker build -t myapp . && docker run -d -p 3000:3000 myapp
```