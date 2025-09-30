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