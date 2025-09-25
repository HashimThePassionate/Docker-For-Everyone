# Dockerfile Instructions 📜

A **Dockerfile** is a text document that contains all the commands a user could call on the command line to assemble an image. It’s essentially a set of instructions for Docker to build an image. By using a Dockerfile, you can automate the process of creating a Docker image, ensuring that your environment is consistent and repeatable.

### What is a Dockerfile? 📝

A **Dockerfile** is a script containing a series of instructions that Docker reads to build an image. Each instruction in a Dockerfile creates a new layer in the image, making it efficient and modular. Dockerfiles are the blueprint for creating Docker images.

### List of Dockerfile Instructions with Details 🛠️

Here’s a detailed list of common Dockerfile instructions, each with an explanation and beautiful emojis for clarity:

1. **`FROM`** 🏗️
   - **Purpose**: Specifies the base image to use for the subsequent instructions. This is the first instruction in every Dockerfile.
   - **Example**: `FROM ubuntu:20.04`

2. **`LABEL`** 🏷️
   - **Purpose**: Adds metadata to an image, such as a version or description.
   - **Example**: `LABEL maintainer="hashim@example.com"`

3. **`RUN`** 🚀
   - **Purpose**: Executes any commands in a new layer on top of the current image and commits the results. Often used to install packages.
   - **Example**: `RUN apt-get update && apt-get install -y nginx`

4. **`CMD`** 📜
   - **Purpose**: Provides the default command that will be executed when a container is run. You can only have one `CMD` instruction in a Dockerfile.
   - **Example**: `CMD ["nginx", "-g", "daemon off;"]`

5. **`ENTRYPOINT`** 🚪
   - **Purpose**: Configures a container that will run as an executable. It works similarly to `CMD` but is harder to override.
   - **Example**: `ENTRYPOINT ["nginx", "-g", "daemon off;"]`

6. **`WORKDIR`** 🏢
   - **Purpose**: Sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions that follow it in the Dockerfile.
   - **Example**: `WORKDIR /usr/src/app`

7. **`COPY`** 📦
   - **Purpose**: Copies files or directories from the host filesystem into the container filesystem.
   - **Example**: `COPY . /usr/src/app`

8. **`ADD`** ➕
   - **Purpose**: Similar to `COPY`, but with additional features like the ability to fetch remote URLs and unpack compressed files.
   - **Example**: `ADD http://example.com/file.tar.gz /tmp/`

9. **`ENV`** 🌍
   - **Purpose**: Sets environment variables in the container.
   - **Example**: `ENV NODE_ENV=production`

10. **`ARG`** 🔧
    - **Purpose**: Defines a variable that users can pass at build-time to the builder with the `docker build` command using the `--build-arg` flag.
    - **Example**: `ARG VERSION=latest`

11. **`EXPOSE`** 🔌
    - **Purpose**: Informs Docker that the container will listen on the specified network ports at runtime. This is purely informational and does not actually publish the ports.
    - **Example**: `EXPOSE 80`

12. **`VOLUME`** 📁
    - **Purpose**: Creates a mount point with the specified path and marks it as holding externally mounted volumes from the host or other containers.
    - **Example**: `VOLUME /var/www/html`

13. **`USER`** 👤
    - **Purpose**: Specifies the user that the subsequent commands should run as.
    - **Example**: `USER www-data`

14. **`HEALTHCHECK`** ❤️‍🩹
    - **Purpose**: Tells Docker how to test the container to check that it’s still working.
    - **Example**: 
      ```Dockerfile
      HEALTHCHECK CMD curl --fail http://localhost/ || exit 1
      ```

15. **`ONBUILD`** 🚧
    - **Purpose**: Adds a trigger instruction to the image that will be executed when the image is used as a base for another build.
    - **Example**: `ONBUILD RUN echo "This runs when used as a base image"`

16. **`STOPSIGNAL`** 🛑
    - **Purpose**: Sets the system call signal that will be sent to the container to exit.
    - **Example**: `STOPSIGNAL SIGTERM`

17. **`SHELL`** 💻
    - **Purpose**: Specifies the default shell to use for the `RUN` command.
    - **Example**: `SHELL ["/bin/bash", "-c"]`

### Example Dockerfile 📝

Here’s an example Dockerfile that uses some of these instructions:

```Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the current directory contents into the container at /usr/src/app
COPY . /usr/src/app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

## Summary 🎯

- **Dockerfile** 📝: A script that contains a list of instructions to build a Docker image.
- **Instructions** 🛠️: Each instruction adds a layer to the image, making it modular and efficient.
- **Common Instructions**:
  - `FROM` 🏗️: Specifies the base image.
  - `RUN` 🚀: Executes commands in a new layer.
  - `CMD` 📜 & `ENTRYPOINT` 🚪: Defines the default command or entry point.
  - `COPY` 📦 & `ADD` ➕: Copies files into the container.
  - `EXPOSE` 🔌: Specifies the ports the container will listen on.
