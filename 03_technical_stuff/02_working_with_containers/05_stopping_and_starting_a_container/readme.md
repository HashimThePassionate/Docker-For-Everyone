# Stoping and Starting a Container 🚀

## What is the difference between `docker start` and `docker run`? 🤔

Before diving into the code, it's essential to understand the difference between two commonly used Docker commands: `docker start` and `docker run`.

- **`docker run`**: This command is used to create and start a new container from an image. It is generally used when you want to create a container from an image and run it for the first time.

- **`docker start`**: This command is used to start an existing container that was previously created (and possibly stopped). It doesn't create a new container but simply starts one that already exists.

### Example Code 💻

Let's look at an example of how to start and stop a container using Docker.

#### Step 1: Start the Container 🟢

To start a container named `con1`, use the following command:

```bash
docker start con1
```

**Output:**

```bash
con1
```

This output indicates that the container `con1` has been successfully started.

#### Step 2: Verify the Container is Running ✅

To check the status of the running container, you can use the `docker ps` command:

```bash
docker ps
```

**Output:**

```bash
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS         PORTS                    NAMES
1d643b67ac34   react-app   "docker-entrypoint.s…"   18 minutes ago   Up 7 seconds   0.0.0.0:8000->3000/tcp   con1
```

Here, you can see that the container `con1` is running, as indicated by the `STATUS` column showing "Up 7 seconds."

#### Step 3: Stop the Container 🛑

To stop the running container `con1`, you can use the `docker stop` command:

```bash
docker stop con1
```

**Output:**

```bash
con1
```

This output shows that the container `con1` has been successfully stopped.

#### Step 4: Verify the Container is Stopped 🔍

To confirm that the container has been stopped, run the `docker ps` command again:

```bash
docker ps
```

**Output:**

```bash
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

The output is empty, indicating that there are no running containers at the moment.

### Conclusion 🎯

In this guide, we explored how to start and stop a Docker container using the `docker start` and `docker stop` commands. Understanding these commands is crucial for managing the lifecycle of your containers effectively.
