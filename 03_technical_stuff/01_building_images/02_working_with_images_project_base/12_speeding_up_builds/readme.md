# Speeding Up Docker Builds 🚀

## How Docker Creates Layers

When Docker builds an image, each instruction in the Dockerfile creates a new layer in the image. These layers are stacked on top of each other to form the final image. The layers are created from the base image (the `FROM` instruction) and each subsequent instruction (e.g., `RUN`, `COPY`, `ENV`, `CMD`) adds a new layer.

### Reading Layers with `docker history`

When you run `docker history react-app`, you can see the layers created during the build process. The layers are listed from top to bottom, with the most recent layer at the top.

```bash
docker history react-app
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
343534f3dd16   19 minutes ago   CMD ["/bin/sh" "-c" "npm start"]                0B        buildkit.dockerfile.v0
<missing>      19 minutes ago   EXPOSE map[3000/tcp:{}]                         0B        buildkit.dockerfile.v0
<missing>      19 minutes ago   ENV API_URL=http://localhost:3000/myapi         0B        buildkit.dockerfile.v0
<missing>      19 minutes ago   RUN /bin/sh -c npm install # buildkit           370MB     buildkit.dockerfile.v0
<missing>      22 minutes ago   COPY . . # buildkit                             739kB     buildkit.dockerfile.v0
<missing>      43 minutes ago   WORKDIR /app                                    0B        buildkit.dockerfile.v0
<missing>      43 minutes ago   USER app                                        0B        buildkit.dockerfile.v0
<missing>      43 minutes ago   RUN /bin/sh -c addgroup app &&  adduser -S -…   4.84kB    buildkit.dockerfile.v0
<missing>      3 weeks ago      /bin/sh -c #(nop)  CMD ["node"]                 0B
<missing>      3 weeks ago      /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B
<missing>      3 weeks ago      /bin/sh -c #(nop) COPY file:238737301d473041…   116B       
<missing>      3 weeks ago      /bin/sh -c apk add --no-cache --virtual .bui…   7.84MB     
<missing>      3 weeks ago      /bin/sh -c #(nop)  ENV YARN_VERSION=1.22.5      0B
<missing>      3 weeks ago      /bin/sh -c addgroup -g 1000 node     && addu…   104MB      
<missing>      3 weeks ago      /bin/sh -c #(nop)  ENV NODE_VERSION=14.17.0     0B
<missing>      3 weeks ago      /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
<missing>      3 weeks ago      /bin/sh -c #(nop) ADD file:8ec69d882e7f29f06…   5.61MB   
```

### Layer Explanation

1. **Base Image**: 
   - **Layer**: `/bin/sh -c #(nop) ADD file:8ec69d882e7f29f06…`
   - **Size**: 5.61MB
   - **Description**: This layer adds the base image to the container. It’s the foundation upon which all other layers are built.

2. **ENV and RUN Instructions**:
   - **Layer**: `/bin/sh -c #(nop)  ENV NODE_VERSION=14.17.0`
   - **Size**: 0B
   - **Description**: Setting environment variables like `NODE_VERSION`.

3. **Adding Packages**:
   - **Layer**: `/bin/sh -c apk add --no-cache --virtual .build-deps…`
   - **Size**: 7.84MB
   - **Description**: This layer installs necessary build dependencies.

4. **Adding Files**:
   - **Layer**: `COPY . .`
   - **Size**: 739kB
   - **Description**: This layer copies all files from the host into the container.

5. **Installing Dependencies**:
   - **Layer**: `RUN npm install`
   - **Size**: 370MB
   - **Description**: This layer runs `npm install`, installing all dependencies required by the application.

6. **Setting Environment Variables**:
   - **Layer**: `ENV API_URL=http://localhost:3000/myapi`
   - **Size**: 0B
   - **Description**: This sets an environment variable inside the container.

7. **Exposing Ports**:
   - **Layer**: `EXPOSE 3000`
   - **Size**: 0B
   - **Description**: This layer indicates that the container will listen on port 3000.

8. **Setting the CMD**:
   - **Layer**: `CMD ["/bin/sh" "-c" "npm start"]`
   - **Size**: 0B
   - **Description**: This layer defines the command that should be run when the container starts.

### Layered Approach in Docker 🛠️

- **Efficiency**: Docker’s layered file system makes builds more efficient. Each layer can be cached, meaning that if nothing changes in the layer’s instructions, Docker can reuse the existing layer instead of rebuilding it.
- **Layer Reuse**: If any layer in the Dockerfile changes, Docker will rebuild that layer and any subsequent layers, but will reuse the cached layers above.

## Optimizing Docker Builds 🚀

By optimizing the Dockerfile, you can speed up builds by reducing the number of layers that need to be rebuilt. This is especially useful for large projects with many dependencies.

### Updated Dockerfile for Optimization

```Dockerfile
FROM node:14.17.0-alpine3.13

# Create a group and user, and set permissions
RUN addgroup app && adduser -S -G app app

# Switch to the non-root user
USER app

# Set the working directory inside the container
WORKDIR /app

# Copy only package.json and package-lock.json first
COPY package*.json .

# Install Node.js dependencies
RUN npm install

# Copy the rest of the application files
COPY . .

# Set an environment variable
ENV API_URL=http://localhost:3000/myapi

# Expose port 3000
EXPOSE 3000

# Specify the command to run the React app
CMD npm start
```

### Rebuilding the Image

