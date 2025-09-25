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

# 📦 **The Influence of the Open Container Initiative (OCI)**

## 🌍 What is OCI?

The **Open Container Initiative (OCI)** was founded to standardize the way containers work across different platforms and tools. At the same time Docker, Inc. was refactoring the Engine, the OCI began defining **container-related standards**.

### 📝 Key Specifications

1. **Image Specification (image-spec)**

   * Defines how container images should be structured and packaged.

2. **Runtime Specification (runtime-spec)**

   * Defines how container runtimes should execute containers.

3. **Distribution Specification (distribution-spec)**

   * Defines how container images should be **distributed** via registries.

📌 All specifications reached **version 1.0 in July 2017** and continue to evolve slowly but steadily:

* `runtime-spec` → **v1.2.0**
* `image-spec` → **v1.1.0**
* `distribution-spec` → **v1.1.0**

👉 Stability is the main goal, since countless projects depend on these specs.

---

## 🐳 Docker’s Role in OCI

* Docker, Inc. was a **founding member** of the OCI.
* Contributed heavily to defining the **original specifications**.
* Continues to contribute code and guide the future of OCI standards.
* Since **2016**, all Docker versions implement OCI specifications:

  * **runc** → Creates **OCI-compliant containers** (runtime-spec).
  * **BuildKit** → Builds **OCI-compliant images** (image-spec).
  * **Docker Hub** → Functions as an **OCI-compliant registry** (distribution-spec).

---

## ⚙️ runc: The Low-Level Runtime

* **Name**: always lowercase `runc` (pronounced “run see”).
* **Role**: Reference implementation of the OCI **runtime-spec**.
* **Origin**: Docker helped define the spec and contributed the initial `runc` code.

### 🔑 Features of runc

* Lightweight **CLI wrapper** around `libcontainer`.
* Manages **OCI-compliant containers** directly.
* Interfaces with the Linux **kernel** (via namespaces, cgroups).
* Very **low-level tool** (no extras like image building or networking).

📢 In practice:

* **Docker** uses `runc` internally as its **low-level runtime**.
* You get the **OCI compliance** of runc with Docker’s **feature-rich experience**.
* **Kubernetes** also uses `runc` by default (paired with `containerd`).

