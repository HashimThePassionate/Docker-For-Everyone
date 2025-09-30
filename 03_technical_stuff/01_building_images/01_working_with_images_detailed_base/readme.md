# 🐳 **Docker Images – The TLDR**

> **Quick Note:** 📝 Before we start, please know that all of these terms mean the same thing and we will use them interchangeably:
> * Image
> * Docker Image
> * Container Image
> * OCI Image

---

## What is an Image? 🖼️

An **image** is a **read-only package** that contains everything you need to run an application. Think of it as a complete bundle! 📦

This bundle includes:
* Application code 🖥️
* Dependencies (like libraries and frameworks) 🔗
* A minimal set of operating system (OS) constructs ⚙️
* Metadata (information about the image) ℹ️

A cool feature is that you can start **multiple containers** from just a **single image**.

---

## Analogies to Understand Images 💡

If you're trying to wrap your head around what an image is, these comparisons might help:

* **For those familiar with VMware:** 💻
    An image is a bit like a **VM template**. A VM template is like a stopped virtual machine, and similarly, an image is like a stopped container.

* **For Developers:** 🧑‍💻
    Images are similar to **classes** in object-oriented programming. You can create one or more **objects** from a single class. In the same way, you can create one or more **containers** from a single image.

---

## How to Get an Image? 📥

The easiest way to get an image is to **pull** (download) one from a **registry**.

