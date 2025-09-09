# Images and Containers in Docker 🐳

## Overview

In Docker, **images** and **containers** are two fundamental concepts that enable the deployment and management of applications in a consistent and isolated environment. While they are closely related, they serve different purposes in the Docker ecosystem.

### What is a Docker Image? 🖼️

A Docker **image** is a lightweight, standalone, and executable package that includes everything needed to run a piece of software. This includes the code, runtime, libraries, environment variables, and configuration files. Images are read-only and serve as a blueprint for creating containers.

- **Think of an image as a recipe** 📝: It contains all the instructions to create and run a container, but it’s not yet running or interactive on its own.

### What is a Docker Container? 📦

A Docker **container** is an instance of an image that is running. Containers are isolated environments where the application runs with all the dependencies defined in the image. They share the host system's kernel but have their own filesystem, processes, and network stack.

- **Think of a container as the dish** 🍽️: It’s created from the recipe (image) and is now running, ready for use.

### Key Differences Between Images and Containers 🧐

| Aspect | Docker Image 🖼️ | Docker Container 📦 |
|--------|-----------------|--------------------|
| **State** | Static and immutable | Dynamic and mutable |
| **Function** | Blueprint or template | Running instance of an image |
| **Creation** | Created using a Dockerfile or pulled from a repository | Created from an image using the `docker run` command |
| **Persistence** | Persistent across environments | Ephemeral; changes to a container are lost when it is removed unless persisted |
| **Usage** | Used to create containers | Used to run applications |

### Example Scenario

Let's go through an example where we have a Docker image and create a container from it.

#### Step 1: Viewing the Filesystem from an Image

We are currently using a Docker image and can see the contents of the `/home` directory:

```bash
root@ea8aaf59a45c:/home# ls -l
total 12
-rw-r--r-- 1 root   root     11 Aug  7 20:05 deploy.sh
drwxr-x--- 2 hashim hashim 4096 Aug  7 19:29 hashim   
drwxr-x--- 2 ubuntu ubuntu 4096 Apr 29 14:05 ubuntu   
```

Here, we can see the `deploy.sh` script along with directories for `hashim` and `ubuntu`.

#### Step 2: Running a Container

Now, let's run a container using the same image:

```bash
# docker run -it ubuntu
root@701c585f0644:/# cd /home/
root@701c585f0644:/home# ls
ubuntu
```

Inside the container, the `/home` directory only contains the `ubuntu` directory, and the `deploy.sh` script is not present.

### Explanation

- The **image** provides the base environment, including the default directories and files.
- When a **container** is created from this image, it starts as a fresh instance. The file system inside the container is isolated from the host's file system unless explicitly shared, which is why the `deploy.sh` script isn't visible inside the container.

## Summary 🎯

- **Docker Image** 🖼️: A static, read-only template that contains the application and its dependencies. It’s the blueprint for creating containers.
- **Docker Container** 📦: A live, running instance of an image that is isolated and can be interacted with. It represents the execution of the image.

