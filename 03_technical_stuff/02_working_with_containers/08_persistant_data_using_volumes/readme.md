# Persisting Data Using Volumes 📂💾

### What are Volumes in Docker? 🤔

In Docker, **volumes** are a way to persist data generated and used by containers. Unlike the default container file system, which is ephemeral (meaning it disappears once the container is removed), volumes allow data to be stored outside the container's writable layer, ensuring that it remains accessible even if the container is deleted or recreated. 🎉

Volumes are the preferred method for persisting data in Docker because:

- **Data Independence**: Volumes exist outside the container lifecycle, so deleting a container doesn't delete the volume or its data. 🔄
- **Performance**: Volumes provide better performance for heavy read/write operations compared to the container's file system. ⚡
- **Shared Storage**: Volumes can be shared between multiple containers, making them ideal for scenarios where multiple containers need to access the same data. 🤝

### Docker Volume Commands 🧰

Docker provides a set of commands to manage volumes:

- **`docker volume create`**: Creates a new volume. 🆕
- **`docker volume inspect`**: Displays detailed information about one or more volumes. 🕵️‍♂️
- **`docker volume ls`**: Lists all volumes on the host. 📋
- **`docker volume prune`**: Removes all unused local volumes. 🧹
- **`docker volume rm`**: Removes one or more volumes. 🗑️

To get more information on any specific command, you can use:

```bash
docker volume COMMAND --help
```

### Creating a Volume 🛠️

Let's create a volume named `app-data`:

```bash
docker volume create app-data
```

**Output:**

```bash
app-data
```

This command creates a new volume named `app-data` that can be used to persist data. 📂

### Inspecting the Volume 🔍

To view detailed information about the `app-data` volume, use the following command:

```bash
docker volume inspect app-data
```

**Output:**

```json
[
    {
        "CreatedAt": "2024-08-14T06:36:35Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/app-data/_data",
        "Name": "app-data",
        "Options": null,
        "Scope": "local"
    }
]
```

**Explanation:**

- **CreatedAt**: The timestamp of when the volume was created. ⏰
- **Driver**: The driver used to manage the volume, typically `local`. 🛠️
- **Mountpoint**: The path on the host where the volume data is stored. 🗂️
- **Name**: The name of the volume. 🏷️
- **Scope**: Indicates whether the volume is local or global. 🌍

### Running a Container with a Volume 🚀

To use the `app-data` volume with a container, you can run the following command:

```bash
docker run -d -p 5000:3000 -v app-data:/app/data react-app
```

**Explanation:**

- **`-d`**: Runs the container in detached mode. 🛫
- **`-p 5000:3000`**: Maps port 5000 on the host to port 3000 in the container. 🔌
- **`-v app-data:/app/data`**: Mounts the `app-data` volume to the `/app/data` directory inside the container. 📁
- **`react-app`**: The Docker image used to create the container. 🖼️

This command starts the `react-app` container, with the `app-data` volume mounted to `/app/data`. 🎉

### Accessing the Container's Shell 🔧

To interact with the container's file system, you can start a shell session inside the container:

```bash
docker exec -it 6ea5 sh
```

Once inside the container, list the files in the `/app` directory:

```bash
ls
```

**Output:**

```bash
Dockerfile         false              package-lock.json  public             src
data               node_modules       package.json       readme.md
```

Navigate to the `data` directory:

```bash
cd data/
```

Try to create a file in the `data` directory:

```bash
echo Hashim > data.txt
```

**Error:**

```bash
sh: can't create data.txt: Permission denied
```

### Understanding Permission Denied 🚫

The "Permission denied" error occurs because the `data` directory is owned by the `root` user, while the `app` user (which is the user running the container) does not have write permissions to this directory. 👮‍♂️

Let's verify the directory's permissions:

```bash
ls -l
```

**Output:**

```bash
total 756
-rwxr-xr-x    1 app      app            918 Aug 12 08:33 Dockerfile
drwxr-xr-x    2 root     root          4096 Aug 14 06:36 data
drwxr-xr-x    1 app      app           4096 Aug 14 06:44 node_modules
...
```

The `data` directory is owned by the `root` user (`drwxr-xr-x`), which means only the `root` user has write permissions. ✋

### Fixing the Permissions ⚙️

To resolve the permission issue, you need to update the `Dockerfile` to set the correct ownership of the directories:

```dockerfile
# Use the official Node.js image with Alpine Linux
FROM node:14.17.0-alpine3.13

# Create a new user and group to run the application
RUN addgroup app && adduser -S -G app app

# Set the working directory for the application
WORKDIR /app

RUN mkdir data

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

This updated `Dockerfile` includes commands to set the correct ownership of the `data` directory and other application files, ensuring that the `app` user has the necessary permissions. ✔️

### Building the Docker Image 🏗️

After updating the `Dockerfile`, build the Docker image again:

```bash
docker build -t react-app .
```

**Output:**

```bash
 => exporting to image                                                                                  7.9s
 => => exporting layers                                                                                 7.8s
 => => writing image sha256:6d45ab3fc466674fdb467484cece0d33b7c6ac6cd3917fcb66236c3f7a10e449            0.0s
 => => naming to docker.io/library/react-app 
```

This command rebuilds the image with the necessary changes. 🚀

### Running the Container Again 🏃‍♂️

Run the container with the updated image:

```bash
docker run -d -p 5000:3000 -v app-data:/app/data react-app
```

Start a shell session inside the container:

```bash
docker exec -it 20e206 sh
```

Navigate to the `data` directory:

```bash
cd data/
```

Create a file and verify its content:

```bash
echo Hashim > data.txt
cat data.txt
```

**Output:**

```bash
Hashim
```

This confirms that the permission issue has been resolved, and data is successfully persisted in the `app-data` volume. ✅

### Removing the Container 🗑️

To remove the running container, use the following command:

```bash
docker rm -f 20e206
```

### Verifying Data Persistence 🔍

Even after removing the container, the data persists in the volume. To verify this, run a new container and check the `data.txt` file:

```bash
docker run -d -p 5000:3000 -v app-data:/app/data react-app
docker exec -it 8b7e sh
cd data/
ls
```

**Output:**

```bash
data.txt
```

View the content of the `data.txt` file:

```bash
cat data.txt
```

**Output:**

```bash
Hashim
```

The data persists, demonstrating the effectiveness of using Docker volumes for persisting data. 🥳

### Conclusion 🎯

In this guide, we explored the concept of Docker volumes, how to manage them, and how to use them to persist data across container lifecycles. By ensuring proper permissions and using volumes, you can effectively manage persistent data in your Docker applications. 🚀