The most common registry is **[Docker Hub](https://hub.docker.com)**. When you pull an image from Docker Hub, it gets downloaded to your local machine. From there, Docker can use it to start one or more containers.

While Docker Hub is the most popular, other registries exist, and Docker works perfectly with all of them.

---

## The Structure of an Image: Layers 겹

Docker creates images by stacking independent **layers** on top of each other and then representing them as a single, unified object.

For example, an image might be built like this:
* **Layer 1:** Has the basic OS components.
* **Layer 2:** Contains application dependencies.
* **Layer 3:** Holds the application itself.

Docker takes these layers, stacks them up, and makes them look like a single, seamless system.

---

## How Big Are Images? 📏

Images are usually quite **small**, which is great for storage and speed!

* The official **NGINX** image is around **80MB**.
* The official **Redis** image is around **40MB**.

However, it's good to know that **Windows images can be quite huge** in comparison.

That’s the elevator pitch for Docker Images! 🚀

---

# 🚀 **Introduction to Images**

We've already mentioned that **images are like stopped containers**. You can even stop an active container and create a brand-new image directly from it.

With this idea in mind, it's helpful to think of them this way:
* **Images** are **build-time** constructs 🏗️.
* **Containers** are **run-time** constructs 🏃.

---

## Visualizing the Process: Figure 6.1 📊

<div align="center">
  <img src="./images/01.svg"/>
</div>

The figure above shows the relationship between building an image and running containers.

* **On the left**, we see the **Image (Build)**. It is shown as a stack of layers, representing the build process.
* **On the right**, we see the **Container(s) (Run)**. The arrows show that a single image can be used to start multiple, separate containers.

This diagram perfectly illustrates the build-time and run-time nature of images and containers and highlights that many containers can be launched from one image.

---

## From Image to Container ➡️

The `docker run` command is the most common way to start a container from an image.

Once a container is running, the image and the container become bound together. This creates an important dependency:

> 🔐 You **cannot delete an image** until you stop and delete the container that is using it. If multiple containers are using the same image, you can only delete that image after you have deleted **all** of the containers using it.

---

## The Philosophy of Lean Containers 🍃

Containers are designed to run a **single application** or microservice. Because of this, they should only contain the essentials:
* Application code
* Required dependencies

You should **not** include non-essential items like build tools or troubleshooting utilities in your images.

---

## What are Slim Images? ✨

For example, the official **Alpine Linux** image is currently about **3MB**. This is incredibly small!

The reason it's so tiny is that it doesn’t come bundled with things you might not need, like:
* Six different shells
* Three different package managers
* A bunch of tools you "might" need once every ten years

In fact, it’s becoming more and more common for images to ship without a shell or a package manager at all. The rule is simple: if the application doesn’t need it at run-time, the image doesn’t include it. We call these **slim images**.

---

## Why Images Stay Small: No OS Kernel! 🧠

Another factor that keeps images small is the **lack of an OS kernel**.

This is because containers don't need their own kernel; they simply use the kernel of the host machine they are running on. The only OS-related parts in most images are filesystem objects. You will sometimes hear people say that images contain **"just enough OS"** to function.

---

## A Note on Windows Images 🪟

Unfortunately, **Windows images can be huge**. For example, some Windows-based images can be gigabytes in size and can take a long time to push (upload) and pull (download).


---

# 📥 **Pulling Images**

A brand new, clean Docker installation starts with an **empty local repository**.

-----

## What is the Local Repository? 📂

The **local repository** is simply a special area on your local machine where Docker stores images for easier and faster access. You might also hear it called the **image cache**.

  * **On Linux**, this is usually located at `/var/lib/docker/`.
  * **With Docker Desktop**, it's stored inside the Docker Virtual Machine (VM).

You can check what's inside your local repository by running the following command. The example below shows three images related to Docker Desktop extensions. Your list will likely be different and might even be empty.

### Code Snippet: Listing Local Images

```bash
docker images
```

### Explanation of the Command

This command lists all the Docker images currently stored on your local machine.

### Example Output

```
REPOSITORY    TAG      IMAGE ID      CREATED        SIZE
ubuntu        24.04    7c06e91f61fa  2 months ago   117MB
```

  * **REPOSITORY**: The name of the image.
  * **TAG**: The specific version of the image (e.g., `24.04`, `latest`).
  * **IMAGE ID**: A unique identifier for the image.
  * **CREATED**: When the image was built.
  * **SIZE**: The amount of disk space the image uses.

-----

## How to Pull an Image ☁️

The process of getting images from a registry (like Docker Hub) and downloading them to your local repository is called **pulling**.

Let's pull the official `redis` image and then check that it exists in our local repository.

> **Linux User Note** 🐧:
> If you are on Linux and haven't added your user account to the local `docker` Unix group, you may need to add `sudo` to the beginning of all the following Docker commands.

### Code Snippet: Pulling the Redis Image

```bash
docker pull redis
```

### Explanation of the Command

This command tells Docker to find an image named `redis` in the default registry (Docker Hub) and download it to your local machine.

### Command Output Explained

```
Using default tag: latest
latest: Pulling from library/redis
d107e437f729: Pull complete
cf596724f63e: Pull complete
a3f725691cac: Pull complete
d7e6e9e45ecf: Pull complete
bd022da7d981: Pull complete
4f4fb700ef54: Pull complete
349073970fc7: Pull complete
Digest: sha256:b0341dc2e0ce47beb7fcef80089b4d469b10ae94fbbea50072b878d7c88de487
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
```

  * `Using default tag: latest`: Since we didn't specify a version, Docker automatically chose the `latest` tag.
  * `latest: Pulling from library/redis`: Docker confirms it's pulling the `latest` tag from the official `redis` library.
  * `d107e437f729: Pull complete`: Each of these lines represents a different layer of the image being downloaded. Images are made of multiple layers, and Docker pulls each one.
  * `Digest: sha256:...`: This is a unique signature for the image content, ensuring what you downloaded is authentic and hasn't been tampered with.
  * `Status: Downloaded newer image...`: A success message confirming the download is complete.
  * `docker.io/library/redis:latest`: This is the full, official name of the image that was pulled.

### Code Snippet: Verifying the Pull

Now, let's run `docker images` again to see the result.

```bash
docker images
```

### New Output

```
REPOSITORY    TAG      IMAGE ID      CREATED       SIZE
redis         latest   b0341dc2e0ce  6 weeks ago   200MB
ubuntu        24.04    7c06e91f61fa  2 months ago  117MB
```

As you can see, the `redis` image with the `latest` tag is now in our local repository, ready to be used\!

-----

## Docker's Default Assumptions 🤔

When we ran `docker pull redis`, Docker was "opinionated" and made two assumptions because we didn't provide more specific details:

1.  It assumed you wanted to pull the image tagged as **`latest`**.
2.  It assumed you wanted to pull the image from **Docker Hub**.

You can override both of these defaults, but Docker will always use them if you don't specify otherwise.


---