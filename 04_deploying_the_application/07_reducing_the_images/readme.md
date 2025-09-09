# 🚀 Reducing Docker Image Size

Optimizing Docker image sizes is crucial for improving deployment speed, reducing disk space usage, and minimizing network bandwidth. In this guide, we’ll walk through how to reduce the image size for a multi-container application, focusing on the frontend, backend, and database services.

## 🔍 Current Docker Images Overview

Let's start by inspecting the current image sizes:

```bash
docker images
```

### 🖥️ Output:
```plaintext
REPOSITORY                        TAG          IMAGE ID       CREATED         SIZE
07_reducing_the_images-frontend   latest       73c1174d7b7f   3 days ago      299MB
07_reducing_the_images-backend    latest       5da4526a308b   3 days ago      184MB
mongo                             4.0-xenial   fb1435e8841c   24 months ago   430MB
```

The frontend image is 299MB, and the backend image is 184MB. Let's optimize these images!

## 🎨 Optimizing the Frontend Image

### Step 1: Building the Frontend for Production

The `package.json` file contains the following build script:

```json
"scripts": {
  "start": "react-scripts start",
  "build": "react-scripts build",
  "test": "react-scripts test --colors",
  "eject": "react-scripts eject"
}
```

To create an optimized production build, navigate to the `frontend` directory and run:

```bash
cd frontend
npm run build
```

### 🛠️ Output:
```plaintext
> vidly-frontend@0.1.0 build
> react-scripts build

Creating an optimized production build...
Error: error:0308010C:digital envelope routines::unsupported
...
Node.js v20.10.0
```

### Step 2: Handling Errors During Build

You might encounter an error like this:

```plaintext
Error: error:0308010C:digital envelope routines::unsupported
```

To resolve this, ensure you’re using the correct Node.js version. Rename the `Dockerfile prod` to `Dockerfile.prod`:

```bash
mv "Dockerfile prod" Dockerfile.prod
```

### Step 3: Creating a Production Dockerfile for the Frontend

Create a `Dockerfile.prod` in the `frontend` directory:

```Dockerfile
FROM node:14.16.0-alpine3.13 AS build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:1.12-alpine 
RUN addgroup app && adduser -S -G app app
USER app
COPY --from=build-stage /app/build /usr/share/nginx/html
EXPOSE 80
ENTRYPOINT [ "nginx", "-g", "daemon off;" ]
```

### 📝 Explanation:

- **Build Stage**:
  - **`FROM node:14.16.0-alpine3.13`**: Uses a lightweight Node.js base image.
  - **`WORKDIR /app`**: Sets the working directory to `/app`.
  - **`COPY package*.json .`**: Copies `package.json` and `package-lock.json` to the working directory.
  - **`RUN npm install`**: Installs dependencies.
  - **`COPY . .`**: Copies the entire project to the container.
  - **`RUN npm run build`**: Builds the optimized production version of the frontend.

- **Final Stage**:
  - **`FROM nginx:1.12-alpine`**: Uses a lightweight Nginx base image.
  - **`RUN addgroup app && adduser -S -G app app`**: Creates a non-root user for security.
  - **`USER app`**: Switches to the non-root user.
  - **`COPY --from=build-stage /app/build /usr/share/nginx/html`**: Copies the built files from the previous stage to Nginx's default HTML directory.
  - **`EXPOSE 80`**: Exposes port 80 for HTTP traffic.
  - **`ENTRYPOINT [ "nginx", "-g", "daemon off;" ]`**: Runs Nginx in the foreground.

### Step 4: Building the Optimized Image

Now, build the Docker image:

```bash
docker build -t vidly_web_opt -f Dockerfile.prod .
```

### 📦 Output:
```plaintext
[+] Building 21.7s (17/17) FINISHED                                                           docker:default 
 => [internal] load build definition from Dockerfile.prod                                               0.0s 
 => => transferring dockerfile: 352B                                                                    0.0s 
 => [internal] load metadata for docker.io/library/nginx:1.12-alpine                                    2.2s 
 => [internal] load metadata for docker.io/library/node:14.16.0-alpine3.13                              2.2s 
 => [auth] library/node:pull token for registry-1.docker.io                                             0.0s 
 => [auth] library/nginx:pull token for registry-1.docker.io                                            0.0s 
 => [internal] load .dockerignore                                                                       0.1s 
 => => transferring context: 53B                                                                        0.0s 
 => [stage-1 1/3] FROM docker.io/library/nginx:1.12-alpine@sha256:db5acc22920799fe387a903437eb89387607  0.0s 
 => [build-stage 1/6] FROM docker.io/library/node:14.16.0-alpine3.13@sha256:2c51dc462a02f15621e7486774  0.0s 
 => [internal] load build context                                                                       0.0s 
 => => transferring context: 1.72kB                                                                     0.0s 
 => CACHED [stage-1 2/3] RUN addgroup app && adduser -S -G app app                                      0.0s 
 => CACHED [build-stage 2/6] WORKDIR /app                                                               0.0s 
 => CACHED [build-stage 3/6] COPY package*.json ./                                                      0.0s 
 => CACHED [build-stage 4/6] RUN npm install                                                            0.0s 
 => [build-stage 5/6] COPY . .                                                                          0.2s 
 => [build-stage 6/6] RUN npm run build                                                                17.7s 
 => [stage-1 3/3] COPY --from=build-stage /app/build /usr/share/nginx/html                              0.1s
 => exporting to image                                                                                  0.1s
 => => exporting layers                                                                                 0.0s
 => => writing image sha256:d5069e1ff60d59e90e5272372b1e387324e76549487e5e85d6e7605d174ad24c            0.0s
 => => naming to docker.io/library/vidly_web_opt                                                        0.0s
```

