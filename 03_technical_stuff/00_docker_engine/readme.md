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


# 🐳 **Docker Engine Components & Responsibilities**

## 📌 Breaking up the Monolithic Docker Daemon

Originally, the **Docker Engine** was a **monolithic daemon**—meaning almost all functionality was bundled together inside a single, large process.

However, this design caused several issues:

1. ⚡ **Performance Degradation** – The daemon got slower over time.
2. 🌍 **Ecosystem Mismatch** – It wasn’t aligned with what the container ecosystem wanted.
3. 🛠️ **Lack of Innovation** – Innovating and extending monolithic software is hard.

👉 To solve these challenges, the Docker team began **refactoring** the Engine into smaller, specialized tools.

## 🔨 Refactoring the Engine

* The monolithic daemon was **broken apart** into **modular components**.
* Each feature became its own **small specialized tool**.
* **Platform builders** could now reuse these tools to build other container platforms.

### ✅ Key Changes

* **Image building** and **container execution** code was removed from the daemon.
* These responsibilities were re-implemented in separate tools:

  * **containerd** → Lifecycle management (start, stop, delete containers) & image management
  * **runc** → Low-level runtime, interfaces directly with the Linux kernel
* These tools are now widely used in projects such as:

  * Docker
  * Kubernetes
  * Firecracker
  * AWS Fargate

📢 As of **Docker Desktop 4.27.0**, image management has also been removed from the daemon and is now handled by **containerd**.

---

## 🖼️ Docker Engine Components (Figure 5.2)

<div align="center">
  <img src="./images/02.svg"/>
</div>

### 🔑 Components Explained

* **`$ docker CLI`**

  * Command Line Interface for Docker.
  * Used to interact with the Docker daemon by sending commands.

* **Daemon (`dockerd`)**

  * Exposes APIs for Docker.
  * Handles requests coming from the CLI or REST API.

* **containerd**

  * Specialized tool for **container lifecycle management**:

    * Start
    * Stop
    * Delete containers
  * Provides an abstraction layer for container runtimes.

* **Shims**

  * Lightweight processes acting as middlemen.
  * Keep the container’s standard input/output open even if `containerd` or `dockerd` crashes.
  * Enable support for **pluggable runtimes**.

* **runc**

  * Low-level container runtime.
  * Interfaces with the Linux kernel (via namespaces, cgroups, etc.).
  * Actually **spawns containers**.

* **Running Containers**

  * Final result: isolated containers created and managed using all these components.

---

## 🧩 Why This Matters?

* ✅ **Flexibility** – Each part can be upgraded, replaced, or reused independently.
* ✅ **Reusability** – Tools like `containerd` and `runc` are now industry standards beyond Docker.
* ✅ **Performance & Reliability** – Smaller, focused tools reduce the risk of one bug crashing the entire system.

---

