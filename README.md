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




***🧭 ENTRYPOINT: Defining the Executable***
The ENTRYPOINT instruction specifies the command that will always be executed when the container starts. It is ideal for containers that should run a specific application or script.

Syntax:

**Shell form:**
ENTRYPOINT command param1 param2

**Exec form (preferred):**
ENTRYPOINT ["executable", "param1", "param2"]

Example:
Dockerfile

```
FROM ubuntu:18.04
ENTRYPOINT ["ping", "-c", "4"]
```

In this example, the container will always execute ping -c 4 when started


**🧾 CMD: Providing Default Arguments**
The CMD instruction provides default arguments for the ENTRYPOINT command. If arguments are provided when running the container, they will override the CMD values.

Syntax:

**Shell form:**
CMD ["param1", "param2"]

**Exec form:**
CMD ["executable", "param1", "param2"]


Example:

Dockerfile
```
FROM ubuntu:18.04
ENTRYPOINT ["ping", "-c"]
CMD ["4", "localhost"]
```
Here, the container will default to ping -c 4 localhost, but if you run the container with different arguments, like docker run <image> 5 google.com, it will execute ping -c 5 google.com

11. Writing a Simple Dockerfile – Step-by-Step
Example for Node.js:
dockerfile
```
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```
Each step builds a layer. Use .dockerignore to avoid copying unnecessary
files like node_modules


[![Hello World Docker Container ...](https://images.openai.com/thumbnails/d760602e936bd2cc3879ed24fd9e39ba.png)](https://flaviocopes.com/docker-node-container-example/)

Certainly! Let's break down the Dockerfile instructions you provided:

```dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```

### 1. `FROM node:18`

This line specifies the base image for your Docker container. By using `node:18`, you're selecting the official Node.js image with version 18. This image includes Node.js and npm, providing a ready-to-use environment for your application.

### 2. `WORKDIR /app`

Sets the working directory inside the container to `/app`. All subsequent commands will be executed in this directory. If the directory doesn't exist, Docker will create it.
If the WORKDIR instruction is not specified in a Dockerfile, the default WORKDIR is the root directory (/)

### 3. `COPY package*.json ./`

Copies the `package.json` and `package-lock.json` files from your local machine to the `/app` directory in the container. This step is crucial for installing dependencies. By copying these files before the rest of your application code, Docker can leverage caching. If these files haven't changed, Docker can reuse the cached layer, speeding up the build process. ([Maxim Orlov][3], [WIREDGORILLA][1], [CroCoder][4])

### 4. `RUN npm install`

Installs the dependencies specified in `package.json` using npm. This command runs inside the container and ensures that all necessary packages are available for your application.([Medium][2])

### 5. `COPY . .`

Copies the rest of your application code into the container's `/app` directory. This step is performed after installing dependencies to take advantage of Docker's caching mechanism. If your application code changes frequently, this approach minimizes the number of layers that need to be rebuilt. ([Medium][2], [This Dot Labs][5])

### 6. `EXPOSE 3000`

Informs Docker that the container will listen on port 3000 at runtime. While this doesn't publish the port, it's a way to document which ports the application will use.([WIREDGORILLA][1])

### 7. `CMD ["node", "index.js"]`

Specifies the command to run when the container starts. In this case, it runs `index.js` using Node.js. This is the entry point of your application

By structuring your Dockerfile in this manner, you create an efficient and maintainable environment for your Node.js application.



