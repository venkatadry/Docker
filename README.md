# Docker

1. Introduction to Containerization and its Need
Containerization is the process of packaging an application and its dependencies
into a standardized unit called a container. Before containers, developers faced
the “It works on my machine” problem because of mismatched environments
across development, testing, and production. Containers solve this by offering
environment consistency, fast scalability, and lightweight deployment across
platforms.


3. What is Docker and Why Use It
Docker is a platform and toolset for building, distributing, and running
containers. It simplifies the containerization process and enables developers to
quickly spin up isolated, reproducible environments. Docker’s ecosystem
includes tools for image creation, orchestration, registry management, and
integration into CI/CD pipelines.


5. Difference Between Virtual Machines and Docker
VMs include a full OS with its own kernel, making them heavyweight and slow
to boot. Docker containers share the host OS kernel, making them lightweight,
fast, and resource-efficient. VMs emulate hardware; containers use OS-level
virtualization. Docker uses namespaces and cgroups to isolate containers on a
shared kernel.


7. Docker Architecture and Components
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


7. Docker Engine and How It Works
Docker Engine is the runtime that powers Docker. It handles:
● Image building and layer management
● Container lifecycle (create, start, stop, destroy)
● Volume and network management
Docker Engine runs as a background service and interacts via the Docker
API.

8. Installing Docker on Linux, Windows, and macOS
● Linux: Install via package manager (apt, yum). Configure post-install
access for non-root users.
● Windows/macOS: Use Docker Desktop. It includes Docker Engine, CLI,
Docker Compose, and Kubernetes.
Docker Desktop also provides a GUI dashboard for managing containers
and images.


10. Basic Docker Commands Every Developer Should Know
Key commands include:
● docker version, docker info – show system details
● docker ps, docker images – list containers/images
● docker run, docker exec – start a container or run a command
● docker build, docker pull, docker push – image operations
● docker stop, docker rm, docker rmi – container/image cleanup


12. Working with Docker Images
Docker images are read-only templates used to create containers. They are built
using layers, each representing an instruction in a Dockerfile. Images can be
pulled from Docker Hub or a private registry. The image contains everything: the
app, dependencies, system libraries, and environment configurations.

13. Dockerfile – Purpose and Structure
A Dockerfile is a plain-text file with a set of instructions to build an image. Key
instructions include:
● FROM (base image)
● RUN (commands to execute during build)
● COPY / ADD (include files)
● CMD / ENTRYPOINT (default command)
● EXPOSE (port information)
● ENV (environment variables)
It serves as the blueprint for Docker images.




***🧭 ENTRYPOINT:***
In Docker, the `ENTRYPOINT` instruction is used to specify the command that will always be executed when a container starts. Unlike `CMD`, which provides default arguments that can be overridden, `ENTRYPOINT` defines the primary purpose of the container and is not easily overridden.

### **Syntax:**
```dockerfile
ENTRYPOINT ["executable", "param1", "param2"]  # (exec form, preferred)
ENTRYPOINT command param1 param2               # (shell form)
```

### **Key Points:**
1. **Purpose:** Defines the main process of the container.
2. **Overriding:** Difficult to override (requires `--entrypoint` in `docker run`).
3. **Combination with `CMD`:** If both `ENTRYPOINT` and `CMD` are present, `CMD` acts as default arguments to `ENTRYPOINT`.
4. **Shell vs Exec Form:**
   - **Exec form (recommended):** Runs directly without a shell (better signal handling).
   - **Shell form:** Runs inside `/bin/sh -c` (allows shell features but may interfere with signals).

### **Example:**
```dockerfile
FROM alpine
ENTRYPOINT ["echo", "Hello"]
CMD ["Docker"]  # Acts as default argument
```
- Running `docker run <image>` outputs: `Hello Docker`
- Running `docker run <image> World` outputs: `Hello World` (overrides `CMD` but not `ENTRYPOINT`)

### **Override Entrypoint at Runtime:**
```bash
docker run --entrypoint /bin/bash <image>  # Overrides ENTRYPOINT
```

### **Best Practices:**
- Use **exec form** for better signal handling.
- Prefer `ENTRYPOINT` for defining the main command and `CMD` for default arguments.
- Avoid mixing `ENTRYPOINT` (shell form) with `CMD` unless necessary.

Would you like a more detailed explanation or a specific use case? 😊


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