1. **Initial Build**:
   ```bash
   # docker build -t react-app .
   [+] Building 191.3s (11/11) FINISHED                                                        docker:default 
   => [internal] load build definition from Dockerfile                                                  0.1s 
   => => transferring dockerfile: 254B                                                                  0.0s 
   => [internal] load metadata for docker.io/library/node:14.17.0-alpine3.13                            3.0s 
   => [internal] load .dockerignore                                                                     0.1s 
   => => transferring context: 53B                                                                      0.0s 
   => [1/6] FROM docker.io/library/node:14.17.0-alpine3.13@sha256:782e891986f16  0.0s 
   => [internal] load build context                                                                     0.2s 
   => => transferring context: 742.07kB                                                                 0.2s 
   => CACHED [2/6] RUN addgroup app &&  adduser -S -G app app                                           0.0s 
   => CACHED [3/6] WORKDIR /app                                                                         0.0s 
   => [4/6] COPY package*.json .                                                                        0.1s 
   => [5/6] RUN npm install                                                                           174.6s 
   => [6/6] COPY . .                                                                                    0.2s
   => exporting to image                                                                               12.6s
   => => exporting layers                                                                              12.5s
   => => writing image sha256:defbc1fc22d8f38b661087f394cc7056ba0b0314233e174fcd6220d0395a4339          0.0s
   => => naming to docker.io/library/react-app   
   ```

2. **Subsequent Build**:
   ```bash
   # docker build -t react-app .
   [+] Building 7.0s (11/11) FINISHED                                                          docker:default
   => [internal] load build definition from Dockerfile                                                  0.3s
   => => transferring dockerfile: 254B                                                                  0.0s
   => [internal] load metadata for docker.io/library/node:14.17.0-alpine3.13                            2.1s
   => [internal] load .dockerignore                                                                     0.6s
   => => transferring context: 53B                                                                      0.0s
   => [1/6] FROM docker.io/library/node:14.17.0-alpine3.13@sha256:782e891986f16cc661bfe928d0d163d4d0e6  0.0s
  

 => [internal] load build context                                                                     0.4s
   => => transferring context: 708B                                                                     0.0s
   => CACHED [2/6] RUN addgroup app &&  adduser -S -G app app                                           0.0s
   => CACHED [3/6] WORKDIR /app                                                                         0.0s
   => CACHED [4/6] COPY package*.json .                                                                 0.0s
   => CACHED [5/6] RUN npm install                                                                      0.0s
   => CACHED [6/6] COPY . .                                                                             0.0s
   => exporting to image                                                                                0.6s
   => => exporting layers                                                                               0.0s
   => => writing image sha256:7bbe58d0a548089c7e311dadd28ccc0367365ff3b4012da0b1e3a0024c690c2f          0.3s
   => => naming to docker.io/library/react-app  
    ```


### How Docker Uses Cache to Optimize Builds 🧩

- **Layer Caching**: Docker caches each layer of the image. If a layer doesn’t change between builds, Docker can reuse the cached layer instead of rebuilding it. This significantly speeds up the build process, especially for large images.
- **Impact of Instruction Order**: Instructions that are less likely to change (like installing OS packages or creating user accounts) should be placed early in the Dockerfile, so they can be cached. Instructions that are more likely to change (like copying application files) should be placed later in the Dockerfile.

### Dockerfile Optimization Techniques 📈
| **Optimization Technique**                              | **Description**                       | **Position in Dockerfile**    |
|---------------------------------------------------------|--------------------------------------------------------------------------------------------------|-------------------------------|
| **Leverage Layer Caching**                              | Place commands that change less frequently earlier in the Dockerfile to maximize caching benefits.| Top                            |
| **Minimize the Number of Layers**                       | Combine multiple `RUN` commands into a single command to reduce the number of layers.             | Top/Mid                        |
| **Use .dockerignore**                                   | Exclude unnecessary files from the build context to speed up the `COPY` and `ADD` instructions.   | Top                            |
| **Copy Only Necessary Files First**                     | Copy package files (e.g., `package.json`) first, run `npm install`, and then copy the rest of the application files. | Mid                            |
| **Avoid Installing Unnecessary Packages**               | Only install the packages needed for your application to minimize the image size.                 | Mid                            |
| **Use Multistage Builds for Production**                | Use multistage builds to separate the build environment from the runtime environment.             | Top/Mid                        |
| **Order Instructions by Frequency of Change**           | Place commands that change less frequently (e.g., OS updates) earlier in the Dockerfile.          | Top                            |
| **Clean Up After Yourself**                             | Remove unnecessary files and cache after installing packages or building software.                | Mid                            |



### Dockerfile Instruction Placement 🚦

- **Top of Dockerfile**:
  - **FROM**: Defines the base image.
  - **RUN** (Static): Installs system packages or sets up the environment that doesn’t change often.
  - **COPY** (Specific files): Copy files that don’t change frequently, like `package.json` for npm.

- **Middle of Dockerfile**:
  - **RUN** (Dynamic): Installs application dependencies (e.g., `npm install`) after copying package files.
  - **COPY** (Remaining files): Copy the rest of the application files.

- **Bottom of Dockerfile**:
  - **ENV**: Set environment variables.
  - **EXPOSE**: Expose the necessary ports.
  - **CMD/ENTRYPOINT**: Define the default command to run when the container starts.

By organizing your Dockerfile with these optimization techniques, you can speed up your Docker builds significantly, especially when you need to rebuild images frequently during development.
