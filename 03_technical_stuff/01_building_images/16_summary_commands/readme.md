
# 🐳 **Docker Commands Cheat Sheet**

## 📝 **Dockerfile Instructions**
- **`FROM`**: 📦 Specify the base image
  ```bash
  FROM <base_image>
  ```
- **`WORKDIR`**: 🛠️ Set the working directory
  ```bash
  WORKDIR /app
  ```
- **`COPY`**: 📂 Copy files/directories
  ```bash
  COPY . /app
  ```
- **`ADD`**: ➕ Copy files/directories (can also extract tar files)
  ```bash
  ADD source.tar.gz /app
  ```
- **`RUN`**: 🏃 Run commands
  ```bash
  RUN apt-get update && apt-get install -y <package>
  ```
- **`ENV`**: 🌍 Set environment variables
  ```bash
  ENV NODE_ENV=production
  ```
- **`EXPOSE`**: 🚪 Document the port the container is listening on
  ```bash
  EXPOSE 3000
  ```
- **`USER`**: 👤 Set the user running the app
  ```bash
  USER appuser
  ```
- **`CMD`**: 🛠️ Set the default command/program
  ```bash
  CMD ["npm", "start"]
  ```
- **`ENTRYPOINT`**: 🚀 Set the default command/program
  ```bash
  ENTRYPOINT ["npm", "start"]
  ```

## 🖼️ **Image Commands**
- **Build an image**: 🔨
  ```bash
  docker build -t <name> .
  ```
- **List images**: 📜
  ```bash
  docker images
  ```
- **Alias for listing images**: 📋
  ```bash
  docker image ls
  ```
- **Run a container interactively**: 🖥️
  ```bash
  docker run -it <image> sh
  ```

## 🚀 **Starting and Stopping Containers**
- **Stop a container**: 🛑
  ```bash
  docker stop <containerID>
  ```
- **Start a container**: ▶️
  ```bash
  docker start <containerID>
  ```

## 🗑️ **Removing Containers**
- **Remove a container**: ❌
  ```bash
  docker container rm <containerID>
  ```
- **Alias for removing a container**: 🧹
  ```bash
  docker rm <containerID>
  ```
- **Force remove a container**: 💥
  ```bash
  docker rm -f <containerID>
  ```
- **Remove all stopped containers**: 🧼
  ```bash
  docker container prune
  ```

## 💾 **Volumes**
- **List volumes**: 📂
  ```bash
  docker volume ls
  ```
- **Create a volume**: 🆕
  ```bash
  docker volume create app-data
  ```
- **Inspect a volume**: 🔍
  ```bash
  docker volume inspect app-data
  ```
- **Run a container with a volume**: 📥
  ```bash
  docker run -v app-data:/app/data <image>
  ```

## 📂 **Copying Files Between Host and Containers**
- **Copy from container to host**: ⬇️
  ```bash
  docker cp <containerID>:/app/log.txt .
  ```
- **Copy from host to container**: ⬆️
  ```bash
  docker cp secret.txt <containerID>:/app
  ```

## 💻 **Sharing Source Code with Container**
- **Mount the current directory into a container**: 🔄
  ```bash
  docker run -v $(pwd):/app <image>
  ```