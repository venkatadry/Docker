# Docker

1. Introduction to Containerization and its Need
Containerization is the process of packaging an application and its dependencies
into a standardized unit called a container. Before containers, developers faced
the “It works on my machine” problem because of mismatched environments
across development, testing, and production. Containers solve this by offering
environment consistency, fast scalability, and lightweight deployment across
platforms.
2. What is Docker and Why Use It
Docker is a platform and toolset for building, distributing, and running
containers. It simplifies the containerization process and enables developers to
quickly spin up isolated, reproducible environments. Docker’s ecosystem
includes tools for image creation, orchestration, registry management, and
integration into CI/CD pipelines.
3. Difference Between Virtual Machines and Docker
VMs include a full OS with its own kernel, making them heavyweight and slow
to boot. Docker containers share the host OS kernel, making them lightweight,
fast, and resource-efficient. VMs emulate hardware; containers use OS-level
virtualization. Docker uses namespaces and cgroups to isolate containers on a
shared kernel.
4. Docker Architecture and Components
Docker has three core components:
● Docker Engine (which includes the daemon)

● Docker CLI (command-line interface)
● Docker Objects (images, containers, volumes, networks).
It also includes Docker Compose, Docker Hub, and the Docker Registry.
The client-server architecture allows Docker clients to communicate with
the Docker daemon to build, run, and manage containers.
5. Understanding Docker Daemon, CLI, and Docker Client
● Docker Daemon (dockerd): Listens for Docker API requests and manages
objects.
● Docker Client (docker): The interface users interact with; it sends
commands to the daemon.
● Docker API: REST API used by clients and tools.
Together, these components let developers efficiently build and run
applications inside containers.
6. Docker Engine and How It Works
Docker Engine is the runtime that powers Docker. It handles:
● Image building and layer management
● Container lifecycle (create, start, stop, destroy)
● Volume and network management
Docker Engine runs as a background service and interacts via the Docker
API.

7. Installing Docker on Linux, Windows, and macOS
● Linux: Install via package manager (apt, yum). Configure post-install
access for non-root users.
● Windows/macOS: Use Docker Desktop. It includes Docker Engine, CLI,
Docker Compose, and Kubernetes.
Docker Desktop also provides a GUI dashboard for managing containers
and images.
8. Basic Docker Commands Every Developer Should Know
Key commands include:
● docker version, docker info – show system details
● docker ps, docker images – list containers/images
● docker run, docker exec – start a container or run a command
● docker build, docker pull, docker push – image operations
● docker stop, docker rm, docker rmi – container/image cleanup
9. Working with Docker Images
Docker images are read-only templates used to create containers. They are built
using layers, each representing an instruction in a Dockerfile. Images can be
pulled from Docker Hub or a private registry. The image contains everything: the
app, dependencies, system libraries, and environment configurations.

10. Dockerfile – Purpose and Structure
A Dockerfile is a plain-text file with a set of instructions to build an image. Key
instructions include:
● FROM (base image)
● RUN (commands to execute during build)
● COPY / ADD (include files)
● CMD / ENTRYPOINT (default command)
● EXPOSE (port information)
● ENV (environment variables)
It serves as the blueprint for Docker images.
11. Writing a Simple Dockerfile – Step-by-Step
Example for Node.js:
dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
