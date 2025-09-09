# 📄 Viewing Logs in Docker Containers

## 🌟 Introduction to Docker Logs

When working with Docker containers, especially in detached mode, you might not see the output directly as you would in the foreground mode. This is where Docker logs come in handy. They allow you to view the output of a container, helping you troubleshoot, monitor, and understand what's happening inside.

## 🛠️ Running Containers in Detached Mode

Let's start by running a container in detached mode. Detached mode (`-d`) allows the container to run in the background, freeing up your terminal for other tasks.

```bash
docker run -d react-app
```

**Output:**

```bash
cbdcd325ae26995a1ddb2996c7520266a190e6f0f66e255d4a113c696e6d67a5
```

Now, if we list the running containers:

```bash
docker ps
```

**Output:**

```bash
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS      NAMES
cbdcd325ae26   react-app   "docker-entrypoint.s…"   46 seconds ago   Up 45 seconds   3000/tcp   great_diffie
```

### 📝 Starting Another Container with a Custom Name

You can start another container and give it a custom name:

```bash
docker run -d --name blue_whale react-app
```

**Output:**

```bash
ac2a310ccd598f93abefdfb3e95b619685003f8f79cd0d9484b9ae45733b5e36
```

Checking the running containers again:

```bash
docker ps
```

**Output:**

```bash
CONTAINER ID   IMAGE       COMMAND                  CREATED              STATUS              PORTS      NAMES
ac2a310ccd59   react-app   "docker-entrypoint.s…"   4 seconds ago        Up 3 seconds        3000/tcp   blue_whale
cbdcd325ae26   react-app   "docker-entrypoint.s…"   About a minute ago   Up About a minute   3000/tcp   great_diffie
```

## 🔍 Viewing Logs of a Container

To view the logs of a running or stopped container, you use the `docker logs` command.

### 🔥 Example: Viewing Logs

```bash
docker logs ac2a
```

**Output:**

```bash
> my-app@0.1.0 start /app
> react-scripts start

(node:24) [DEP_WEBPACK_DEV_SERVER_ON_AFTER_SETUP_MIDDLEWARE] DeprecationWarning: 'onAfterSetupMiddleware' option is deprecated. Please use the 'setupMiddlewares' option.
(Use `node --trace-deprecation ...` to show where the warning was created)
(node:24) [DEP_WEBPACK_DEV_SERVER_ON_BEFORE_SETUP_MIDDLEWARE] DeprecationWarning: 'onBeforeSetupMiddleware' option is deprecated. Please use the 'setupMiddlewares' option.
Starting the development server...

One of your dependencies, babel-preset-react-app, is importing the
"@babel/plugin-proposal-private-property-in-object" package without
declaring it in its dependencies. This is currently working because
"@babel/plugin-proposal-private-property-in-object" is already in your
node_modules folder for unrelated reasons, but it may break at any time.

babel-preset-react-app is part of the create-react-app project, which
is not maintained anymore. It is thus unlikely that this bug will
ever be fixed. Add "@babel/plugin-proposal-private-property-in-object" to
your devDependencies to work around this error. This will make this message
go away.

Compiled successfully!

You can now view my-app in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://172.17.0.3:3000

Note that the development build is not optimized.
To create a production build, use npm run build.

webpack compiled successfully
Compiling...
Compiled successfully!
webpack compiled successfully
```

### 🚀 Exploring Log Options

The `docker logs` command comes with several options to customize how you view the logs.

```bash
docker logs --help
```

**Output:**

```bash
Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Aliases:
  docker container logs, docker logs

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. "2013-01-02T13:23:37Z") or relative (e.g. "42m" for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. "2013-01-02T13:23:37Z") or relative (e.g. "42m" for 42 minutes)
```

### 📋 Tail Last Few Lines of Logs

You can use the `-n` option to display the last few lines of logs:

```bash
docker logs -n 5 ac2a3
```

**Output:**

```bash
webpack compiled successfully
Compiling...
Compiled successfully!
webpack compiled successfully
```

To display the last 10 lines:

```bash
docker logs -n 10 ac2a3
```

**Output:**

```bash
Local:            http://localhost:3000
On Your Network:  http://172.17.0.3:3000

Note that the development build is not optimized.
To create a production build, use npm run build.

webpack compiled successfully
Compiling...
Compiled successfully!
webpack compiled successfully
```

### ⏰ Adding Timestamps

To include timestamps with the logs, use the `-t` option:

```bash
docker logs -n 10 -t ac2a3
```

**Output:**

```bash
2024-08-12T09:00:17.600712254Z   Local:            http://localhost:3000
2024-08-12T09:00:17.600828861Z   On Your Network:  http://172.17.0.3:3000
2024-08-12T09:00:17.600841562Z
2024-08-12T09:00:17.600854563Z Note that the development build is not optimized.
2024-08-12T09:00:17.601031674Z To create a production build, use npm run build.
2024-08-12T09:00:17.601043074Z
2024-08-12T09:00:17.610588349Z webpack compiled successfully
2024-08-12T09:00:17.723080218Z Compiling...
2024-08-12T09:00:17.963690398Z Compiled successfully!
2024-08-12T09:00:17.967364019Z webpack compiled successfully
```

## 🎯 Purpose of Viewing Logs

Viewing logs is crucial for several reasons:

1. **Troubleshooting**: Logs provide insight into what’s happening inside your container, helping you diagnose issues.
2. **Monitoring**: Keep track of your application's behavior and performance.
3. **Debugging**: Identify and fix bugs or configuration issues.
4. **Auditing**: Logs can serve as a record of activity, which is useful for auditing and compliance.

## 💡 Conclusion

Logs are an essential tool for managing and understanding your Docker containers. They give you visibility into your container’s activities, even when it’s running in detached mode. By mastering Docker logs, you’ll be better equipped to troubleshoot, monitor, and maintain your containerized applications effectively.

