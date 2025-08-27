<h1 align="center">
  <img src="./assests/docker.svg" alt="Docker Logo" width="550"/>
</h1>

<p align="center"><i>Your complete guide to mastering Linux using Docker containers</i></p>

<p align="center">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" />
  <img src="https://img.shields.io/github/repo-size/HashimThePassionate/Docker-For-Everyone" />
  <img src="https://img.shields.io/github/stars/HashimThePassionate/Docker-For-Everyone?style=social" />
  <img src="https://img.shields.io/github/last-commit/HashimThePassionate/Docker-For-Everyone" />
  <img src="https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-%23009688?logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/React-%2361DAFB?logo=react&logoColor=black" />
  <img src="https://img.shields.io/badge/Express-%23000000?logo=express&logoColor=white" />
</p>

---

## 🧭 What will you learn here?

By following this repo step by step, you will master:

- **Container Basics** 🐳 — What containers are, how they differ from VMs, and why they matter.  
- **Linux Essentials for Docker** 🐧 — Core Linux commands and concepts you’ll need for containers.  
- **Docker Engine & Internals** ⚙️ — runc, containerd, shims, namespaces, cgroups.  
- **Working with Images** 🖼️ — Build, tag, push, scan, and optimize images with layers & BuildKit.  
- **Working with Containers** 📦 — Run, exec, logs, inspect, healthcheck, restart policies.  
- **Containerizing Applications** 🛠️ — Containerize Python, Node, React, and FastAPI apps.  
- **Multi-Container Applications** 🧩 — Compose full stacks (frontend + backend + database).  
- **Deployments** 🚀 — From development to production with Compose and best practices.  
- **Docker Networking** 🌐 — Bridge, host, overlay networks, DNS resolution, service discovery.  
- **Volumes & Data Persistence** 💾 — Use named volumes and bind mounts.  
- **Security** 🔐 — Namespaces, capabilities, seccomp, secrets management, image signing.  
- **Swarm Mode & Orchestration** 🕸️ — Deploy services in a Swarm cluster.  
- **Wasm + Docker** ⚡ — Explore WebAssembly in containers.  
- **AI/ML Workloads** 🤖 — Run ML models with Docker Model Runner & Compose.  
- **Real-World Projects** 🧪  
  - React (SPA)  
  - React + Express (Full-stack)  
  - FastAPI (Python backend)  

---

## ⚡ Quickstart

1. **Install** Docker Desktop or Docker Engine.  
2. **Clone the repo**  

```bash
   git clone https://github.com/HashimThePassionate/Docker-For-Everyone
   cd Docker-For-Everyone
```

3. **Pick a module/project** folder and follow its README.
4. **Run it**

```bash
   docker compose up --build
```

---

## 📂 Repository Structure

Navigate each module by clicking the folder name 👇

| 📁 Folder | 📖 Description |
|-----------|----------------|
| [Getting Started](./00_the_big_picture_of_containers/) | 🐳 Docker Hello-World and Ops basics |
| [Container Poetry](./01_containerizing_poetry) | 📦 Containerizing with Poetry (Python packaging) |
| [Linux Command Line](./02_linux_command_line) | 🐧 Linux command line essentials |
| [Building Images](./03_building_images) | 🖼️ Docker Commands Cheat Sheet & Image building |
| [Working With Containers](./04_working_with_containers) | 📦 Container lifecycle, exec, inspect, logs |
| [Running Multicontainers Applications](./05_running_multicontainers_applications) | 🧩 Running multi-container applications |
| [Deploying the Application](./06_deploying_the_application) | 🚀 Deploying the application with Docker Compose |

---

## 🛡️ Best Practices You’ll Pick Up

* Multi-stage builds for smaller images
* Running as non-root
* Using healthchecks & restart policies
* Externalizing configs & secrets
* One responsibility per container

---

## 🗺️ Roadmap

* [ ] AI/ML Model Runner labs
* [ ] WebAssembly demo
* [ ] Overlay networking (multi-node demo)
* [ ] Security labs (capabilities, seccomp)
* [ ] Postgres + pgAdmin stack

---

## 🤝 Contributing

Fork → Create branch → PR.
Keep examples small, self-contained, with **Compose files + README**.

---

## 📄 License

This project is licensed under the **MIT License** — see [`LICENSE`](./LICENSE).

---

## 💬 Feedback & Stars

If this repo helps you, please **⭐ star it** and open an **issue** for suggestions.
Happy shipping with Docker! 🐳