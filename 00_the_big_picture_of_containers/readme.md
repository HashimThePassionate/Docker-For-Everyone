# Hello World Container Operations

1. Overview
[Read Chapters 1 and 7 of our Text Book: Docker Deep Dive by Nigel Poulton 2023 Edition](https://www.amazon.com/Docker-Deep-Dive-Nigel-Poulton/dp/1916585256)

2. Installing Docker Desktop (Chapter 3)

https://docs.docker.com/get-docker/

    docker version

Test Docker Installation:

    docker run hello-world

**For Windows:**

- 64-bit version of Windows 10/11
- Hardware virtualization support must be enabled in your system’s BIOS
- WSL 2

[How to Install WSL2 with Ubuntu](https://youtu.be/J2PQuVAI99c?si=X-lg60sGq6PkkD5P)

[Docker for Windows Installation and Troubleshooting for Beginners](https://youtu.be/R4uy6Oqiy5I?si=DglDYuvf-zvFY9bS)

**Multipass:**

[Running a container with the Docker blueprint in Multipass](https://multipass.run/docs/docker-tutorial)


**For Mac**, if docker command not running:

https://stackoverflow.com/questions/64009138/docker-command-not-found-when-running-on-mac

https://www.insightsjava.com/2022/01/how-to-create-bash-profile-on-mac.html

3. Play with Docker

Play with Docker (PWD) is a fully functional internet-based Docker playground that lasts for 4 hours. You can add multiple nodes and even cluster them in a swarm.


# Getting Started with Docker
1. Create a file Dockerfile inside in your project and Write
```docker
FROM python:alpine
COPY . /app
# CMD python /app/main.py
WORKDIR /app
CMD python main.py
``` 
2. FROM python (means find python from docker hub and download)
3. COPY ./app (means copy everything from current dir and paste it to app directory)
4. WORKDIR /app (means we are already in working directory which is /app)
5. CMD python main.py (means run python file main.py)

# Now we need to build docker file
1. i assume we are already in current directory which is 
<pre>
C:\Users\aaaa\Desktop\docker\Docker\00_getting_started_ops\Hello-world>
</pre>
2. Run this commnad in cmd to build docker
```docker
docker build -t hello-world .
```
3. docker build -t hello-world . (we are building docker and give a tag hello-world)
4. to see docker image list simple type in cmd
```
docker image ls
```
5. To run docker simple on command line just type this 
```
docker run hello-world
```