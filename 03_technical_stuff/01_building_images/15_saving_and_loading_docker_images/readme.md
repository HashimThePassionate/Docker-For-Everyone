# 🚀 Saving and Loading Docker Images

## Overview 🌟

Docker allows you to **save** an image to a tar archive file and **load** it later. This is useful for sharing images without using a Docker registry like Docker Hub. You can save the image as a file, transfer it to another system, and load it back into Docker on that system.

## 🖼️ Listing Docker Images

Before we start, let’s list the available Docker images on your local machine:

```bash
docker images
```

**Output:**

```bash
REPOSITORY                      TAG       IMAGE ID       CREATED        SIZE  
react-app                       latest    135560fe1d7e   2 hours ago    489MB 
muhammadhashim24/react-app      2         135560fe1d7e   2 hours ago    489MB 
muhammadhashim24/react-app      1         863701cc7202   2 hours ago    489MB 
ubuntu                          latest    35a88802559d   2 months ago   78.1MB
```

Here, you can see several images, including our `react-app` image tagged as `2`.

## 💾 Saving a Docker Image

Docker allows you to **save** an image to a file using the `docker image save` command. This command creates a `.tar` archive containing the image data.

### 📖 Help Command

You can check the help for the save command:

```bash
docker image save --help
```

**Output:**

```text
Usage:  docker image save [OPTIONS] IMAGE [IMAGE...]

Save one or more images to a tar archive (streamed to STDOUT by default)

Aliases:
  docker image save, docker save

Options:
  -o, --output string   Write to a file, instead of STDOUT
```

### 🛠️ Saving the `react-app:2` Image

Let’s save the image `muhammadhashim24/react-app:2` to a file named `react-app.tar`:

```bash
docker image save -o react-app.tar muhammadhashim24/react-app:2
```

This command creates a file named `react-app.tar` in your current directory.

## 🗑️ Verifying Image Deletion

To simulate the process of transferring an image, let’s delete the image from Docker:

```bash
docker images
```

**Output:**

```bash
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
react-app    latest    135560fe1d7e   2 hours ago    489MB
ubuntu       latest    35a88802559d   2 months ago   78.1MB
```

Now, only the `latest` tag of the `react-app` image and the `ubuntu` image remain.

## 🔄 Loading a Docker Image

After saving the image to a file, you can **load** it back into Docker using the `docker image load` command.

### 📖 Help Command

Check the help for the load command:

```bash
docker image load --help
```

**Output:**

```text
Usage:  docker image load [OPTIONS]

Load an image from a tar archive or STDIN

Aliases:
  docker image load, docker load

Options:
  -i, --input string   Read from tar archive file, instead of STDIN
  -q, --quiet          Suppress the load output
```

### 🛠️ Loading the `react-app:2` Image

Let’s load the image from the `react-app.tar` file:

```bash
docker image load -i react-app.tar
```

**Output:**

```text
Loaded image: muhammadhashim24/react-app:2
```

Docker successfully loads the image back into your local registry.

## 🔍 Verifying the Loaded Image

Finally, let’s list the images to verify that the `react-app:2` image has been loaded back:

```bash
docker images
```

**Output:**

```bash
REPOSITORY                   TAG       IMAGE ID       CREATED        SIZE  
react-app                    latest    135560fe1d7e   2 hours ago    489MB 
muhammadhashim24/react-app   2         135560fe1d7e   2 hours ago    489MB 
ubuntu                       latest    35a88802559d   2 months ago   78.1MB
```

The `react-app:2` image has been successfully reloaded, along with its tags and image ID.

---

## 🎯 Summary

1. **Save an Image**: Use the `docker image save` command to save a Docker image to a `.tar` archive.
2. **Delete the Image**: Optionally delete the image from Docker to simulate transferring the image.
3. **Load an Image**: Use the `docker image load` command to load the image back from the `.tar` archive.
4. **Verify**: Check that the image has been successfully loaded.

This process is essential for sharing images in environments without direct access to a Docker registry, ensuring that you can easily transfer images between systems or keep backups of important builds.