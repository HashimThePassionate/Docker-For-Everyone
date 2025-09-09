# Starting and Stopping Applications with Docker Compose 🐳

## Introduction 🌟

Docker Compose is a powerful tool that allows you to define and manage multi-container Docker applications using a single configuration file, `docker-compose.yml`. This file enables you to configure and control your services, such as web servers, databases, and APIs, in one place, making it easier to deploy, start, and stop your applications. In this guide, we’ll walk through the process of starting and stopping applications using Docker Compose, explain the relevant commands, and provide detailed examples with outputs to help you understand the process thoroughly.

## Starting Applications with Docker Compose 🚀

### Command: `docker-compose up` 🛠️

The `docker-compose up` command is used to build, create, and start all the services defined in your `docker-compose.yml` file. You can run this command with different options depending on your needs.

#### 1. Basic Usage

```bash
docker-compose up
```

- **Description**: This command will start all the services defined in the `docker-compose.yml` file. If the images are not already built, it will build them first.

#### 2. Detached Mode (`-d` Option)

```bash
docker-compose up -d
```

- **Description**: Running `docker-compose up -d` will start the services in detached mode, meaning the containers will run in the background. This is useful when you want to start your application without tying up your terminal.
  
**Output Example:**

```bash
[+] Building 5.3s (21/21) FINISHED
 => [api internal] load build definition from Dockerfile  0.0s
 => [web internal] load metadata for docker.io/library/node:14.16.0-alpine3.13  3.4s
 => [api auth] library/node:pull token for registry-1.docker.io  0.0s
 => [api internal] load .dockerignore  0.0s
 => => transferring context: 53B  0.0s
 => [web 1/6] FROM docker.io/library/node:14.16.0-alpine3.13@sha256:2c51dc462a02f15621e7486774d36d048a27225d581374b002b8477a79a59b4b  0.0s
 => [api internal] load build context  0.0s
 => => transferring context: 712B  0.0s
 => CACHED [web 2/6] RUN addgroup app && adduser -S -G app app  0.0s
 => CACHED [web 3/6] WORKDIR /app  0.0s
 => CACHED [api 4/6] COPY package*.json ./  0.0s
 => CACHED [api 5/6] RUN npm install  0.0s
 => CACHED [api 6/6] COPY . .  0.0s
 => [api] exporting to image  0.1s
 => => exporting layers  0.0s
 => => writing image sha256:f1ed740db11d4c9b377c5cb6696600e42e89e5faab4a19f2d0f8a717b31d000c  0.0s
 => => naming to docker.io/library/07_starting_and_stopping_applications-api  0.0s
 => [api] resolving provenance for metadata file  0.0s
 => [web internal] load build definition from Dockerfile  0.1s 
 => => transferring dockerfile: 216B  0.0s 
 => [web internal] load .dockerignore  0.1s 
 => => transferring context: 53B  0.0s 
 => [web internal] load build context  0.1s 
 => => transferring context: 1.15kB  0.0s 
 => CACHED [web 4/6] COPY package*.json ./  0.0s 
 => CACHED [web 5/6] RUN npm install  0.0s 
 => CACHED [web 6/6] COPY . .  0.0s 
 => [web] exporting to image  0.1s 
 => => exporting layers  0.0s 
 => => writing image sha256:1c35356f5f47256b210e1666929f08cbd465ba7a5d9e02311c65951f27cfae3e  0.0s 
 => => naming to docker.io/library/07_starting_and_stopping_applications-web  0.0s
 => [web] resolving provenance for metadata file  0.0s
[+] Running 5/5
 ✔ Network 07_starting_and_stopping_applications_default  Created  0.2s
 ✔ Volume "07_starting_and_stopping_applications_vidly"   Created  0.1s
 ✔ Container 07_starting_and_stopping_applications-db-1   Started  1.4s
 ✔ Container 07_starting_and_stopping_applications-api-1  Started  2.1s
 ✔ Container 07_starting_and_stopping_applications-web-1  Started  3.2s
```

In this output:
- The `api`, `db`, and `web` containers are created and started.
- The images are built (or reused from cache if they haven’t changed).
- The services are detached, so they continue to run in the background.

### Checking the Status of Services (`docker-compose ps`) 📊

After starting the services, you can check their status using the `docker-compose ps` command:

```bash
docker-compose ps
```

**Output Example:**

```bash
NAME                                          IMAGE                                       COMMAND                  SERVICE   CREATED          STATUS          PORTS
07_starting_and_stopping_applications-api-1   07_starting_and_stopping_applications-api   "docker-entrypoint.s…"   api       44 seconds ago   Up 42 seconds   0.0.0.0:3001->3001/tcp
07_starting_and_stopping_applications-db-1    mongo:4.0-xenial                            "docker-entrypoint.s…"   db        44 seconds ago   Up 43 seconds   0.0.0.0:27017->27017/tcp
07_starting_and_stopping_applications-web-1   07_starting_and_stopping_applications-web   "docker-entrypoint.s…"   web       44 seconds ago   Up 41 seconds   0.0.0.0:3000->3000/tcp
```

