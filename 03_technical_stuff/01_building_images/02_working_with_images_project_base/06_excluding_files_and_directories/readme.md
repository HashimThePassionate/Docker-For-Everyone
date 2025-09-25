# Excluding Files and Directories in Docker 🐳

## Why Do We Exclude Files and Directories? 🤔

When building Docker images, it's important to be mindful of what files and directories are included in the build context. Including unnecessary files can lead to several issues:

1. **Increased Image Size** 📏: Including large directories or files (like `node_modules/`) can significantly increase the size of your Docker image, making it slower to build, push, and pull.

2. **Longer Build Times** 🕒: The more files you include in the build context, the longer it takes for Docker to process and build the image.

3. **Security Concerns** 🔒: Including sensitive files (e.g., environment variables, private keys) in the image could expose them unintentionally when the image is shared or deployed.

4. **Unnecessary Dependencies** ⚙️: Some directories and files are not needed inside the Docker container. For example, the `build` directory might contain build artifacts that aren't necessary for running the application in production.

## Why Exclude `node_modules/` Directory? 🚫

The `node_modules/` directory contains all the installed dependencies for a Node.js project. Here's why it's often excluded:

1. **Size** 📂: The `node_modules/` directory can be extremely large. Including it in the Docker image would bloat the image size, leading to inefficiency.

2. **Redundancy** 🌀: When you specify dependencies in `package.json`, Docker can install these dependencies inside the container using `npm install` or `yarn install`. This ensures that your container has a clean, consistent environment without carrying over potential issues from the host's `node_modules/`.

3. **Cross-Platform Issues** 🌍: Dependencies installed on a different OS or environment may not work correctly inside the container, leading to compatibility issues. It’s better to let Docker handle the installation within the container.

## Example: Creating a `.dockerignore` File 📄

To exclude files and directories from being included in the Docker image, you can create a `.dockerignore` file. This file works similarly to `.gitignore`, specifying patterns for files and directories that should be excluded from the build context.

### Step 1: Create a `.dockerignore` File

```plaintext
# .dockerignore

node_modules/
build
```

### Step 2: Dockerfile

Here’s the corresponding Dockerfile:

```Dockerfile
# Use a lightweight Node.js base image
FROM node:14.17.0-alpine3.13

# Set the working directory inside the container
WORKDIR /app

# Copy all files and directories from the current directory on the host to /app in the container, excluding those in .dockerignore
COPY . .
```

### Step 3: Build the Docker Image

```bash
# docker build -t react-app .
```

#### Build Output

```bash
[+] Building 2.9s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                                                                               0.1s
 => => transferring dockerfile: 89B                                                                                                0.0s
 => [internal] load metadata for docker.io/library/node:14.17.0-alpine3.13                                                         2.3s
 => [internal] load .dockerignore                                                                                                  0.1s
 => => transferring context: 53B                                                                                                   0.0s
 => [1/3] FROM docker.io/library/node:14.17.0-alpine3.13@sha256:782e891986f16cc661bfe928d0d163d4d0e6cf5cc05453dff2093c015fcc4a64   0.0s
 => [internal] load build context                                                                                                  0.1s
 => => transferring context: 535.92kB                                                                                              0.1s
 => CACHED [2/3] WORKDIR /app                                                                                                      0.0s
 => [3/3] COPY . .                                                                                                                 0.1s
 => exporting to image                                                                                                             0.1s
 => => exporting layers                                                                                                            0.0s
 => => writing image sha256:f8f526efe8239300fde5ac66c143380f8aca1ea18e71d25601724489530a9d55                                       0.0s
 => => naming to docker.io/library/react-app                                                                                       0.0s

View build details: docker-desktop://dashboard/build/default/default/ae74uqrtsgw58c7xeledcqdd9
```

### Step 4: Run the Docker Container

```bash
# docker run -it react-app sh
```

### Step 5: Verify Files Inside the Container

Inside the container, you’ll see that the `node_modules/` directory was not copied:

```bash
/app # ls
Dockerfile    package.json  public        readme.md     src           yarn.lock
```

## Conclusion 🎯

Excluding files and directories using a `.dockerignore` file is a best practice in Docker:

- **Reduce Image Size** 📏: Avoid bloating your Docker images with unnecessary files.
- **Improve Build Times** 🕒: Speed up your builds by limiting the files Docker needs to process.
- **Maintain Clean Environments** 🌱: Ensure that your container environments are consistent and free from potential issues related to host-specific files.