# 📦 **Containers from 30,000 Feet**

Containers have taken over the world! 🌍
In this section, you'll learn **why we have containers, what they do for us, and where we can use them.**

---

<details>
<summary>📋 <strong>Table of Contents</strong></summary>

| 🔢 | 📖 **Section** | 📝 **Description** |
|-----|----------------|-------------------|
| 1️⃣ | [🕰️ The Bad Old Days](#️-the-bad-old-days) | Understanding the limitations of traditional server architecture |
| 2️⃣ | [💡 Hello VMware!](#-hello-vmware) | How Virtual Machines revolutionized server utilization |
| 3️⃣ | [🐛 VMWarts (Drawbacks of VMs)](#-vmwarts-drawbacks-of-vms) | The problems and limitations of Virtual Machines |
| 4️⃣ | [👋 Hello Containers!](#-hello-containers) | Introduction to container technology and its advantages |
| 5️⃣ | [🐧 Linux Containers](#-linux-containers) | The origins and core technologies of modern containers |
| 6️⃣ | [🐳 Docker to the Rescue](#-docker-to-the-rescue) | How Docker made containers accessible to everyone |
| 7️⃣ | [🐳 Docker and Windows](#-docker-and-windows) | Container support on Windows platforms |
| 8️⃣ | [🪟 Windows Containers](#-windows-containers) | Running Windows applications in containers |
| 9️⃣ | [🐧 Linux Containers on Windows](#-linux-containers-on-windows) | Using WSL 2 to run Linux containers on Windows |
| � | [⚖️ Windows vs. Linux Containers](#️-windows-containers-vs-linux-containers) | Understanding kernel requirements and compatibility |
| 1️⃣1️⃣ | [🧩 What about Wasm?](#-what-about-wasm-webassembly) | WebAssembly as an emerging alternative to containers |
| 1️⃣2️⃣ | [🤖 Docker and AI](#-docker-and-ai) | Container challenges and solutions for AI workloads |

</details>

---


## 🕰️ The Bad Old Days

Applications are the powerhouse ⚡ of every modern business.
When applications break, businesses break.

* Most applications ran on servers.
* In the past, we were limited to **running one application per server**.

👉 This created a **big problem**:

* Every time a business needed a new application → it had to **buy a new server**.
* We weren’t very good at predicting the performance requirements of new apps.
* To avoid underpowered servers, companies bought **bigger, faster, more expensive servers** than necessary.

⚠️ Result:

* Racks of overpowered servers 🖥️🖥️🖥️ operating at just **5–10% of their capacity**.
* Massive **waste of company capital 💸** and **environmental resources 🌱**.

---

## 💡 Hello VMware!

Then came **VMware, Inc.** with a gift 🎁 → **the Virtual Machine (VM)**.

✨ What VMs did:

* Allowed **multiple business applications** to run safely on a single server.
* Businesses could now **use spare capacity of existing servers**.

🌟 This was a **game-changer**, creating a **golden age of maximizing server resources**.

---

## 🐛 VMWarts (Drawbacks of VMs)

But… VMs weren’t perfect. ❌

Each VM needs its **own dedicated operating system (OS)**. This caused several problems:

* 🖥️ Every OS consumed **CPU, RAM, and resources** that should’ve gone to applications.
* 🔧 Every VM + OS required **patching and monitoring**.
* 🐢 VMs were **slow to boot**.
* 📦 VMs were **not portable**.

---

## 👋 Hello Containers!

While most of us were enjoying VMs, **Google and other web scalers had already moved on → Containers.**

🔑 Key difference:

* Containers **share the host’s OS** instead of needing a full OS per application.

✅ Advantages of containers over VMs:

* ⚡ **Faster**
* 📦 **More portable**
* 🖥️ **More efficient** (e.g., a host that runs 10 VMs can often run **50 containers!**)

---

## 🐧 Linux Containers

Modern containers were born in the **Linux world** 🐧.
They’re the product of **years of contributions** from many developers and organizations.

🔹 Example:

* **Google** contributed many container-related technologies to the Linux kernel.

### 🔧 Core Technologies Behind Containers:

* **Namespaces**
* **Control Groups (cgroups)**
* **Capabilities**

💡 Problem: Containers were still **complicated to use**.

---

## 🐳 Docker to the Rescue

Then came **Docker** — the technology that **made containers accessible to the masses**.

> ⚠️ Note: Many container-like technologies existed before Docker.
> But none changed the world 🌍 the way **Docker** has.

---

# 🐳 **Docker and Windows**

Microsoft worked hard to bring **Docker** and container technologies to the **Windows platform**.

At present, both **Windows desktop** and **Windows Server** platforms support:

* 🪟 **Windows containers**
* 🐧 **Linux containers**

---

## 🪟 Windows Containers

* Run **Windows applications**.
* Require a host system with a **Windows kernel**.
* Supported natively on:

  * Windows 10
  * Windows 11
  * All modern versions of **Windows Server**

---

## 🐧 Linux Containers on Windows

* Windows systems can also run **Linux containers** using **WSL 2** (Windows Subsystem for Linux).
* This makes Windows 10 and Windows 11 great platforms for **developing and testing both Windows and Linux containers**.

⚡ **Note:**
Despite all the progress on Windows containers, **most containers are Linux-based** because:

* They are **smaller**.
* They are **faster**.
* The **Linux tooling ecosystem** is richer and more mature.

---

## ⚖️ Windows Containers vs. Linux Containers

It’s important to remember:

* Containers **share the kernel** of the host they’re running on.
* 🔹 A **Windows containerized app** requires a **Windows kernel**.
* 🔹 A **Linux containerized app** requires a **Linux kernel**.

👉 With WSL 2, you can run **Linux containers on Windows**, bridging both worlds!

---

# 🧩 What about Wasm (WebAssembly)?

**Wasm (WebAssembly)** is a **modern binary instruction set** that allows apps to be:

* ⚡ **Smaller**
* 🚀 **Faster**
* 🔒 **More secure**
* 🌍 **Highly portable**

### How it works:

1. Write your app in your favorite programming language.
2. Compile it into a **Wasm binary**.
3. Run it anywhere a **Wasm runtime** is available.

### Limitations of Wasm:

* Standards are still evolving.
* Many **limitations** exist compared to containers.

👉 Result: **Containers remain the dominant model** for cloud-native applications (for now).

But… the **ecosystem is evolving**:

* Docker and other container tools are already adapting to support **Wasm apps**.
* In the future, expect to see **VMs, containers, and Wasm apps running side-by-side** in most cloud environments.

---

# 🤖 Docker and AI

With the rise of **AI applications**, Docker continues to be one of the **most desired and most used developer tools** (as per the Stack Overflow Annual Developer Survey).

### The Challenge 🚧

Running **AI workloads inside containers** is difficult because:

* GPUs and other AI acceleration hardware each have their **own drivers and SDKs**.
* Standardizing them across containers is too complex.

### The Solution 💡

Docker introduced **Docker Model Runner**:

* Lets developers run **local LLMs (Large Language Models)** **outside of containers**.
* This gives direct access to the host’s hardware (GPUs, accelerators).
* Makes it easier to run AI workloads efficiently without container overhead.

---