- **`STATUS`**: Indicates that all services are up and running.
- **`PORTS`**: Shows the port mappings between the host and the container.

## Stopping Applications with Docker Compose 🛑

### Command: `docker-compose down` 🔄

The `docker-compose down` command is used to stop and remove the containers, networks, and volumes created by `docker-compose up`.

```bash
docker-compose down
```

**Output Example:**

```bash
[+] Running 4/4
 ✔ Container 07_starting_and_stopping_applications-web-1  Removed  2.1s 
 ✔ Container 07_starting_and_stopping_applications-api-1  Removed  12.4s 
 ✔ Container 07_starting_and_stopping_applications-db-1   Removed  1.1s 
 ✔ Network 07_starting_and_stopping_applications_default  Removed  0.3s 
```

In this output:
- All containers (`web`, `api`, and `db`) are stopped and removed.
- The network created by Docker Compose is also removed.

## Rebuilding and Restarting Applications 🛠️

### Command: `docker-compose up --build` 🔄

Sometimes, you may need to rebuild your images and restart the services, especially after making changes to the Dockerfile or dependencies. The `--build` option forces Docker Compose to rebuild the images before starting the containers.

```bash
docker-compose up --build
```

**Output Example:**

```bash
[+] Building 17.6s (20/20) FINISHED
 => [api internal] load build definition from Dockerfile  0.1s 
 => => transferring dockerfile: 217B  0.1s 
 => [web internal] load metadata for docker.io/library/node:14.16.0-alpine3.13  3.4s 
 => [api internal] load .dockerignore  0.5s 
 => => transferring context: 53B  0.1s 
 => [web 1/6] FROM docker.io/library/node:14.16.0-alpine3.13@sha256:2c51dc462a02f15621e7486774d36d048a27225d581374b002b8477a79a59b4b  0.0s 
 => [api internal] load build context  0.9s 
 => => transferring context: 712B  0.4s 
 => CACHED [web 2/6] RUN addgroup app && adduser -S -G app app  0.0s 
 => CACHED [web 3/6] WORKDIR /app  0.0

s 
 => CACHED [api 4/6] COPY package*.json ./  0.0s 
 => CACHED [api 5/6] RUN npm install  0.0s 
 => CACHED [api 6/6] COPY . .  0.0s 
 => [api] exporting to image  0.7s 
 => => exporting layers  0.0s 
 => => writing image sha256:f1ed740db11d4c9b377c5cb6696600e42e89e5faab4a19f2d0f8a717b31d000c  0.2s 
 => => naming to docker.io/library/07_starting_and_stopping_applications-api  0.2s
 => [api] resolving provenance for metadata file  1.2s 
 => [web internal] load build definition from Dockerfile  1.2s 
 => => transferring dockerfile: 216B  0.0s 
 => [web internal] load .dockerignore  0.4s 
 => => transferring context: 53B  0.0s 
 => [web internal] load build context  0.3s 
 => => transferring context: 1.15kB  0.0s 
 => CACHED [web 4/6] COPY package*.json ./  0.0s 
 => CACHED [web 5/6] RUN npm install  0.0s
 => CACHED [web 6/6] COPY . .  0.0s 
 => [web] exporting to image  0.4s 
 => => exporting layers  0.0s 
 => => writing image sha256:1c35356f5f47256b210e1666929f08cbd465ba7a5d9e02311c65951f27cfae3e  0.1s 
 => => naming to docker.io/library/07_starting_and_stopping_applications-web  0.1s 
 => [web] resolving provenance for metadata file  0.0s 
[+] Running 4/4
 ✔ Network 07_starting_and_stopping_applications_default  Created  0.6s 
 ✔ Container 07_starting_and_stopping_applications-db-1   Created  3.3s 
 ✔ Container 07_starting_and_stopping_applications-api-1  Created  2.6s 
 ✔ Container 07_starting_and_stopping_applications-web-1  Created  0.3s 
```

- **`--build`**: Forces Docker Compose to rebuild the images before starting the services.
- This command is particularly useful when you’ve made changes to your Dockerfiles or any dependencies and want to ensure that your containers are running with the latest code.

## Application Logs and Monitoring 📝

### Viewing Logs (`docker-compose logs`) 📄

To view the logs of all running services, you can use the `docker-compose logs` command:

```bash
docker-compose logs
```

This will display the logs of all the services, which can be helpful for debugging or monitoring the application.

### Monitoring Resource Usage (`docker-compose top` and `docker stats`) 📊

You can monitor the running processes in your containers with:

```bash
docker-compose top
```

For more detailed resource usage statistics, use:

```bash
docker stats
```

This provides a live stream of resource usage statistics (CPU, memory, etc.) for your running containers.

## Conclusion 🎉

Docker Compose makes it incredibly easy to start, stop, and manage multi-container applications. Whether you're developing, testing, or deploying your applications, Docker Compose provides a consistent environment that streamlines your workflow. By using commands like `docker-compose up`, `docker-compose down`, and `docker-compose logs`, you can efficiently manage your containers and ensure that your applications are always running smoothly.

With Docker Compose, you can focus more on building great applications and less on managing the infrastructure. 🚀