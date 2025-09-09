# Defining Entry Points in Docker 🐳

## Initial Problem: Permission Denied Error 😕

When you initially ran the container with `docker run react-app npm start`, you encountered a permission denied error:

```bash
Failed to compile.

[eslint] EACCES: permission denied, mkdir '/app/node_modules/.cache'
ERROR in [eslint] EACCES: permission denied, mkdir '/app/node_modules/.cache'
```

This error occurred because the `npm install` command was run as the root user, and then you switched to a non-root user (`app`) afterward. The non-root user didn’t have permission to write to directories created by the root user.

## Solution: Adjusting User Setup 🛠️

To fix this, you moved the user creation steps earlier in the Dockerfile, so that the non-root user (`app`) is used during the entire build process, ensuring all files and directories created are owned by this user.

### Original Dockerfile

```Dockerfile
FROM node:14.17.0-alpine3.13
WORKDIR /app
COPY . .
RUN npm install 
ENV API_URL=http://localhost:3000/myapi
EXPOSE 3000
RUN addgroup app &&  adduser -S -G app app
USER app
```

### Updated Dockerfile

Here's the updated Dockerfile:

```Dockerfile
# Use a lightweight Node.js base image
FROM node:14.17.0-alpine3.13

# Create a group and user, and set permissions
RUN addgroup app && adduser -S -G app app

# Switch to the non-root user
USER app

# Set the working directory inside the container
WORKDIR /app

# Copy all files and directories from the current directory on the host to /app in the container
COPY . .

# Install Node.js dependencies
RUN npm install

# Set an environment variable
ENV API_URL=http://localhost:3000/myapi

# Expose port 3000
EXPOSE 3000

# Specify the command to run the React app
CMD ["npm", "start"]
```

### Rebuild the Image

After updating the Dockerfile, you can rebuild the Docker image with the following command:

```bash
docker build -t react-app .
```

### Building the Image Output

Here’s the output you might see during the build process:

```bash
[+] Building 277.0s (10/10) FINISHED                                        docker:default
 => [internal] load build definition from Dockerfile                                  0.2s
 => => transferring dockerfile: 217B                                                  0.1s
 => [internal] load metadata for docker.io/library/node:14.17.0-alpine3.13            3.2s
 => [internal] load .dockerignore                                                     0.1s
 => => transferring context: 53B                                                      0.0s
 => CACHED [1/5] FROM docker.io/library/node:14.17.0-alpine3.13@sha256:782e891986f16  0.0s
 => [internal] load build context                                                     0.2s
 => => transferring context: 740.21kB                                                 0.1s
 => [2/5] RUN addgroup app &&  adduser -S -G app app                                  0.7s
 => [3/5] WORKDIR /app                                                                0.1s
 => [4/5] COPY . .                                                                    0.1s
 => [5/5] RUN npm install                                                           257.8s
 => exporting to image                                                               14.2s
 => => exporting layers                                                              14.0s 
 => => writing image sha256:9902c2db5dad427690ddffa558b99990299ee3d171d090b6285b2460  0.0s 
 => => naming to docker.io/library/react-app                                          0.0s 
```

### Running the Container

Now, when you run the container:

```bash
docker run react-app
```

You should see the app starting without any permission issues:

```bash
> my-app@0.1.0 start /app
> react-scripts start

Compiled successfully!

You can now view my-app in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://172.17.0.2:3000

Note that the development build is not optimized.
To create a production build, use npm run build.

webpack compiled successfully
```

### CMD vs. RUN: What's the Difference? 🛠️

- **RUN**:
  - **Purpose**: Used to execute commands during the image build process.
  - **Execution**: Commands are executed during build time to create layers in the image.
  - **Use Case**: Installing software, setting up the environment, and preparing the container’s filesystem.
  
  **Example**:
  ```Dockerfile
  RUN npm install
  ```
  This command installs the necessary Node.js dependencies during the image build.

- **CMD**:
  - **Purpose**: Provides the default command that should run when a container starts.
  - **Execution**: Commands are executed at runtime when the container is started.
  - **Use Case**: Running the main application or service inside the container.
  - **Override**: `CMD` can be overridden by specifying a different command at runtime using `docker run`.

  **Example**:
  ```Dockerfile
  CMD ["npm", "start"]
  ```
  This command starts the React app when the container runs.

### Forms of CMD 📝

1. **Shell Form**:
   ```Dockerfile
   CMD npm start
   ```
   - **Use Case**: Executes the command using the shell (`/bin/sh -c`), which allows the use of shell features like environment variable expansion.
   - **Performance Consideration**: This form involves a slight overhead because it spawns a shell process.

2. **Exec Form**:
   ```Dockerfile
   CMD ["npm", "start"]
   ```
   - **Use Case**: Preferred when you don’t need shell features. This form executes the command directly without invoking a shell.
   - **Performance Consideration**: More efficient as it directly runs the command without any shell overhead.

### What is ENTRYPOINT? 🎯

**ENTRYPOINT** is similar to `CMD`, but it defines a command that will always run when the container starts. Unlike `CMD`, which can be overridden by passing a different command at runtime, `ENTRYPOINT` is more rigid and is intended to be the main executable of the container.

### Forms of ENTRYPOINT 📝

1. **Shell Form**:
   ```Dockerfile
   ENTRYPOINT npm start
   ```
   - **Use Case**: Similar to `CMD`, it executes the command in a shell. Useful when you need to use shell features.
   - **Behavior**: Runs `npm start` every time the container starts, but can be overridden using `docker run` with `--entrypoint`.

2. **Exec Form**:
   ```Dockerfile
   ENTRYPOINT ["npm", "start"]
   ```
   - **Use Case**: Ensures that `npm start` is always run when the container starts, and is not easily overridden by `docker run`.
   - **Behavior**: More efficient as it directly runs the command without shell overhead.

### Difference Between CMD and ENTRYPOINT 🚦

- **CMD**: Provides default commands and arguments that can be overridden by `docker run`. If multiple `CMD` instructions are present, only the last one is executed.

- **ENTRYPOINT**: Specifies a command that will always run when the container starts. It is not easily overridden and is ideal for defining the main process of a container.

### Example: Prevent Overriding ENTRYPOINT 🚫

When you specify an `ENTRYPOINT`, it’s not as easily overridden. For example:

```Dockerfile
ENTRYPOINT ["npm", "start"]
```

Running the container with an interactive shell:

```bash
docker run -it react-app sh
```

This will still run `npm start` first, unless you use the `--entrypoint` flag to override it:

```bash
docker run -it --entrypoint sh react-app
```

### Use Cases for ENTRYPOINT 📚

- **Single-purpose containers**: ENTRYPOINT is ideal for containers designed to run a specific task, like a web server or batch processing job.
- **Ensuring critical commands are run**: If you want to ensure that a specific command always runs, ENTRYPOINT is the way to go.

### Final Dockerfile with CMD and ENTRYPOINT

```Dockerfile
# Use a lightweight Node.js base image
FROM node:14.17.0-alpine3.13

# Create a group and user, and set permissions
RUN addgroup app && adduser -S -G app app

# Switch to the non-root user
USER app

# Set the working directory inside the container
WORKDIR /app

# Copy all files and directories from the current directory on the host to /app in the container
COPY . .

# Install Node.js dependencies
RUN npm install

# Set an environment variable
ENV API_URL=http://localhost:3000/myapi

# Expose port 3000
EXPOSE 3000

# Specify the command to run the React app
ENTRYPOINT ["npm", "start"]
```

By defining `CMD` or `ENTRYPOINT`, you can control what your container does when it starts, making
