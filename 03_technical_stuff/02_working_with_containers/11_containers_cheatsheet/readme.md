# Docker Containers Commands Cheat Sheet 🐳✨

## Containers 🚀
- **Run a Container**: 
  ```bash
  docker run <image>
  ```
  🏃
- **Run in the Background**: 
  ```bash
  docker run -d <image>
  ```
  🕶️
- **Give a Custom Name**: 
  ```bash
  docker run --name <name> <image>
  ```
  📝
- **Publish a Port (HOST:CONTAINER)**: 
  ```bash
  docker run -p 3000:3000 <image>
  ```
  🔌

## Viewing the Logs 📜
- **View Logs**: 
  ```bash
  docker logs <containerID>
  ```
  🔍
- **Follow Logs in Real-Time**: 
  ```bash
  docker logs -f <containerID>
  ```
  ⏩
- **Add Timestamps to Logs**: 
  ```bash
  docker logs -t <containerID>
  ```
  🕒
- **View Last 10 Lines**: 
  ```bash
  docker logs -n 10 <containerID>
  ```
  🔟

## Executing Commands in Running Containers 🛠️
- **Execute a Command**: 
  ```bash
  docker exec <containerID> <cmd>
  ```
  🎛️
- **Start a Shell**: 
  ```bash
  docker exec -it <containerID> sh
  ```
  🐚

## Listing Containers 📋
- **List Running Containers**: 
  ```bash
  docker ps
  ```
  📂
- **List All Containers (Running & Stopped)**: 
  ```bash
  docker ps -a
  ```
  📜

## Starting and Stopping Containers 🟢🛑
- **Stop a Container**: 
  ```bash
  docker stop <containerID>
  ```
  🛑
- **Start a Container**: 
  ```bash
  docker start <containerID>
  ```
  🟢

## Removing Containers 🗑️
- **Remove a Container**: 
  ```bash
  docker container rm <containerID>
  ```
  🧹
- **Force Remove a Container**: 
  ```bash
  docker rm -f <containerID>
  ```
  ⚠️
- **Remove All Stopped Containers**: 
  ```bash
  docker container prune
  ```
  🧽

## Volumes 💾
- **List Volumes**: 
  ```bash
  docker volume ls
  ```
  📋
- **Create a Volume**: 
  ```bash
  docker volume create app-data
  ```
  🆕
- **Inspect a Volume**: 
  ```bash
  docker volume inspect app-data
  ```
  🔍
- **Mount a Volume to a Container**: 
  ```bash
  docker run -v app-data:/app/data <image>
  ```
  📦

## Copying Files Between Host and Containers 📁
- **Copy from Container to Host**: 
  ```bash
  docker cp <containerID>:/app/log.txt .
  ```
  ⬇️
- **Copy from Host to Container**: 
  ```bash
  docker cp secret.txt <containerID>:/app
  ```
  ⬆️

## Sharing Source Code with Containers 💻📦
- **Mount Host Directory to Container**: 
  ```bash
  docker run -v $(pwd):/app <image>
  ```
  🔗
