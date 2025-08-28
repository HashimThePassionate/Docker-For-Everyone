# 🐳 **Docker Overview**

Docker is at the heart of the modern **container ecosystem**.
However, the word **Docker** can mean two different things:

1. **The Docker Platform** → A collection of technologies for creating, managing, and orchestrating containers.
2. **Docker, Inc.** → The company that created and continues to develop Docker.

---

## 🏢 Docker, Inc.

* 📍 Based in **Palo Alto, USA**
* 👨‍💻 Founded by **Solomon Hykes** (French-born American developer & entrepreneur)
* 🛠️ Originally started as a **Platform as a Service (PaaS)** company called **dotCloud**
* ⚙️ dotCloud used containers internally and built a tool to manage them → they named it **Docker**
* ⚓ The term **Docker** comes from the British slang for a **dock worker**, someone who loads/unloads cargo from ships.

👉 In **2013**, dotCloud:

* Dropped the struggling PaaS business.
* Rebranded as **Docker, Inc.**
* Focused fully on **Docker and containers**.

---

## ⚙️ The Docker Platform

The Docker platform simplifies **building, sharing, and running containers**.

It has **two main parts**:

1. **🖥️ CLI (Client)**

   * The `docker` command-line tool.
   * Converts user commands into **API requests**.
   * Sends them to the Docker Engine.

2. **⚡ Engine (Server)**

   * Runs and manages containers.
   * Receives API calls from the CLI.
   * Handles container lifecycle and orchestration.

---

## 🖼️ Docker Architecture

### 🔹 High-level view (Client ↔ Engine)

<div align="center">
  <img src="./images/01.png" width="500"/>

*Figure 2.1 – Docker client and engine.*
</div>

* **Client (CLI):** User interacts using `docker` commands.
* **Engine (Server):** Executes commands and manages containers.
* Communication happens via **API calls**.
* Client and Engine can be on the **same host** or connected **over a network**.

---

### 🔹 Detailed view (CLI ↔ Daemon ↔ Components)

<div align="center">
  <img src="./images/02.png" alt="" width="500px"/>

*Figure 2.2 – Docker CLI and daemon hiding complexity.*
</div>

* **Client (CLI):**

  * You run commands like:

    ```bash
    docker build
    docker push
    docker run
    docker scout
    ```
  * These get converted into **API requests**.

* **Daemon (Server side):**

  * Handles container operations behind the scenes.
  * Connects with specialized subcomponents.

* **Specialized Components in Engine:**

  * 🏗️ **BuildKit** → Builds container images efficiently.
  * 📦 **containerd** → Core container runtime.
  * ⚙️ **runc** → Low-level runtime to spawn containers.
  * 🌐 **LibNetwork** → Networking for containers.
  * 🔐 **Notary** → Image signing & security.
  * 🐙 **Compose** → Multi-container applications.
  * 🔄 **SwarmKit** → Native container orchestration.
  * 🗂️ **Registries** → Store & distribute images.
  * 🕵️ **Scout** → Security scanning & monitoring.

👉 **Key point:**
The **CLI hides the complexity** of all these tools. You type simple commands → CLI converts them to API calls → **Daemon + Engine handle the rest**.

---

