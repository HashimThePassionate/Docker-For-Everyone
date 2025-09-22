# 🐳 **Docker Engine**

Docker Engine is the **server-side component of Docker** that runs and manages containers.
If you’ve ever worked with **VMware**, you can think of Docker Engine as similar to **ESXi**.

It is **modular**, built from many small specialized components, originating from projects like:

* **OCI** (Open Container Initiative)
* **CNCF** (Cloud Native Computing Foundation)
* **Moby Project**

---

## 🚗 Analogy: Docker Engine = Car Engine

Just like a car engine is made of many specialized parts working together,
the Docker Engine is made of specialized tools that together create and run containers.

* **Car Engine parts** → intake manifolds, throttle bodies, cylinders, pistons, spark plugs, exhaust manifolds, etc.
* **Docker Engine parts** → API, image builder, high-level runtime, low-level runtime, shims, etc.

---

## 🖼️ Visual Representation

<div align="center">
  <img src="./images/01.svg" alt=""/>
</div>

### Flow:

1. User interacts via **`docker CLI`**
2. CLI talks to the **Docker API**
3. API requests are handled by the **daemon**
4. Daemon delegates to **containerd**
5. containerd talks to **runc** (low-level runtime)
6. runc creates and manages **running containers**
7. Plugins & other components extend functionality

---

## ⚙️ Components of Docker Engine

### 🔹 1. Docker Daemon

* Sometimes simply called **"the daemon"**
* Originally a **monolithic binary** containing everything:

  * API
  * Image builders
  * Container execution
  * Volumes
  * Networking
* Acts as the **brain** of Docker Engine.

---

### 🔹 2. LXC (Initial Runtime)

* In early Docker versions, **LXC** was used for:

  * Interfacing with the **Linux kernel**
  * Constructing **namespaces** and **cgroups**
  * Building & starting containers

⚠️ **Problem:**

* LXC is **Linux-specific** (no multi-platform support).
* Docker was evolving fast, but LXC’s pace didn’t align.

---

### 🔹 3. Replacing LXC with libcontainer

* To solve issues, Docker built its own tool: **libcontainer**.
* **Goal:** Be **platform-agnostic** while providing direct access to container building blocks in the kernel.
* **Outcome:**

  * libcontainer replaced LXC **a long time ago**.
  * Enabled Docker to evolve independently.

---

### 🔹 4. containerd

* A **high-level runtime** that manages container lifecycle:

  * Start
  * Stop
  * Delete
  * Supervising processes
* Provides APIs for managing containers.

---

### 🔹 5. runc

* A **low-level runtime** (from the OCI project).
* Handles the **actual execution** of containers by:

  * Creating namespaces
  * Setting up cgroups
  * Launching the container process
* Lightweight and minimal.

---
