# Docker Networking

## Introduction 🌟

Docker Networking is a fundamental aspect of containerization that allows different containers to communicate with each other. In Docker, networking is handled in such a way that each container gets its own network namespace, effectively isolating it from other containers unless explicitly connected. This allows for the creation of complex, multi-container applications that can talk to each other seamlessly.

In this guide, we'll explore Docker networking by looking at how containers communicate within a Docker network, how they can ping each other, and how services like API and DB can interact using environment variables. We'll also delve into the `docker-compose` commands and networking details to give you a comprehensive understanding.

## Docker Networking Basics 🌐

### Creating and Managing Networks 🛠️

When you run a Docker Compose file, Docker automatically creates a network for your containers. This network allows containers to discover each other and communicate using simple hostnames.

Let's start by examining the output of some commands:

```bash
docker-compose up -d 
```

**Output Example:**
```bash
[+] Running 4/4
 ✔ Network 08_docker_networking_default  Created                                                                   0.1s 
 ✔ Container 08_docker_networking-db-1   Started                                                                   1.4s 
 ✔ Container 08_docker_networking-api-1  Started                                                                   2.1s 
 ✔ Container 08_docker_networking-web-1  Started                                                                   2.5s 
```

Here, Docker created a network named `08_docker_networking_default` and started three containers (`web`, `api`, and `db`), each connected to this network.

You can list all networks using the following command:

```bash
docker network ls
```

**Output Example:**
```bash
NETWORK ID     NAME                           DRIVER    SCOPE
00ca9c753d2e   08_docker_networking_default   bridge    local
1cae35e002ac   bridge                         bridge    local
81ca756a92c1   host                           host      local
5e1d192522d2   none                           null      local
```

The `08_docker_networking_default` network is a custom bridge network created by Docker Compose. The `bridge` network is the default network Docker uses when no specific network is specified.

## How Containers Communicate 🗣️

### Communication Between Containers 📡

In a Docker network, containers can communicate with each other using the container name as a hostname. For instance, if you have a container named `api` and another named `web`, the `web` container can ping the `api` container by simply using the hostname `api`.

#### Example: Pinging Between Containers

```bash
docker exec -it -u root 85bb sh
/app # ping api
```

**Output Example:**
```bash
PING api (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.159 ms
64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.155 ms
...
```

In this example:
- The `web` container (`85bb`) pings the `api` container.
- The IP address of the `api` container is `172.18.0.3`.
- The `ping` command shows that packets are successfully sent and received between the containers.

This is possible because both containers are on the same Docker network (`08_docker_networking_default`), allowing them to resolve each other's hostnames.

### Network Configuration Inside a Container 🔧

You can check the network configuration inside a container using the `ifconfig` command:

```bash
docker exec -it -u root 85bb sh
/app # ifconfig
```

**Output Example:**
```bash
eth0      Link encap:Ethernet  HWaddr 02:42:AC:12:00:04
          inet addr:172.18.0.4  Bcast:172.18.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:81 errors:0 dropped:0 overruns:0 frame:0
          TX packets:72 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:7658 (7.4 KiB)  TX bytes:6720 (6.5 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:4 errors:0 dropped:0 overruns:0 frame:0
          TX packets:4 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:215 (215.0 B)  TX bytes:215 (215.0 B)
```

In this example:
- **`eth0`**: The network interface associated with the Docker network, with an IP address of `172.18.0.4`.
- **`lo` (loopback)**: The local loopback interface, commonly used for testing and local network communications.

### Text Representation of Container Communication 🖼️

Let's represent the communication between containers in a tabular format:

| Container Name | IP Address   | Can Communicate With  |
|----------------|--------------|-----------------------|
| `web`          | `172.18.0.4` | `api`, `db`           |
| `api`          | `172.18.0.3` | `web`, `db`           |
| `db`           | `172.18.0.2` | `web`, `api`          |

Each container can communicate with others by their names (e.g., `api`, `db`, etc.), and the DNS resolution within the Docker network takes care of mapping the names to their respective IP addresses.

## Understanding Service Dependencies 🚀

### `depends_on` and Environment Variables 🧩

In your `docker-compose.yml` file, you can specify dependencies between services using the `depends_on` keyword. This ensures that a service (e.g., `api`) only starts after its dependencies (e.g., `db`) have been started.

#### Example:

```yaml
services:
  api: 
    depends_on: 
      - db
    build: ./backend
    ports: 
      - 3001:3001
    environment: 
      DB_URL: mongodb://db/vidly
```

**Explanation:**
- **`depends_on`**: The `api` service depends on the `db` service. Docker Compose ensures that the `db` service is started before the `api` service.
- **`environment`**: The `DB_URL` environment variable is set to `mongodb://db/vidly`, where:
  - `mongodb://` is the protocol for MongoDB connections.
  - `db` is the hostname (container name) of the database service.
  - `vidly` is the name of the database.

This setup allows the `api` service to connect to the `db` service using the environment variable `DB_URL`, which resolves to the correct IP address within the Docker network.

### Visualizing Container Communication 🎨

Containers communicate within a Docker network using IP addresses assigned by Docker. Here's a simple illustration of how this communication works:

```
[ Container: web ]                 <-- (172.18.0.4) ->                        [ Container: api ]
        |                                                                             |
   (3000:3000)                                                                   (3001:3001)
        |                                                                             |
[ Container: db]                    <-- (172.18.0.2)                      <-- [ Container: api ]
        |                                                                             |
  (27017:27017)                                                                  (27017:27017)
```

- **Web (Container `web`)**: Communicates with `api` and `db` containers using the Docker network.
- **API (Container `api`)**: Connects to the `db` container using the `DB_URL` environment variable.

## Conclusion 🎉

Docker networking simplifies the communication between containers, making it possible to build and deploy complex, multi-container applications. By understanding how containers communicate within a Docker network and how to configure service dependencies, you can create robust, scalable applications that leverage Docker's full potential.

With Docker Compose, networking is abstracted in a way that allows you to focus more on your application's functionality and less on the underlying infrastructure. Whether it's through `ping` commands, network interfaces, or environment variables, Docker networking is the backbone of modern containerized applications.

