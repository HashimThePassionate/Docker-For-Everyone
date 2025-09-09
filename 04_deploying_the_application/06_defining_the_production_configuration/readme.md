# Defining the Production Configuration 🛠️

When deploying an application to a production environment, it's crucial to define a configuration that ensures stability, scalability, and reliability. Docker Compose makes this easier by allowing you to define a production-specific configuration in a separate file, such as `docker-compose.prod.yml`. This file can be used to tailor your services for a production environment.

Below is a breakdown of a typical production configuration using `docker-compose.prod.yml`.

## `docker-compose.prod.yml` Overview 📝

```yaml
services:
  web:
    depends_on:
      - api
    build: ./frontend
    ports:
      - 80:3000
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

### Services Configuration 🌐

#### 1. **Web Service (Frontend) 🌟**
   - **`depends_on`**: Specifies that the web service depends on the API service. Docker Compose will ensure that the API service starts before the web service.
   - **`build`**: Points to the `./frontend` directory where the `Dockerfile` for the frontend is located.
   - **`ports`**: Maps port `80` on the host to port `3000` in the container, making the frontend accessible via standard HTTP on port `80`.
   - **`restart`**: Configured as `unless-stopped`, meaning the container will always restart unless it is explicitly stopped.

#### 2. **API Service (Backend) 🛠️**
   - **`depends_on`**: Specifies that the API service depends on the database (DB) service. Docker Compose will ensure that the DB service starts before the API service.
   - **`build`**: Points to the `./backend` directory where the `Dockerfile` for the backend is located.
   - **`ports`**: Maps port `3001` on the host to port `3001` in the container, keeping the API accessible on the same port.
   - **`environment`**: Sets environment variables for the service. Here, `DB_URL` is used to specify the database connection string.
   - **`command`**: Overrides the default command to execute `./docker-entrypoint.sh`, which handles tasks like database migration before starting the server.
   - **`restart`**: Also configured as `unless-stopped`, ensuring the service restarts automatically if it fails.

#### 3. **DB Service (Database) 🗄️**
   - **`image`**: Uses the `mongo:4.0-xenial` image from Docker Hub, which is a specific version of MongoDB.
   - **`ports`**: Maps port `27017` on the host to port `27017` in the container, making the database accessible on its default port.
   - **`volumes`**: Mounts a volume named `vidly` to `/data/db` inside the container. This ensures that the database data is stored persistently, even if the container is removed.
   - **`restart`**: Configured as `unless-stopped`, keeping the database running unless manually stopped.

### Volumes 📦

#### **Persistent Storage**
   - **`vidly`**: A named volume that stores the MongoDB data. Volumes are essential in production to ensure that data is not lost when containers are stopped or removed.

### Key Points to Consider in Production 🚀

1. **Port Binding**: 
   - For the web service, port `80` is used instead of `3000`. Port `80` is the standard port for HTTP, making it more suitable for production as users typically expect to access web applications without specifying a port number.

2. **Restart Policies**:
   - The `restart: unless-stopped` policy ensures that if a container fails, it will automatically restart, which is vital for maintaining service availability in production.

3. **Environment Variables**:
   - Environment variables, such as `DB_URL`, are used to configure services dynamically. In production, these variables can be managed through external services or secrets management tools to enhance security.

4. **Volumes for Data Persistence**:
   - Using volumes like `vidly` ensures that your database data persists even if the container is destroyed. This is crucial for maintaining the integrity and availability of data in production environments.

### Conclusion 🎯

By defining a production-specific configuration in `docker-compose.prod.yml`, you can ensure that your application is optimized for reliability and stability. This setup includes proper port bindings, restart policies, and persistent storage, which are all critical components of a robust production environment.

Deploying your services using this configuration will help maintain uptime, handle failures gracefully, and ensure that your application runs smoothly in a production setting. If you have any further questions or need more details, feel free to ask! 😊