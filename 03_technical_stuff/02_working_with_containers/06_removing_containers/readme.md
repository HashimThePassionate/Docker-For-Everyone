# Removing Containers 🗑️

Managing Docker containers involves not only starting and stopping them but also removing them when they are no longer needed. In this guide, we will explore how to remove Docker containers using various commands.

### Listing All Containers 📝

Before removing any containers, you can list all containers, including those that are stopped, by using the following command:

```bash
docker ps -a
```

**Output:**

```bash
CONTAINER ID   IMAGE       COMMAND                  CREATED              STATUS              PORTS       NAMES
9f3b1efcabf0   react-app   "docker-entrypoint.s…"   About a minute ago   Up About a minute   3000/tcp    con3
ee575e9a2e2d   react-app   "docker-entrypoint.s…"   About a minute ago   Up About a minute   3000/tcp    con2
80bd256456ba   react-app   "docker-entrypoint.s…"   2 minutes ago        Up 2 minutes        3000/tcp    sr71
1d643b67ac34   react-app   "docker-entrypoint.s…"   31 minutes ago       Exited (0) 15 seconds ago        con1
6728b51b25d5   react-app   "docker-entrypoint.s…"   32 minutes ago       Up About a minute   3000/tcp    blue_whale
```

This command lists all containers, along with their status. Containers that are stopped will have a status of `Exited`.

### Removing a Specific Container 🛠️

To remove a specific container, you can use the `docker rm` command. However, the container must be stopped before it can be removed.

#### Step 1: Start the Container (if needed) 🟢

If the container is stopped and you want to start it before removing, you can use:

```bash
docker start con1
```

**Output:**

```bash
con1
```

This starts the container named `con1`.

#### Step 2: Remove the Container 🗑️

Now, let's try to remove the container named `con1`:

```bash
docker rm con1
```

**Error Output:**

```bash
Error response from daemon: cannot remove container "/con1": container is running: stop the container before removing or force remove
```

The above error occurs because the container is running. You must stop the container before removing it or use the `-f` (force) option to remove it.

#### Step 3: Force Remove the Container 🚨

To forcefully remove a running container, use the `-f` option:

```bash
docker rm -f con1
```

**Output:**

```bash
con1
```

The container `con1` is now removed successfully.

### Verifying the Removal 🔍

To confirm that the container has been removed, list all containers again:

```bash
docker ps -a
```

**Output:**

```bash
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS         PORTS        NAMES
9f3b1efcabf0   react-app   "docker-entrypoint.s…"   2 minutes ago    Up 2 minutes   3000/tcp     con3
ee575e9a2e2d   react-app   "docker-entrypoint.s…"   3 minutes ago    Up 3 minutes   3000/tcp     con2
80bd256456ba   react-app   "docker-entrypoint.s…"   3 minutes ago    Up 3 minutes   3000/tcp     sr71
6728b51b25d5   react-app   "docker-entrypoint.s…"   33 minutes ago   Up 2 minutes   3000/tcp     blue_whale
```

As you can see, the container `con1` is no longer in the list.

### Removing All Stopped Containers 🧹

To clean up all stopped containers, you can use the `docker container prune` command:

```bash
docker container prune
```

**Output:**

```bash
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Total reclaimed space: 0B
```

This command will remove all containers that are currently stopped, helping to free up system resources.

### Conclusion 🎯

In this guide, we covered how to remove Docker containers using the `docker rm` and `docker container prune` commands. By managing your containers effectively, you can maintain a clean and efficient Docker environment.