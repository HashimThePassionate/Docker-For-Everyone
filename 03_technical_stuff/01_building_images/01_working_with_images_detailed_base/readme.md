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