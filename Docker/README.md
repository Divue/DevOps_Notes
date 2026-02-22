# Docker — Theory Notes

---

# 1. What is a Container?

A container is a standardized unit of software that packages:

- Application code 
- Application dependencies (libraries, runtimes) 
- Minimal system dependencies 

This ensures the application runs consistently across different environments.

More formally:

A container image is a lightweight, standalone, executable package that includes everything needed to run an application — code, runtime, system tools, system libraries, and configurations.

Simplified definition:

Container = Application + Required Libraries + Minimal System Dependencies

---

# 2. Containers vs Virtual Machines

Both containers and virtual machines (VMs) isolate applications, but they differ in architecture and overhead.

## Resource Utilization

- Containers share the host OS kernel.
- VMs include a full operating system and run on a hypervisor.

Result:
Containers are lighter and faster.
VMs are heavier and more resource-intensive.

## Portability

- Containers run anywhere the host OS is compatible.
- VMs require compatible hypervisors.

Containers are generally more portable.

## Security

- VMs provide stronger isolation (separate OS per VM).
- Containers share the host kernel → less isolation than VMs.

## Management

Containers are easier and faster to manage due to their lightweight nature.

---

# 3. Why Are Containers Lightweight?

Containers are lightweight because:

1. They share the host operating system kernel.
2. They do not include a full OS.
3. They only contain minimal components required to run the application.

Comparison example (conceptual):

- Ubuntu container base image ≈ tens of MB
- Ubuntu VM image ≈ multiple GB

Reason:
VMs emulate an entire operating system.
Containers reuse the host kernel and isolate processes using OS-level mechanisms.

---

# 4. Files and Folders in Container Base Images

Typical structure inside a Linux-based container image:

```
/bin   → binary executables (ls, cp, ps)
/sbin  → system binaries (init, shutdown)
/etc   → configuration files
/lib   → shared libraries
/usr   → user utilities, applications
/var   → logs, temporary files, variable data
/root  → root user home directory
```

These are minimal OS-level components required for execution.

---

# 5. What Containers Use from Host OS

Although isolated, containers rely on the host for core functionality.

## Host File System

Using bind mounts, containers can read/write to host files.

## Networking Stack

Containers use the host networking system or virtual networks created by Docker.

## System Calls

Containers rely on the host kernel to execute system calls (CPU, memory, I/O access).

## Namespaces

Linux namespaces provide isolation for:

- Process IDs  
- Filesystem  
- Network  
- Users  

## Control Groups (cgroups)

cgroups limit and control resource usage:

- CPU  
- Memory  
- Disk I/O  

Key point:
Containers use host resources but remain logically isolated from the host and other containers.

---

# 6. Docker

## What is Docker?

Docker is a containerization platform.

Containerization = Concept  
Docker = Implementation of containerization

Docker enables you to:

- Build container images  
- Run containers  
- Push images to registries  
- Pull images from registries  

---

# 7. Docker Architecture

Docker operates using a client-server architecture.

Core components:

## Docker Daemon (dockerd)

- Runs in the background.
- Manages images, containers, networks, volumes.
- Listens for Docker API requests.

If the daemon stops, Docker stops functioning.

## Docker Client (docker)

- CLI tool used by users.
- Sends commands (docker run, docker build, etc.) to the daemon via API.

## Docker Desktop

All-in-one application including:

- Docker daemon 
- Docker CLI 
- Docker Compose 
- Kubernetes (optional integration) 
- Credential helpers 

Used primarily on Windows, macOS, Linux desktop environments.

## Docker Registries

Registry = Storage for Docker images.

- Public registry (e.g., Docker Hub)
- Private registry

Docker pull → downloads image 
Docker push → uploads image 

---

# 8. Docker Objects

When using Docker, you work with:

- Images 
- Containers 
- Networks 
- Volumes 
- Plugins

---

# 9. Dockerfile

A Dockerfile is a text file containing instructions to build a Docker image.

Each instruction creates a new layer.

Layering enables:

- Efficient rebuilds 
- Caching 
- Smaller incremental updates 

If only one layer changes, only that layer is rebuilt.

This contributes to Docker’s efficiency and speed.

---

# 10. Images

An image is:

- A read-only template
- Used to create containers
- Built in layers

Images are often built on top of base images.

Example concept:

Base Image → Ubuntu 
Add layer → Install web server 
Add layer → Copy application code 
Add layer → Set configuration 

Each step creates a new image layer.

Images are:

- Lightweight 
- Layered 
- Reusable 

---

# 11. Docker Lifecycle

There are three core lifecycle commands:

## 1. Build

Creates an image from a Dockerfile.

Concept:
Source code + Dockerfile → Image

## 2. Run

Creates and starts a container from an image.

Image → Running Container

## 3. Push

Uploads image to a registry for sharing.

Local Image → Registry

Lifecycle flow:

Dockerfile → Build → Image → Run → Container → Push → Registry

---

# 12. Why Containers Are Smaller Than VMs (Summary)

Containers:
- Share host kernel
- Contain minimal required files
- Use namespaces and cgroups for isolation
- Do not include full OS

VMs:
- Emulate entire OS
- Require hypervisor
- Include full system libraries and kernel

Therefore:
Container images are significantly smaller than VM images by design.

---


