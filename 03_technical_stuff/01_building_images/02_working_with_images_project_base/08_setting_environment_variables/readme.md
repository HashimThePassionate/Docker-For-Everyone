# Setting Environment Variables in Docker 🐳

## What Are Environment Variables? 🌍

**Environment variables** are key-value pairs that can influence the behavior of running processes. They are used to store configuration settings that can change between different environments (like development, staging, and production). Instead of hardcoding configuration settings into your code, you can define them as environment variables, making your application more flexible and easier to manage.

### Why Do We Need Environment Variables? 🤔

1. **Configuration Management** 🛠️: Environment variables allow you to configure your application without changing the code. For example, you can specify different database URLs, API endpoints, or feature flags depending on the environment your application is running in.

2. **Security** 🔒: Sensitive information like API keys, passwords, and tokens can be stored as environment variables rather than hardcoded into the source code. This keeps your secrets secure and out of version control.

3. **Portability** 🌍: Environment variables make your Docker containers more portable by allowing you to define environment-specific settings at runtime rather than during the build process.

4. **Flexibility** 🌀: They provide an easy way to change settings or features without modifying the application code. You can deploy the same Docker image to different environments with different configurations.

## Dockerfile Example with Environment Variables 📝

Here’s how you can define environment variables in a Dockerfile:

```Dockerfile
# Use a lightweight Node.js base image
FROM node:14.17.0-alpine3.13

# Set the working directory inside the container
WORKDIR /app

# Copy all files and directories from the current directory on the host to /app in the container
COPY . .

# Install Node.js dependencies
RUN npm install

# Set an environment variable
ENV API_URL=http://localhost:3000/myapi
```

### Explanation of the Dockerfile

1. **FROM**: Specifies the base image `node:14.17.0-alpine3.13`.
2. **WORKDIR**: Sets the working directory inside the container to `/app`.
3. **COPY**: Copies all files and directories from the host to the container’s `/app` directory.
4. **RUN**: Executes `npm install` to install Node.js dependencies.
5. **ENV**: Defines an environment variable `API_URL` with the value `http://localhost:3000/myapi`. This variable will be accessible to the application running inside the container.

## Building and Running the Docker Image 🛠️

### Step 1: Build the Docker Image

```bash
# docker build -t react-app .
```

### Step 2: Build Process Overview

Here’s a snapshot of what the build process might look like:

```bash
[+] Building 372.5s (9/9) FINISHED                                           docker:default
 => [internal] load build definition from Dockerfile                                   0.1s
 => => transferring dockerfile: 150B                                                   0.0s
 => [internal] load metadata for docker.io/library/node:14.17.0-alpine3.13             4.1s
 => [internal] load .dockerignore                                                      0.1s
 => => transferring context: 53B                                                       0.0s
 => [1/4] FROM docker.io/library/node:14.17.0-alpine3.13@sha256:782e891986f16cc661bfe  0.0s 
 => [internal] load build context                                                      0.3s 
 => => transferring context: 547.61kB                                                  0.3s 
 => CACHED [2/4] WORKDIR /app                                                          0.0s 
 => [3/4] COPY . .                                                                     0.1s 
 => [4/4] RUN npm install                                                            354.5s 
 => exporting to image                                                                12.3s
 => => exporting layers                                                               12.1s
 => => writing image sha256:fdafe3f03a747464e63f08386e4af8735a7fbea37d29c6de1c2f24c4c  0.0s
 => => naming to docker.io/library/react-app     
```

### Step 3: Run the Docker Container

After building the image, run a container:

```bash
# docker run -it react-app sh
```

### Step 4: Check Environment Variables Inside the Container

Inside the container, you can use the `printenv` command to see all environment variables:

```bash
/app # printenv
NODE_VERSION=14.17.0
HOSTNAME=a528dc924700
YARN_VERSION=1.22.5
SHLVL=2
HOME=/root
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/app
API_URL=http://localhost:3000/myapi
```

You can also check a specific environment variable:

```bash
/app # printenv API_URL
http://localhost:3000/myapi
```

Or use `echo` to display its value:

```bash
/app # echo $API_URL
http://localhost:3000/myapi
```

## Purpose of Environment Variables 🎯

- **Control Configuration** 🛠️: Easily change the configuration of your application without altering the code.
- **Environment-Specific Settings** 🌍: Use different values for different environments (e.g., development, staging, production).
- **Security** 🔒: Keep sensitive information out of your source code by storing it in environment variables.
- **Flexibility and Portability** 🚀: Deploy the same Docker image in different environments with varying configurations, improving the portability of your application.
