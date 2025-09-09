# Connecting to the Host 🖥️

Connecting to your Docker host is an essential step in managing and deploying your applications. In this guide, we'll walk through the commands needed to connect to a Docker host using Docker Machine and SSH.

## Step 1: List Docker Machines 📝

Before connecting to a Docker host, it's helpful to list all available Docker Machines using the `docker-machine ls` command.

### Command:
```bash
docker-machine ls
```

### Output Explanation:
```plaintext
NAME     ACTIVE   DRIVER         STATE     URL                        SWARM   DOCKER  
vidly2   -        digitalocean   Running   tcp://***.***.***.***:2376           v19.03.9
```

- **NAME**: The name of the Docker Machine instance. In this case, it's `vidly2`.
- **ACTIVE**: Indicates if the Docker Machine is currently active. A `-` means it's not the active machine.
- **DRIVER**: The driver used to create the Docker Machine. Here, it's `digitalocean`, meaning the machine is hosted on DigitalOcean.
- **STATE**: The current state of the machine. `Running` means the machine is up and operational.
- **URL**: The URL to connect to the Docker Engine running on this machine. The IP address `***.***.***.***` is the public IP of the DigitalOcean Droplet, and `2376` is the port Docker is listening on.
- **SWARM**: Indicates whether the Docker Machine is part of a Docker Swarm. This is blank, meaning Swarm is not configured.
- **DOCKER**: The version of Docker running on this machine. In this case, it's `v19.03.9`.

## Step 2: Connect to the Docker Machine via SSH 🔐

### What is SSH? 🛡️

**SSH (Secure Shell)** is a protocol that provides a secure way to remotely access a server or machine over a network. It allows you to execute commands, manage files, and configure services on a remote machine securely.

To connect to your Docker Machine via SSH, use the following command:

### Command:
```bash
docker-machine ssh vidly2
```

### Explanation:
- **`docker-machine ssh vidly2`**: This command uses Docker Machine to establish an SSH connection to the `vidly2` machine. Once connected, you'll be placed in a shell session on the remote machine, where you can execute commands directly on the server.

### Example Output:
```plaintext
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-122-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Aug 19 05:53:39 UTC 2024

  System load:  0.0               Users logged in:          0
  Usage of /:   8.6% of 24.05GB   IPv4 address for docker0: 172.17.0.1
  Memory usage: 26%               IPv4 address for eth0:    ***.***.***.***
  Swap usage:   0%                IPv4 address for eth0:    10.17.0.5
  Processes:    106               IPv4 address for eth1:    10.108.0.2

207 updates can be applied immediately.
164 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable
```

#### Key Information:
- **System Information**: Displays current system stats such as load, memory usage, swap usage, and the number of processes.
- **IP Addresses**: Lists the IP addresses for different network interfaces:
  - `docker0`: The bridge network interface used by Docker containers.
  - `eth0`: The public IP address of the server, `***.***.***.***`.
  - `eth1` and other interfaces may show internal or private IP addresses.

Once connected, you're in a secure shell session on the `vidly2` server, where you can navigate the file system and manage the server.

### Example Commands in SSH:

1. **List Files in Home Directory**:
   ```bash
   ls
   ```
   Output:
   ```plaintext
   snap
   ```

2. **Navigate to Root Directory**:
   ```bash
   cd ..
   ```

3. **List Files in Root Directory**:
   ```bash
   ls
   ```
   Output:
   ```plaintext
   bin   dev  home  lib32  libx32      media  opt   root  sbin  srv  tmp  var
   boot  etc  lib   lib64  lost+found  mnt    proc  run   snap  sys  usr
   ```

4. **Return to Home Directory**:
   ```bash
   cd ~
   ```

5. **List Files Again in Home Directory**:
   ```bash
   ls
   ```
   Output:
   ```plaintext
   snap
   ```

### Security Note:
For security purposes, always ensure that sensitive information, such as API keys and passwords, are not exposed in public logs or screenshots. If any sensitive data is shown in your output, replace it with asterisks (****) before sharing or publishing.

---

## Conclusion 🎯

By following these steps, you can successfully connect to your Docker host using Docker Machine and SSH. This allows you to manage your remote Docker environments securely and efficiently. Understanding the commands and their outputs provides a solid foundation for managing Docker hosts in production environments.