```dockerfile
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



### 1. `FROM node:18`

This line specifies the base image for your Docker container. By using `node:18`, you're selecting the official Node.js image with version 18. This image includes Node.js and npm, providing a ready-to-use environment for your application.

### 2. `WORKDIR /app`

Sets the working directory inside the container to `/app`. All subsequent commands will be executed in this directory. If the directory doesn't exist, Docker will create it.
If the WORKDIR instruction is not specified in a Dockerfile, the default WORKDIR is the root directory (/)

### 3. `COPY package*.json ./`

Copies the `package.json` and `package-lock.json` files from your local machine to the `/app` directory in the container. This step is crucial for installing dependencies. By copying these files before the rest of your application code, Docker can leverage caching. If these files haven't changed, Docker can reuse the cached layer, speeding up the build process. ([Maxim Orlov][3], [WIREDGORILLA][1], [CroCoder][4])

### 4. `RUN npm install`

Installs the dependencies specified in `package.json` using npm. This command runs inside the container and ensures that all necessary packages are available for your application.([Medium][2])

Let me break down these two Dockerfile instructions for you:

1. `RUN unzip install.zip /opt/install`
   - `RUN` is a Dockerfile instruction that executes a command during the image build process
   - `unzip install.zip /opt/install` is the command being run, which:
     - Unzips the file `install.zip`
     - Extracts its contents to the directory `/opt/install`
   - This assumes there's an `install.zip` file in the current build context

2. `RUN echo hello`
   - Another `RUN` instruction
   - `echo hello` simply outputs the string "hello" during the build process
   - This output will be visible in the build logs but won't affect the final image

Important notes:
- Each `RUN` instruction creates a new layer in the Docker image
- These commands run during image build, not when the container starts
- The unzip command requires that the zip file exists in the build context and that the unzip utility is installed in the image
- For better practices, these might be combined into a single `RUN` instruction to reduce layers:
  ```
  RUN unzip install.zip /opt/install && \
      echo hello
  ```

### 5. `COPY . .`

Copies the rest of your application code into the container's `/app` directory. This step is performed after installing dependencies to take advantage of Docker's caching mechanism. If your application code changes frequently, this approach minimizes the number of layers that need to be rebuilt.

# COPY in Dockerfile

The `COPY` instruction in a Dockerfile is used to copy files and directories from the host machine into the Docker image during the build process.

## Basic Syntax

```dockerfile
COPY <source> <destination>
```

- `<source>`: Path to the file or directory on the host (relative to the build context)
- `<destination>`: Path inside the container where files will be copied

## Examples

1. **Copy a single file**:
   ```dockerfile
   COPY app.py /app/
   ```

2. **Copy multiple files**:
   ```dockerfile
   COPY file1.txt file2.txt /app/
   ```

3. **Copy a directory**:
   ```dockerfile
   COPY src/ /app/src/
   ```

4. **Copy with wildcards**:
   ```dockerfile
   COPY *.sh /scripts/
   ```

5. **Copy while preserving file permissions** (using `--chown`):
   ```dockerfile
   COPY --chown=user:group files/ /app/files/
   ```

## Important Notes

- The source path must be inside the build context (the directory you specify when running `docker build`)
- If the destination doesn't exist, it will be created
- If the destination ends with `/`, it's treated as a directory
- The `COPY` instruction creates a new layer in the image
- For more complex copying needs, consider `.dockerignore` to exclude files




### 6. `EXPOSE 3000`

Informs Docker that the container will listen on port 3000 at runtime. While this doesn't publish the port, it's a way to document which ports the application will use.([WIREDGORILLA][1])

### 7. `CMD ["node", "index.js"]`

Specifies the command to run when the container starts. In this case, it runs `index.js` using Node.js. This is the entry point of your application

By structuring your Dockerfile in this manner, you create an efficient and maintainable environment for your Node.js application.

### ADD
# Adding ADD Instruction in Dockerfile

The `ADD` instruction in a Dockerfile is used to copy files, directories, or remote file URLs from the source to the destination in the container's filesystem.

## Basic Syntax
```dockerfile
ADD <source> <destination>
```

## Examples

1. **Copy a single file**:
```dockerfile
ADD app.py /app/
```

2. **Copy multiple files**:
```dockerfile
ADD file1.txt file2.txt /app/
```

3. **Copy a directory**:
```dockerfile
ADD config /app/config
```

4. **Copy from a URL**:
```dockerfile
ADD https://example.com/file.tar.gz /tmp/
```

5. **Copy with wildcards**:
```dockerfile
ADD *.sh /scripts/
```

6. **Copy and extract tar files automatically**:
```dockerfile
ADD archive.tar.gz /opt/
```

## Important Notes

- The `<source>` can be a file, directory, or URL
- The `<destination>` is an absolute path or path relative to `WORKDIR`
- `ADD` can automatically extract tar archives (gzip, bzip2, xz)
- For simple file copying, `COPY` is often preferred as it's more transparent
- URLs can only be used for remote files, not directories

## Best Practices

1. Use `COPY` instead of `ADD` unless you need the additional features
2. Be specific with paths to avoid unintended file copies
3. Consider using `.dockerignore` to exclude unnecessary files

## COPY vs ADD

While similar, `COPY` is preferred over `ADD` for most use cases because:
- `COPY` is more transparent (just copies files)
- `ADD` has additional features like URL support and automatic tar extraction, which can lead to unexpected behavior


# ENV Instruction in Dockerfile

The `ENV` instruction in a Dockerfile is used to set environment variables that will be available to containers created from the image. These variables can be accessed during the build process and by running containers.

## Basic Syntax

```dockerfile
ENV <key>=<value> ...
```

## Examples

1. **Setting a single variable**:
   ```dockerfile
   ENV MY_NAME="John Doe"
   ```

2. **Setting multiple variables in one line**:
   ```dockerfile
   ENV MY_NAME="John Doe" MY_AGE=30
   ```

3. **Using variables in subsequent instructions**:
   ```dockerfile
   ENV APP_DIR=/usr/src/app
   WORKDIR $APP_DIR
   ```

## Best Practices

1. **Group related environment variables** together for better readability
2. **Place ENV instructions** near the top of the Dockerfile if they're used throughout the build
3. **Consider using .env files** with `docker run --env-file` for sensitive data instead of hardcoding in Dockerfile

## Important Notes

- Environment variables set with `ENV` persist in the final image
- They can be overridden at runtime using `docker run -e` flag
- Variables can be referenced in other Dockerfile instructions using `$variable_name` or `${variable_name}` syntax

## Example Dockerfile

```dockerfile
FROM alpine:latest

ENV APP_VERSION=1.0.0 \
    NODE_ENV=production \
    PORT=8080

RUN echo "Building version ${APP_VERSION} for ${NODE_ENV} environment" && \
    echo "Server will listen on port ${PORT}"

CMD ["sh", "-c", "echo 'App is running on port $PORT'"]
```