The frontend image size is now reduced to 16.1MB! 🎉

### Final Images:

```bash
docker images
```

### 📊 Output:
```plaintext
REPOSITORY                        TAG          IMAGE ID       CREATED          SIZE
vidly_web_opt                     latest       d5069e1ff60d   47 seconds ago   16.1MB
07_reducing_the_images-frontend   latest       73c1174d7b7f   3 days ago       299MB
07_reducing_the_images-backend    latest       5da4526a308b   3 days ago       184MB
mongo                             4.0-xenial   fb1435e8841c   24 months ago    430MB
```

## 🛠️ Updating `docker-compose.prod.yml`

Now, update the `docker-compose.prod.yml` file to use the optimized frontend image:

```yaml
services:
  web:
    depends_on:
      - api
    build:
      context: ./frontend
      dockerfile: Dockerfile.prod
    ports:
      - 80:80
    restart: unless-stopped

  api:
    depends_on:
      - db
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/vidly
    command: ./docker-entrypoint.sh
    restart: unless-stopped

  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
    volumes:
      - vidly:/data/db
    restart: unless-stopped

volumes:
  vidly:
```

### Step 5: Rebuild the Images

Rebuild the images using the updated configuration:

```bash
docker-compose -f docker-compose.prod.yml build
```

### 🔄 Output:
```plaintext
[+] Building 6.0s (26/26) FINISHED                                                            docker:default 
 => [api internal] load build definition from Dockerfile                                                0.0s 
 => => transferring dockerfile: 217B                                                                    0.0s 
 => [web internal] load metadata for docker.io/library/node:14.16.0-alpine3.13                          2.8s 
 => [api auth] library/node:

pull token for registry-1.docker.io                                         0.0s 
 => [api internal] load .dockerignore                                                                   0.0s 
 => => transferring context: 53B                                                                        0.0s 
 => [web build-stage 1/6] FROM docker.io/library/node:14.16.0-alpine3.13@sha256:2c51dc462a02f15621e748  0.0s 
 => [api internal] load build context                                                                   0.0s 
 => => transferring context: 712B                                                                       0.0s 
 => CACHED [api 2/6] RUN addgroup app && adduser -S -G app app                                          0.0s 
 => CACHED [api 3/6] WORKDIR /app                                                                       0.0s 
 => CACHED [api 4/6] COPY package*.json ./                                                              0.0s 
 => CACHED [api 5/6] RUN npm install                                                                    0.0s 
 => CACHED [api 6/6] COPY . .                                                                           0.0s 
 => [api] exporting to image                                                                            0.1s 
 => => exporting layers                                                                                 0.0s 
 => => writing image sha256:f4b5bb39392349c132efb2a055cc2036ad1643517cfc1ac7c85b6478accdedcc            0.0s 
 => => naming to docker.io/library/07_reducing_the_images-api                                           0.0s 
 => [web internal] load build definition from Dockerfile.prod                                           0.1s
 => => transferring dockerfile: 352B                                                                    0.0s
 => [web internal] load metadata for docker.io/library/nginx:1.12-alpine                                2.4s
 => [web auth] library/nginx:pull token for registry-1.docker.io                                        0.0s
 => [web internal] load .dockerignore                                                                   0.0s
 => => transferring context: 53B                                                                        0.0s
 => [web stage-1 1/3] FROM docker.io/library/nginx:1.12-alpine@sha256:db5acc22920799fe387a903437eb8938  0.0s
 => [web internal] load build context                                                                   0.0s
 => => transferring context: 1.40kB                                                                     0.0s 
 => CACHED [web stage-1 2/3] RUN addgroup app && adduser -S -G app app                                  0.0s 
 => CACHED [web build-stage 2/6] WORKDIR /app                                                           0.0s 
 => CACHED [web build-stage 3/6] COPY package*.json ./                                                  0.0s 
 => CACHED [web build-stage 4/6] RUN npm install                                                        0.0s 
 => CACHED [web build-stage 5/6] COPY . .                                                               0.0s 
 => CACHED [web build-stage 6/6] RUN npm run build                                                      0.0s 
 => CACHED [web stage-1 3/3] COPY --from=build-stage /app/build /usr/share/nginx/html                   0.0s 
 => [web] exporting to image                                                                            0.1s 
 => => exporting layers                                                                                 0.0s 
 => => writing image sha256:d8e92ffcf6ec8c8fb8dcebd8dffc63f185079065d4d583172370ca38b141e7ae            0.0s 
 => => naming to docker.io/library/07_reducing_the_images-web                                           0.0s
```

### Final Docker Images:
```bash
docker images
```

### 📊 Output:
```plaintext
REPOSITORY                        TAG          IMAGE ID       CREATED          SIZE
vidly_web_opt                     latest       d5069e1ff60d   10 minutes ago   16.1MB
07_reducing_the_images-web        latest       d8e92ffcf6ec   10 minutes ago   16.1MB
```

## 🎯 Conclusion

By following these steps, you've successfully reduced the size of your Docker images, making your application more efficient and faster to deploy. Optimizing images is a crucial aspect of maintaining a lean, high-performance application in production. 🚀