👉 Latest releases: [runc GitHub Releases](https://github.com/opencontainers/runc/releases)

---

## ⚙️ containerd: The High-Level Runtime

* **Name**: always lowercase `containerd` (pronounced “container dee”).
* **Role**: Reference implementation of the OCI **high-level runtime**.
* **Origin**: Created by Docker, Inc., then donated to the **Cloud Native Computing Foundation (CNCF)**.

### 🔑 Features of containerd

* Manages **container lifecycle events**:

  * Start
  * Stop
  * Delete containers
* Uses **shims** to plug in different low-level runtimes (commonly `runc`).
* Expanded functionality:

  * **Image management** (push, pull).
  * **Networking**.
  * **Volumes**.
* Modular design → Projects like **Kubernetes** can include only what they need.

📢 Status: **Graduated CNCF project** → stable & production-ready.
👉 Latest releases: [containerd GitHub Releases](https://github.com/containerd/containerd/releases)

---

## 🪜 runc vs. containerd

| Component      | Type               | Responsibilities                                                               | Layer                   |
| -------------- | ------------------ | ------------------------------------------------------------------------------ | ----------------------- |
| **runc**       | Low-level runtime  | Executes container lifecycle by talking to kernel (namespaces, cgroups).       | **OCI layer**           |
| **containerd** | High-level runtime | Manages lifecycle events, images, networks, volumes. Needs runc for execution. | **Orchestration layer** |

---

## ⚡ In Action: How Docker Uses Them Together

1. **Docker CLI** → User runs a command (`docker run hello-world`).
2. **dockerd** → Docker daemon parses request.
3. **containerd** → Handles lifecycle management (start container).
4. **Shim** → Keeps I/O open, allows runtime replacement.
5. **runc** → Talks to Linux kernel to actually **spawn container**.
6. ✅ Running container appears!

---

## 🧩 Why OCI Matters

* Ensures **cross-platform compatibility**.
* Promotes **reusability** (same runtime across Docker, Kubernetes, Firecracker, etc.).
* Provides **stability & trust** for container-based systems.

---


# 🐳 **Starting a New Docker Container**

This section explains how Docker starts a new container, step by step, with the **Docker CLI → Daemon → containerd → shim → runc** pipeline.

The figure below shows the workflow:

<div align="center">
  <img src="./images/03.svg"/>

*Figure: Starting a new container process*
</div>


---

## 🔄 Workflow of Container Creation

1. **Docker CLI (`docker run`)**

   * You issue a command like `docker run ...`.
   * The Docker CLI converts this into an **API request** and sends it to the **Docker daemon (dockerd)**.

2. **Docker Daemon (`dockerd`)**

   * Receives the API request.
   * Interprets it as a container creation request.
   * Communicates with **containerd** via a **CRUD-style API over gRPC**.

3. **containerd**

   * Responsible for container lifecycle management.
   * Converts the Docker image into an **OCI bundle**.
   * Instructs **runc** to create the container.

4. **runc**

   * Talks directly to the **Linux kernel**.
   * Uses kernel features like:

     * **Namespaces** → provide isolation (PID, network, filesystem, etc.).
     * **cgroups** → control CPU, memory, and other resources.
   * Creates the container as a child process.
   * Exits once the container is running.

5. **shim**

   * Takes over as the container’s parent process after `runc` exits.
   * Keeps the container alive.
   * Handles container I/O streams and communicates with `containerd`.

---

## 📝 Example: Start an NGINX Container

Run the following command to create a container called **`ctrl`** from the **nginx** image:

```bash
$ docker run -d --name ctrl nginx
```

### 🔍 Command Breakdown:

* `docker run` → Start a new container.
* `-d` → Detached mode (container runs in the background).
* `--name ctrl` → Assign a custom name to the container (`ctrl`).
* `nginx` → The base image used for the container.

---

## ✅ Checking Running Containers

Use the following command to see running containers:

```bash
$ docker ps
```

### Example Output:

```
CONTAINER ID   IMAGE    COMMAND                  CREATED         STATUS         PORTS    NAMES
9cfb0c9aacb2   nginx    "/docker-entrypoint.…"  9 seconds ago   Up 9 seconds   80/tcp   ctrl
```

**Explanation of Columns:**

* **CONTAINER ID** → Unique ID of the container.
* **IMAGE** → Image used (here: `nginx`).
* **COMMAND** → The process running inside.
* **CREATED** → When the container was started.
* **STATUS** → Current state (e.g., "Up 9 seconds").
* **PORTS** → Exposed ports (`80/tcp`).
* **NAMES** → Container name (`ctrl`).

---

## ⚙️ Daemon Communication Sockets

* On **Linux**: `/var/run/docker.sock`
* On **Windows**: `\\pipe\\docker_engine`

These sockets allow the Docker CLI to communicate with the daemon.

---

## 🔒 Why Use `containerd` and `runc`?

* The **daemon does not directly create containers** anymore.
* Separation of duties:

  * `dockerd` → Handles API & orchestration.
  * `containerd` → Lifecycle management.
  * `runc` → Interfaces with the kernel to create containers.
* This allows **daemonless containers**:

  * You can **restart, stop, or update the Docker daemon** without affecting running containers.

---

## 🗑️ Removing the Container

If you created the NGINX container earlier, remove it with:

```bash
$ docker rm ctrl -f
```

### 🔍 Command Breakdown:

* `docker rm` → Remove a container.
* `ctrl` → The name of the container.
* `-f` → Force removal (even if running).

---

## ⚙️ Docker Shim Explained

A **shim** is a popular software engineering pattern used in Docker Engine to sit between **containerd** and the **OCI runtime (runc)**.

It plays a critical role in enabling **daemonless containers**, improving efficiency, and allowing flexibility in runtime selection.

---

## 🧩 What is a Shim?

A **shim** is a lightweight process that:

* Acts as an **intermediary** between `containerd` and `runc`.
* Stays alive after `runc` exits.
* Becomes the **container’s parent process**.
* Manages I/O streams (STDIN, STDOUT).
* Reports container status to `containerd`.

---

## 🚀 Benefits of Shim in Docker

1. **Daemonless Containers**

   * You can stop, restart, or update the Docker daemon **without affecting running containers**.
   * The shim process ensures containers continue running even if the daemon disappears.

2. **Improved Efficiency**

   * For each new container:

     * `containerd` forks a **shim** and a **runc** process.
     * `runc` exits after the container starts.
     * The shim (lightweight) remains to manage the container.

3. **Pluggable OCI Layer**

   * By using a shim, Docker allows `runc` to be swapped with **other runtimes** (e.g., `crun`, `gVisor`, `Kata Containers`).
   * This enables flexibility in the container ecosystem.

---

## 🐧 Implementation on Linux

On Linux, Docker components are implemented as **separate binaries**:

* **Docker Daemon:** `/usr/bin/dockerd`
* **Containerd:** `/usr/bin/containerd`
* **Shim:** `/usr/bin/containerd-shim-runc-v2`
* **Runtime (runc):** `/usr/bin/runc`

### 🔍 Viewing Processes

You can check these binaries and their processes with:

```bash
$ ps aux | grep containerd
```

👉 Some processes (like shim and runc) only appear when containers are running.
👉 On Docker Desktop (Mac/Windows), these run **inside a VM**, so you won’t see them directly on your host OS.

---

## 🤔 Do We Still Need the Daemon?

* Over time, **most container creation logic has been stripped out of the daemon** and moved to `containerd` + `runc`.
* However, the **daemon still serves the Docker API**, which is crucial for:

  * Handling CLI/API requests (`docker run`, `docker ps`, etc.).
  * Managing high-level orchestration and communication.

So yes, the daemon is still required—but its role is now more about **API handling** rather than container creation.

---
