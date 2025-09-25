
# Setting the User in Docker 🐳

## Overview

In Docker, running containers as a non-root user is a best practice to enhance security. This involves creating a specific user and group for the application, assigning appropriate permissions, and ensuring that the container runs as this non-root user.

### Why Run as a Non-Root User? 🔒

1. **Security** 🛡️: Running containers as root poses security risks. If a container is compromised, it can potentially escalate privileges and affect the host system.
2. **Minimize Damage** ⚠️: Running as a non-root user restricts the user's capabilities, limiting the damage if the container is compromised.
3. **Compliance** 📜: Certain security policies and compliance standards require applications to not run as root.

### Why Matching User and Group Names is a Best Practice 📛

1. **Simplicity and Clarity** 🧩: Using the same name for both the user and group makes the permissions structure clear and easy to understand. For example, if you have a user named `app` and a group named `app`, it's immediately obvious that they are related and intended to control access to the same resources.

2. **Ease of Management** 🔧: When user and group names are the same, it's easier to manage file permissions and user access, especially in environments with multiple users and groups. You can quickly set ownership and permissions with a single command, reducing the risk of errors.

3. **Consistency** 📏: Maintaining a consistent naming convention across your system helps to avoid confusion and mistakes. If you consistently match user and group names, it becomes easier for you and others to understand and maintain the system.

### Step 1: Create a User and Group in Alpine Container

Let's start by running an Alpine Linux container and creating a user and group:

```bash
# docker run -it alpine
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
c6a83fedfae6: Pull complete
Digest: sha256:0a4eaa0eecf5f8c050e5bba433f58c052be7587ee8af3e8b3910ef9ab5fbe9f5
Status: Downloaded newer image for alpine:latest
/ # ls
bin    etc    lib    mnt    proc   run    srv    tmp    var
dev    home   media  opt    root   sbin   sys    usr       
/ # cd ~
~ # adduser
BusyBox v1.36.1 (2024-06-10 07:11:47 UTC) multi-call binary.

Usage: adduser [OPTIONS] USER [GROUP]

Create new user, or add USER to GROUP

        -h DIR          Home directory
        -g GECOS        GECOS field
        -s SHELL        Login shell
        -G GRP          Group
        -S              Create a system user
        -D              Don't assign a password
        -H              Don't create home directory
        -u UID          User id
        -k SKEL         Skeleton directory (/etc/skel)
~ # addgroup app
~ # adduser -S -G app app
~ # groups app
app
~ # addgroup hashim &&  adduser -S -G hashim hashim
~ # groups hashim
hashim
```

### Explanation:
- **`addgroup app`**: Creates a new group named `app`.
- **`adduser -S -G app app`**: Creates a new system user named `app` and adds it to the `app` group.
- **`groups app`**: Verifies that the user `app` belongs to the `app` group.
- **`addgroup hashim && adduser -S -G hashim hashim`**: Creates a new group and user named `hashim` and adds the user to the group `hashim`.

### Step 2: Dockerfile Configuration

You can automate this process in your Dockerfile to ensure the container runs as the non-root user:

```Dockerfile
# Use a lightweight Node.js base image
FROM node:14.17.0-alpine3.13

# Set the working directory inside the container
WORKDIR /app

# Copy all files and directories from the current directory on the host to /app in the container
COPY . .

# Install Node.js dependencies
RUN npm install

# Set an environment variable
ENV API_URL=http://localhost:3000/myapi

# Expose port 3000
EXPOSE 3000

# Create a group and user, and set permissions
RUN addgroup app && adduser -S -G app app

# Switch to the non-root user
USER app
```

### Step 3: Build the Docker Image

Now, build the Docker image:

```bash
docker build -t react-app .
```

### Step 4: Building the Image Output

Here’s what the build process might look like:

```bash
# docker build -t react-app .
[+] Building 271.1s (10/10) FINISHED                                        docker:default 
 => [internal] load build definition from Dockerfile                                  0.1s 
 => => transferring dockerfile: 217B                                                  0.0s 
 => [internal] load metadata for docker.io/library/node:14.17.0-alpine3.13            2.9s 
 => [internal] load .dockerignore                                                     0.1s 
 => => transferring context: 53B                                                      0.0s 
 => [1/5] FROM docker.io/library/node:14.17.0-alpine3.13@sha256:782e891986f16cc661bf  0.0s 
 => [internal] load build context                                                     0.2s 
 => => transferring context: 740.22kB                                                 0.2s 
 => CACHED [2/5] WORKDIR /app                                                         0.0s 
 => [3/5] COPY . .                                                                    0.2s 
 => [4/5] RUN npm install                                                           254.3s 
 => [5/5] RUN addgroup app &&  adduser -S -G app app                                  0.5s
 => exporting to image                                                               12.4s
 => => exporting layers                                                              12.2s 
 => => writing image sha256:1b12b148ab3ce323975d3d3c3789a027841d5db8c536ba809406f38f  0.0s 
 => => naming to docker.io/library/react-app                                          0.0s
```

### Step 5: Run the Docker Container

After building, run the container and verify the user:

```bash
# docker run -it react-app sh
/app $ whoami
app
```

### Step 6: Verify File Permissions

Inside the container, check the ownership and permissions of the files:

```bash
/app $ ls -l
total 632
-rwxr-xr-x    1 root     root           178 Aug 10 10:19 Dockerfile       
drwxr-xr-x  870 root     root         32768 Aug 10 10:25 node_modules     
-rwxr-xr-x    1 root     root        586679 Aug 10 10:25 package-lock.json
-rwxr-xr-x    1 root     root           809 Aug  9 12:22 package.json     
drwxr-xr-x    2 root     root          4096 Aug 10 10:10 public
-rwxr-xr-x    1 root     root             2 Aug 10 10:11 readme.md        
drwxr-xr-x    2 root     root          4096 Aug 10 10:10 src
```

### Explanation:
- The `whoami` command confirms that the container is running as the `app` user, not as root.
- The file permissions indicate that the root user still owns the critical files, protecting them from being modified by the `app` user.

### Why This Setup Enhances Security 🛡️

1. **Limited Permissions** 🔐: The `app` user has limited permissions and can only interact with files and directories that are explicitly allowed, reducing the risk of accidental or malicious actions.
2. **Root User Protection** 🛡️: Since the root user owns the critical application files, the `app` user cannot modify these files, preserving the integrity of the application and preventing unauthorized changes.
3. **Enhanced Security** 🔒: By running as a non-root user, you minimize the potential impact if the container is compromised, as the attacker would have limited access.

### Conclusion 🎯

- **Non-Root User**: Running the container as a non-root user significantly enhances security.
- **File Permissions**: Properly set file permissions ensure that only the necessary user can access or modify files, protecting the application’s integrity.
- **User-Group Naming Convention**: Matching the user and group names simplifies management, ensures clarity, and maintains consistency across your system.
- **Best Practices**: Including user creation and permissions settings in your Dockerfile ensures that your containers are secure by default.