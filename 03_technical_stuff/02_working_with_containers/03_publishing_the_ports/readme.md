# 🌐 Publishing the Port

## Understanding Port Publishing in Docker

When running applications inside a Docker container, the application typically listens on a specific port inside the container. However, by default, this port is not accessible from outside the container. To make the application accessible from the host machine or other devices on the network, you need to "publish" the container's port to a port on the host.

### Running a Container and Publishing Ports

Let's look at an example where we publish a port from a container to the host machine.

1. **Current Running Containers**:

   ```bash
   docker ps
   ```

   **Output**:

   ```bash
   CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS      NAMES       
   ac2a310ccd59   react-app   "docker-entrypoint.s…"   12 minutes ago   Up 12 minutes   3000/tcp   blue_whale  
   cbdcd325ae26   react-app   "docker-entrypoint.s…"   13 minutes ago   Up 13 minutes   3000/tcp   great_diffie
   ```

2. **Publishing a Port**:

   To run a new container and publish a port, you can use the `-p` flag with the `docker run` command. Here’s an example where we map the container’s port `3000` to port `8000` on the host machine:

   ```bash
   docker run -d -p 8000:3000 --name con1 react-app
   ```

   **Output**:

   ```bash
   7d03a29d8a96de8a4d1085e1ab5676ba9a39cae7918ad0348bb857891fa025de
   ```

   This command creates a new container named `con1`, and it maps port `3000` inside the container to port `8000` on the host.

3. **Checking the Published Ports**:

   You can verify the port mapping by running `docker ps`:

   ```bash
   docker ps
   ```

   **Output**:

   ```bash
   CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                  NAMES
   7d03a29d8a96   react-app   "docker-entrypoint.s…"   8 seconds ago    Up 6 seconds    0.0.0.0:8000->3000/tcp con1
   ac2a310ccd59   react-app   "docker-entrypoint.s…"   13 minutes ago   Up 13 minutes   3000/tcp                blue_whale
   cbdcd325ae26   react-app   "docker-entrypoint.s…"   14 minutes ago   Up 14 minutes   3000/tcp                great_diffie
   ```

   In this case, the port `8000` on the host is mapped to port `3000` inside the container `con1`.

### 📲 Accessing the React Application on the Host

Now that port `8000` is published, you can access the React application running inside the Docker container by navigating to `http://localhost:8000` in your web browser. This URL routes traffic from the host's port `8000` to the container's port `3000`, where the React app is listening.

### 📝 Publishing Ports in Dockerfile

You can also define port publishing in the `Dockerfile` using the `EXPOSE` instruction. However, the `EXPOSE` instruction only serves as documentation for the person running the container and does not actually publish the port. To publish the port, you still need to use the `-p` flag when running the container.

Here’s an example of a `Dockerfile` with an `EXPOSE` instruction:

```dockerfile
# Use the official Node.js image with Alpine Linux
FROM node:14.17.0-alpine3.13

# Create a new user and group to run the application
RUN addgroup app && adduser -S -G app app

# Set the working directory for the application
WORKDIR /app

# Copy package.json and package-lock.json files to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install --no-cache

# Change ownership of the node_modules folder to the app user
RUN chown -R app:app /app/node_modules

# Copy the rest of the application files to the working directory
COPY . .

# Ensure that the app user has ownership of the entire app directory
RUN chown -R app:app /app

# Set the user to use when running this image
USER app

# Set environment variables
ENV API_URL=http://localhost:3000/myapi

# Expose port 3000 to the outside world
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
```

### 🧑‍💻 Using EXPOSE in Dockerfile

- **EXPOSE 3000**: This line indicates that the application inside the container will be listening on port `3000`. It’s a good practice to include this in your Dockerfile to document which ports your application will be using.

### 🚀 Publishing Ports When Running Containers

When you run a container using a Docker image built from the above `Dockerfile`, you should use the `-p` flag to publish the port:

```bash
docker run -d -p 8080:3000 <image_name>
```

This command will map port `8080` on your host machine to port `3000` inside the container.

### 📝 Summary

- **Publishing Ports**: The `-p` flag is used to map container ports to host ports.
- **Accessing the Application**: Once a port is published, the application inside the container can be accessed via `http://localhost:<host_port>`.
- **Dockerfile EXPOSE Instruction**: The `EXPOSE` instruction in a Dockerfile is for documentation and does not actually publish the port. You need to use the `-p` flag when running the container.

This approach ensures that your applications running inside Docker containers are accessible as needed and that you have control over the port mappings between the container and the host machine.