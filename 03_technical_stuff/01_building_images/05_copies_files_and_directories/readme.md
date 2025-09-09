# Copying Files and Directories in Docker 🐳
In Docker, the `COPY` and `ADD` instructions are essential for copying files and directories from your host machine into the Docker image during the build process. These commands help you package your application code, configuration files, and other necessary data into your Docker container.

### Dockerfile Example

Here's a basic Dockerfile using the `COPY` command:

```Dockerfile
# Use a lightweight Node.js base image
FROM node:14.17.0-alpine3.13

# Set the working directory inside the container
WORKDIR /app

# Copy all files and directories from the current directory on the host to /app in the container
COPY . .
```

This Dockerfile does the following:
1. **FROM**: Specifies the base image `node:14.17.0-alpine3.13`.
2. **WORKDIR**: Sets the working directory inside the container to `/app`.
3. **COPY**: Copies all files and directories from the current directory on the host machine to the `/app` directory in the container.

## Using the `COPY` Command 📦

The `COPY` command is straightforward but powerful. Below are different ways to use it, along with examples.

### Basic Syntax

```Dockerfile
COPY [source path] [destination path]
```

### Examples of `COPY` Command 🛠️

1. **Copying a Single File** 📄
   - **Command**: `COPY index.html /usr/share/nginx/html/`
   - **Description**: Copies `index.html` from the current directory on the host to the `/usr/share/nginx/html/` directory in the container.

2. **Copying Multiple Files** 📂
   - **Command**: `COPY ["file1.txt", "file2.txt", "/app/"]`
   - **Description**: Copies `file1.txt` and `file2.txt` into the `/app/` directory in the container.

3. **Copying Directories** 📁
   - **Command**: `COPY ./src /app/src`
   - **Description**: Copies the entire `src` directory from the current directory on the host to `/app/src` in the container.

4. **Copying Files with Wildcards** 🃏
   - **Command**: `COPY *.txt /app/textfiles/`
   - **Description**: Copies all `.txt` files from the current directory on the host to `/app/textfiles/` in the container.

5. **Copying and Renaming** 📝
   - **Command**: `COPY src/index.html /app/renamed-index.html`
   - **Description**: Copies `src/index.html` from the host to `/app/renamed-index.html` in the container.

6. **Using Dockerignore** 🚫
   - **Command**: `COPY . .`
   - **Description**: Copies everything from the current directory into the container, excluding files specified in `.dockerignore`.

## Using the `ADD` Command ➕

The `ADD` command is similar to `COPY` but offers additional functionality, such as downloading files from a URL or automatically extracting compressed files.

### Examples of `ADD` Command 🛠️

1. **Adding a Local File** 📄
   - **Command**: `ADD index.html /usr/share/nginx/html/`
   - **Description**: Similar to `COPY`, it adds `index.html` to the specified directory in the container.

2. **Adding a Remote File** 🌐
   - **Command**: `ADD http://example.com/file.tar.gz /app/`
   - **Description**: Downloads `file.tar.gz` from the specified URL and adds it to the `/app/` directory in the container.

3. **Extracting a Tarball** 🗂️
   - **Command**: `ADD archive.tar.gz /app/`
   - **Description**: Copies `archive.tar.gz` from the host to the container and automatically extracts it into the `/app/` directory.

4. **Adding Files with Wildcards** 🃏
   - **Command**: `ADD ["*.txt", "/app/textfiles/"]`
   - **Description**: Copies all `.txt` files from the host to `/app/textfiles/` in the container.

### Important Differences Between `COPY` and `ADD` 🔄

- **`COPY`** is simpler and should be used when you only need to copy files and directories from the host.
- **`ADD`** provides additional functionality like downloading remote files and extracting archives. It's recommended to use `COPY` unless you specifically need the extra features of `ADD`.

## Build and Run Example 🛠️

Here’s an example of building and running a Docker container using the `COPY` command from the Dockerfile above:

### Step 1: Build the Docker Image

```bash
# docker build -t react-app .
```

#### Build Output

```bash
[+] Building 2.8s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                                                                               0.1s
 => => transferring dockerfile: 89B                                                                                                0.0s
 => [internal] load metadata for docker.io/library/node:14.17.0-alpine3.13                                                         2.3s
 => [internal] load .dockerignore                                                                                                  0.1s
 => => transferring context: 2B                                                                                                    0.0s
 => CACHED [1/3] FROM docker.io/library/node:14.17.0-alpine3.13@sha256:782e891986f16cc661bfe928d0d163d4d0e6cf5cc05453dff2093c015f  0.0s
 => [internal] load build context                                                                                                  0.1s
 => => transferring context: 535.87kB                                                                                              0.1s
 => [2/3] WORKDIR /app                                                                                                             0.1s
 => [3/3] COPY . .                                                                                                                 0.1s
 => exporting to image                                                                                                             0.1s
 => => exporting layers                                                                                                            0.0s
 => => writing image sha256:659422295c8dca497a60945b1a1efd703d6effe9983597ccd1455ca5086a797d                                       0.0s
 => => naming to docker.io/library/react-app                                                                                       0.0s

View build details: docker-desktop://dashboard/build/default/default/w48kmb8wrejfoobrnjd8apygn
```

### Step 2: Run the Docker Container

```bash
# docker run -it react-app sh
```

### Step 3: Verify Files Inside the Container

```bash
/app # ls
Dockerfile    package.json  public        readme.md     src           yarn.lock
```

In this output, you can see that all the files and directories from your project (`Dockerfile`, `package.json`, `public`, `readme.md`, `src`, `yarn.lock`) have been copied into the `/app` directory of the container.

## Conclusion 🎯

- **COPY** 📦: Use this command for simple file and directory copying. It's the most straightforward way to get your files into your Docker container.
- **ADD** ➕: Use this command when you need to download files from a URL or automatically extract compressed files.
- **Building and Running** 🛠️: The example demonstrated how to build a Docker image with the `COPY` command and verify that files were correctly copied into the container.