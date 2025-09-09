# The Simple Full Stack Web Application 🌐

## Overview 📝

In this project, we have a full-stack web application comprising three main components:

- **Frontend Directory**: A React project for the client-side.
- **Backend Directory**: A Node.js project for the server-side.
- **MongoDB Database**: A NoSQL database for data storage.

Traditionally, setting up such a project involves several steps, including installing dependencies for both frontend and backend, and setting up the MongoDB database. However, with Docker and Docker Compose, these steps can be significantly simplified. Let's dive in! 🚀

## Project Structure 📂

```bash
$ ls
backend  docker-compose.yml  frontend
```

When you look at the project directory, you'll notice three main components:

1. **backend**: Contains the Node.js server-side code.
2. **frontend**: Contains the React client-side code.
3. **docker-compose.yml**: A Docker Compose file that defines how to run our multi-container application.

### What is Docker Compose? 🐳

Docker Compose is a tool that allows us to define and run multi-container Docker applications. With a single configuration file (`docker-compose.yml`), we can define all the services our application needs, and with a single command, we can start everything up.

### Running the Application with Docker Compose 🏃‍♂️

Once the `docker-compose.yml` file is in place, we don't need to manually install dependencies or set up the database. Docker Compose will take care of everything for us.

#### Step 1: Start the Docker Containers

To start the application, simply run the following command:

```bash
docker-compose up
```

Docker Compose will automatically:

- Install MongoDB
- Install frontend dependencies and start the React development server
- Install backend dependencies and start the Node.js server

#### Output Example 📊

```bash
time="2024-08-15T13:48:00+05:00" level=warning msg="C:\\Users\\aaaa\\Desktop\\Docker\\05_running_multicontainers_applications\\03_the_simple_web_application\\docker-compose.yml: version is obsolete"
[+] Running 13/13
 ✔ db Pulled                                                                             550.1s 
   ✔ 58690f9b18fc Pull complete                                                          245.8s 
   ✔ b51569e7c507 Pull complete                                                            1.7s 
   ✔ da8ef40b9eca Pull complete                                                            4.4s 
   ✔ fb15d46c38dc Pull complete                                                            3.7s 
   ✔ a0dc15b16822 Pull complete                                                            6.8s 
   ✔ b7a3e92f19af Pull complete                                                           40.6s 
   ✔ ed4a7b863fa1 Pull complete                                                           16.5s 
   ✔ a58b030ea8e4 Pull complete                                                           32.7s 
   ✔ 6aa1ba699846 Pull complete                                                           35.1s 
   ✔ ebc52c729dca Pull complete                                                           37.7s 
   ✔ 52e8c440d4d6 Pull complete                                                          540.8s 
   ✔ 22b97876323d Pull complete                                                           42.9s 
[+] Building 217.8s (19/19) FINISHED                                             docker:default 
 => [backend internal] load build definition from Dockerfile                               0.5s 
 => => transferring dockerfile: 217B                                                       0.1s 
 => [frontend internal] load metadata for docker.io/library/node:14.16.0-alpine3.13       11.3s
 => [backend auth] library/node:pull token for registry-1.docker.io                        0.0s
 => [backend internal] load .dockerignore                                                  0.2s
 => => transferring context: 53B                                                           0.0s 
 => [frontend 1/6] FROM docker.io/library/node:14.16.0-alpine3.13@sha256:2c51dc462a02f15  67.2s
```

Docker will pull necessary images, build the frontend and backend, and start the containers. The output will show the progress of each step, such as pulling images, building the frontend and backend, and starting the MongoDB database.

#### Step 2: Access the Application

Once the containers are up and running, you can access the frontend and backend as follows:

- **Frontend (React)**: [http://localhost:3000](http://localhost:3000)
- **Backend (Node.js)**: The backend will run on port `3001`, and you can use tools like Postman or your browser to interact with the API.

Here is the output from running the application:

```bash
frontend-1  | Compiled successfully!
frontend-1  | 
frontend-1  | You can now view vidly-frontend in the browser.
frontend-1  | 
frontend-1  |   Local:            http://localhost:3000
frontend-1  |   On Your Network:  http://172.18.0.4:3000
backend-1   | Server started on port 3001...
backend-1   | Connected to MongoDB: mongodb://db/vidly
```

- The frontend server is running on [http://localhost:3000](http://localhost:3000).
- The backend server is running on port 3001 and is connected to the MongoDB database.

## Conclusion 🎉

Using Docker and Docker Compose, we have streamlined the process of setting up and running a full-stack web application. With just a single command, we can have our entire application up and running, without the need for manual dependency installations or setup steps.

This approach not only saves time but also ensures consistency across different development environments. In the upcoming sections, we will dive deeper into how Docker Compose works and explore the `docker-compose.yml` file in detail. Stay tuned! 🌟