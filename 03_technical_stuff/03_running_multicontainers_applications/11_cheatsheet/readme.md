# 🐳 **Docker Compose Multi-Container Cheatsheet** 📝

### 1. **Build Your Docker Containers** 🛠️

- **Build Containers:**
  ```bash
  docker-compose build
  ```
  🔨 Builds all the services defined in your `docker-compose.yml`.

- **Build Without Cache:**
  ```bash
  docker-compose build --no-cache
  ```
  🚫 Builds the containers without using the cache, ensuring a fresh build.

### 2. **Start Your Containers** 🚀

- **Start Containers (Foreground):**
  ```bash
  docker-compose up
  ```
  ▶️ Starts all the containers and shows their logs in the terminal.

- **Start Containers (Detached Mode):**
  ```bash
  docker-compose up -d
  ```
  🖥️ Starts all the containers in the background, freeing up your terminal.

- **Build and Start Containers:**
  ```bash
  docker-compose up --build
  ```
  🔄 Builds the containers and then starts them, ensuring any changes to the Dockerfile are applied.

### 3. **Stop and Remove Containers** 🛑

- **Stop and Remove Containers:**
  ```bash
  docker-compose down
  ```
  ⬇️ Stops all running containers and removes them, along with the networks.

### 4. **Monitor Your Containers** 👀

- **List Running Containers:**
  ```bash
  docker-compose ps
  ```
  📋 Shows the status of the containers managed by Docker Compose.

- **View Logs:**
  ```bash
  docker-compose logs
  ```
  📜 Displays the logs of the running containers, helping you debug any issues.

---

