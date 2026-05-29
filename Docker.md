# Docker Notes

## What is Docker?

Docker is a platform used to package applications and their dependencies into isolated environments called containers.

A container includes:

- the application
- required libraries
- runtime dependencies
- configuration

This allows the application to run consistently across different machines and environments.

---

# Docker Concepts

## Images

A Docker image is a blueprint or template used to create containers.

Images are:

- read-only
- reusable
- used to start containers

Example image:

```bash id="c1a5ez"
docker/welcome-to-docker:latest
```

View images:

```bash id="h6pv27"
docker images
```

---

## Containers

A container is a running instance of an image.

Containers are:

- isolated environments
- created from images
- capable of running services/applications

View running containers:

```bash id="k2xvfp"
docker ps
```

View all containers:

```bash id="3r7tv8"
docker ps -a
```

---

# Renaming Containers

Containers can be renamed for easier management.

Example:

```bash id="7vch4n"
docker rename welcome-to-docker service
```

Where:

- `welcome-to-docker` = current container name
- `service` = new container name

---

# Tagging (Renaming) Images

Docker images are not directly renamed. Instead, a new tag is created.

Example:

```bash id="0t6f5v"
docker tag docker/welcome-to-docker:latest my-service
```

This creates a new image name:

- `my-service`

Both names point to the same underlying image data.

---

# Running Applications with Docker Compose

`docker-compose.yml` is used to define and run multiple Docker services using a single configuration file.

Benefits:

- simpler container management
- reusable configuration
- environment variable management
- easier networking between services

---

# Example docker-compose.yml

```yaml id="df8kcl"
services:
  app:
    image: my-service
    container_name: service

    ports:
      - "3000:80"

    environment:
      DB_HOST: host.docker.internal
      DB_PORT: 5432
      DB_USER: postgres
      DB_PASSWORD: yourpassword
      DB_NAME: yourdatabase
```

---

# Understanding `image` vs `container_name`

```yaml id="q4s93g"
services:
  app:
    image: my-service
    container_name: service
```

These two properties are related, but they serve different purposes.

---

## `image`

```yaml id="a7nk9y"
image: my-service
```

The `image` field tells Docker:

> Use this image as the blueprint for the container.

Docker searches:

1. local Docker images
2. Docker Hub / remote registries if not found locally

Example:

```bash id="n6u3yw"
docker images
```

Output:

```txt id="2r1mc9"
REPOSITORY    TAG       IMAGE ID
my-service    latest    abc123
```

Docker Compose will use this image to create the container.

---

## How Docker Identifies Images

Docker images are identified by:

- image name
- optional tag

Example:

```yaml id="n4g7xv"
image: my-service
```

Actually means:

```txt id="mzv9w2"
my-service:latest
```

Specifying a tag:

```yaml id="p8j6yx"
image: my-service:v1
```

means:

- image name = `my-service`
- tag = `v1`

---

## `container_name`

```yaml id="y3w5qu"
container_name: service
```

This sets the name of the running container instance.

It does NOT rename the image.

Instead, it names the container created FROM the image.

---

# Relationship Between Images and Containers

Think of it like this:

```txt id="1r2e7u"
Image = Blueprint
Container = Running instance of the blueprint
```

Example:

```yaml id="o9d8ca"
image: my-service
container_name: service
```

Means:

```txt id="5z3mvr"
Create a container named "service"
using the image named "my-service"
```

---

# Example Workflow

## 1. Create/tag an image

```bash id="9b4vtn"
docker tag docker/welcome-to-docker:latest my-service
```

Now Docker has a local image named:

```txt id="3s0wph"
my-service
```

---

## 2. Docker Compose Uses That Image

```yaml id="v8n0tx"
services:
  app:
    image: my-service
```

Docker Compose searches for:

- `my-service`

If found locally:

- it uses it immediately

If not found locally:

- Docker attempts to pull it from Docker Hub

---

# Understanding Port Mapping

Port mapping format:

```yaml id="0g8yul"
HOST_PORT:CONTAINER_PORT
```

Example:

```yaml id="4t1prq"
3000:80
```

Meaning:

- `3000` = port on the host machine (Mac)
- `80` = port inside the container

The application runs internally on port `80`, but is accessed externally using:

```txt id="7u4v3q"
http://localhost:3000
```

---

# Connecting Docker to Local PostgreSQL

The PostgreSQL database is running locally on the Mac machine.

From inside Docker:

- `localhost` refers to the container itself
- not the host machine

Docker uses:

```txt id="y1l7es"
host.docker.internal
```

to connect back to the Mac host.

Example PostgreSQL connection string:

```txt id="ep9b7n"
postgresql://postgres:password@host.docker.internal:5432/mydatabase
```

---

# Docker Compose Commands

Start containers:

```bash id="h5z8cd"
docker compose up
```

Start in background:

```bash id="u3x6kp"
docker compose up -d
```

Stop containers:

```bash id="l9f0rn"
docker compose down
```

View logs:

```bash id="g7v1xp"
docker compose logs
```

Restart services:

```bash id="s6q2tb"
docker compose restart
```

---

# Workflow Summary

1. Pull or create a Docker image
2. Create containers from the image
3. Rename/tag images if necessary
4. Configure services using `docker-compose.yml`
5. Map ports between the host and container
6. Connect the containerized application to PostgreSQL
7. Run services using Docker Compose
