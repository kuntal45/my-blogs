[comment]: # (🐳 Docker Images vs Containers Explained: Complete Beginner's Guide with Commands & Examples)

## 📌 **Introduction**

Docker has become the standard tool for containerization in modern DevOps and software development workflows. Whether you're building microservices, deploying full-stack apps, or simplifying your development environment—**Docker images and containers** are at the heart of it all.

In this guide, we’ll break down the **difference between Docker images and containers**, explain how they work, and walk through **all the essential commands** you need to get started.

By the end of this post, you’ll have a solid foundation to confidently use Docker in your development workflow.

---

## 🧊 **What is a Docker Image?**

A **Docker image** is a **lightweight, standalone, and immutable file** that includes the source code, libraries, dependencies, tools, and settings required to run an application. Think of it as a snapshot or **blueprint** of your application environment.

## 🔑 **Key Characteristics of Docker Images**

A **Docker image** is a **read-only template** that contains everything needed to run an application.

### 1. ✅ **Immutable (Read-Only)**

Once you build a Docker image, its contents **cannot be changed**. Every time you need a change (like a config or new dependency), you must build a new image.

#### 📌 Example:

You built an image `my-node-app:v1`. If you now want to update the app with a new NPM package, you:

* Change your code or Dockerfile
* Rebuild the image as `my-node-app:v2`

```bash
docker build -t my-node-app:v2 .
```

Now you have a new, updated image, while the previous version (`v1`) remains unchanged.


### 2. 📁 **Layered File System**

Images are built in layers. Each command in a `Dockerfile` creates a new layer. These layers can be **cached and reused**, which makes builds faster.

#### 📌 Example:

```dockerfile
FROM node:18         # Base layer
WORKDIR /app         # Adds a new layer
COPY . .             # Adds another layer
RUN npm install      # Yet another layer
```

If you only change the last line (e.g., `RUN` command), Docker reuses the previous layers and builds only the changed ones.


### 3. ☁️ **Stored in Registries**

Images can be stored locally or pushed to cloud-based registries like **Docker Hub**, **GitHub Container Registry**, or **private registries**.

#### 📌 Example:

```bash
docker tag my-node-app:v1 kuntalmaity/my-node-app:v1
docker push kuntalmaity/my-node-app:v1
```

Others can now pull your image using:

```bash
docker pull kuntalmaity/my-node-app:v1
```


### 4. 🔁 **Reusable and Shareable**

You can create **multiple containers** from the same image. You can also **share** the image with your team or CI/CD system to ensure consistent environments.

#### 📌 Example:

```bash
docker run -d --name container1 my-node-app:v1
docker run -d --name container2 my-node-app:v1
```

Both containers run the same app, using the same environment.

---

<br>

## 📦 **What is a Docker Container?**

A **Docker container** is a **running instance** of a Docker image. It’s the live, executable environment created from an image. Containers are **isolated**, **lightweight**, and **portable**—meaning they run the same on any OS that supports Docker.

## 🔑 **Key Characteristics of Docker Containers**

A **Docker container** is a **running instance** of an image. It executes the application and can be stopped, started, or deleted independently of the image.

### 1. 🏃 **Runtime Instance of an Image**

When you start a container from an image, Docker **copies the image and adds a writable layer** to it.

#### 📌 Example:

```bash
docker run -it ubuntu bash
```

You're running a container based on the Ubuntu image, and you can now install packages or create files inside it.


### 2. 📝 **Writable Layer**

Unlike images, containers allow changes. You can:

* Create files
* Install packages
* Edit configuration

But these changes are **temporary** unless saved to a volume or committed to a new image.

#### 📌 Example:

Inside the container:

```bash
apt update && apt install curl
```

Once you exit or delete the container, these changes are lost **unless** you commit them:

```bash
docker commit <container_id> custom-ubuntu
```


### 3. 🔐 **Isolated and Lightweight**

Each container runs in **isolation** with its own file system, processes, and networking. It's like a mini virtual machine but **much more efficient**.

#### 📌 Example:

You can run:

```bash
docker run -d --name app1 my-node-app
docker run -d --name app2 my-node-app
```

Each container runs the same app but **doesn’t interfere** with the other’s file system or memory.


### 4. 🔄 **Ephemeral by Default**

Containers are **temporary by nature**. When stopped or removed, their changes are gone—unless you're using **volumes** or creating a new image.

#### 📌 Example:

```bash
docker run --rm ubuntu echo "hello"
```

This container will run, print "hello", and then delete itself automatically.


### 5. 🧩 **Configurable and Networked**

Containers can be customized at runtime:

* Set environment variables
* Map ports
* Mount volumes
* Link with other containers

#### 📌 Example:

```bash
docker run -d -p 3000:3000 --env NODE_ENV=production my-node-app
```

This container:

* Maps container port 3000 to your machine
* Sets an environment variable

---



## 🔄 **How Docker Images and Containers Work Together**

### 🔧 **Analogy: Blueprints and Houses**

Think of it this way:

* **Docker Image** = 🧾 A **blueprint** for building a house.
* **Docker Container** = 🏠 A **house built** using that blueprint.

You can create **many houses (containers)** using the **same blueprint (image)**, and each house can be lived in, modified, or destroyed without changing the original blueprint.


### 🧪 **Real-World Example**

Imagine you have a Node.js web app. Here’s how Docker makes it portable and reproducible:

#### Step 1: Create a Docker Image (the blueprint)

You write a `Dockerfile`:

```dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

Then build the image:

```bash
docker build -t my-node-app .
```

Now you have an **image named `my-node-app`** — a static, read-only snapshot of your app environment.


#### Step 2: Run a Docker Container (a running app)

Now you spin up a container:

```bash
docker run -d --name node-container -p 3000:3000 my-node-app
```

This command:

* Uses the `my-node-app` image.
* Creates a new **container**.
* Runs the app inside the container.
* Binds port 3000 of the container to your local machine.

Now your app is live and accessible!

---

## ⚖️ Summary Table

| Feature       | Docker Image                    | Docker Container                        |
| ------------- | ------------------------------- | --------------------------------------- |
| Type          | Blueprint / Template            | Running Instance                        |
| Mutability    | Immutable (read-only)           | Mutable (can change at runtime)         |
| Lifecycle     | Built once, reusable            | Created, used, and usually discarded    |
| Storage       | Stored in registries or locally | Runs in memory and temporary filesystem |
| Use Case      | Defines app environment         | Runs the actual app                     |
| Customization | Defined in Dockerfile           | Customized at runtime with CLI options  |
| Example       | `docker build -t app .`         | `docker run -d --name app app-image`    |

---


## 🧰 Essential Docker Image Commands

- **List Images**: View all Docker images on your system.
  ```bash
  docker images
  ```

- **Pull Image**: Download an image from Docker Hub.
  ```bash
  docker pull <image_name>:<tag>
    # Example: docker pull nginx:latest
  ```
  Here, `<image_name>` is the name of the image you want to pull, and `<tag>` is the specific version or tag of the image. If no tag is specified, Docker defaults to `latest`.
  

<br>

- **Build Image**: Create a Docker image from a Dockerfile.
  ```bash
  docker build -t <image_name>:<tag> <path_to_dockerfile>
    # Example: docker build -t myapp:latest .
  ```
    Here, `<image_name>` is the name you want to give to your image, `<tag>` is the version tag (Optional), and `<path_to_dockerfile>` is the directory containing your Dockerfile (often `.` for the current directory), `-t` is used to tag the image with a name and version. If you don't specify a tag, Docker will use `latest` by default and if you don't specify a path, Docker will look for a Dockerfile in the current directory.


<br>

- **Build Image build without cache**:
  ```bash
  docker build --no-cache -t <image_name>:<tag> <path_to_dockerfile>
    # Example: docker build --no-cache -t myapp:latest .
  ```
  This command builds the image without using any cached layers, ensuring a fresh build.


  <br>

- **Remove Image**: Delete a Docker image.
  ```bash
  docker rmi <image_name or image_id>
    ```
    Here, `<image_name>` is the name of the image you want to remove, or you can use `<image_id>` which is the unique identifier for the image. If the image is being used by any container, you will need to stop and remove those containers first.

    <br>

- **Tag Image**: Add a tag to an existing image.
  ```bash
  docker tag <existing_image_name>:<existing_tag> <new_image_name>:<new_tag>
    # Example: docker tag myapp:latest myapp:v1.0
  ```
  This command creates a new tag for an existing image, allowing you to version your images easily.


  <br>

- **Remove dangling images**: Clean up unused images.
  ```bash
  docker image prune
  ```
  This command removes dangling images, which are images that are not tagged and not referenced by any containers. It helps free up disk space by removing images that are no longer needed.

---

<br>

## 🚀 Essential Docker Container Commands

- **List Running Containers**: View all currently running containers.
  ```bash
  docker ps
  ```

- **List All Containers**: View all containers, including stopped ones.
  ```bash
  docker ps -a
  ```

- **Run Container**: Start a new container from an image.
  ```bash
  docker run -d --name <container_name> <image_name>:<tag>
    # Example: docker run -d --name myapp_container myapp:latest
  ```
  Here, `-d` runs the container in detached mode (in the background), `--name` assigns a name to the container, `<image_name>` is the name of the image you want to run, and `<tag>` is the specific version or tag of the image. If no tag is specified, Docker defaults to `latest`. If you want to run the container in the foreground, you can omit the `-d` flag.

  **Note**: If the image is not present locally, Docker will automatically pull it from Docker Hub and then run the container.


  <br>

- **Run Container with Port Mapping**: Map a container port to a host port.
  ```bash
  docker run -d -p <host_port>:<container_port> --name <container_name> <image_name>:<tag>
    # Example: docker run -d -p 8080:80 --name myapp_container myapp:latest
  ```
  Here, `-p` maps the host port to the container port, allowing you to access the application running inside the container from your host machine. `<host_port>` is the port on your host machine, and `<container_port>` is the port inside the container where the application is running.

  <br>

- **Run Container with Volume Mounting**: Mount a host directory as a volume in the container.
  ```bash
  docker run -d -v <host_directory>:<container_directory> --name <container_name> <image_name>:<tag>
    # Example: docker run -d -v /path/to/host/dir:/path/in/container --name myapp_container myapp:latest
  ```
  Here, `-v` mounts a directory from your host machine into the container, allowing you to persist data or share files between the host and the container. `<host_directory>` is the path on your host machine, and `<container_directory>` is the path inside the container where you want to mount the directory.

    <br>


- **Set environment variables in a container**: Pass environment variables to the container at runtime.
  ```bash
  docker run -d -e <ENV_VAR_NAME>=<value> --name <container_name> <image_name>:<tag>
    # Example: docker run -d -e MY_ENV_VAR=my_value --name myapp_container myapp:latest
  ```
  Here, `-e` sets an environment variable inside the container, which can be used by the application running in the container. `<ENV_VAR_NAME>` is the name of the environment variable, and `<value>` is its value.

  <br>


  - **Interactive shell access**: Start a container with an interactive shell.
  ```bash
  docker run -it --name <container_name> <image_name>:<tag> /bin/bash
    # Example: docker run -it --name myapp_container myapp:latest
  ```
  Here, `-it` allows you to interact with the container's shell, and `/bin/bash` specifies the command to run inside the container (in this case, starting a Bash shell). This is useful for debugging or running commands directly inside the container.

<br>

- **Start Container**: Start a stopped container.
  ```bash
  docker start <container_name or container_id>
    # Example: docker start myapp_container
  ```
  Here, `<container_name>` is the name of the container you want to start, or you can use `<container_id>` which is the unique identifier for the container.
    If the container is already running, this command will have no effect.


    <br>

- **Stop Container**: Stop a running container.
  ```bash
  docker stop <container_name or container_id>
    # Example: docker stop myapp_container
  ```
  Here, `<container_name>` is the name of the container you want to stop, or you can use `<container_id>` which is the unique identifier for the container.
  This command sends a SIGTERM signal to the container, allowing it to gracefully shut down. If the container does not stop within a certain timeout period (default is 10 seconds), Docker will forcefully kill it.


    <br>

- **Restart Container**: Restart a running or stopped container.
  ```bash
  docker restart <container_name or container_id>
    # Example: docker restart myapp_container
  ```
  Here, `<container_name>` is the name of the container you want to restart, or you can use `<container_id>` which is the unique identifier for the container.
  This command stops the container if it is running and then starts it again. It is useful for applying changes or recovering from issues without having to remove and recreate the container.

    <br>

- **Remove Container**: Delete a stopped container.
  ```bash
  docker rm <container_name or container_id>
    # Example: docker rm myapp_container
  ```
  Here, `<container_name>` is the name of the container you want to remove, or you can use `<container_id>` which is the unique identifier for the container.
  This command removes the container from your system. If the container is running, you will need to stop it first using `docker stop <container_name or container_id>`. If you want to remove a running container, you can use the `-f` flag to forcefully remove it:
  ```bash
  docker rm -f <container_name or container_id>
    # Example: docker rm -f myapp_container
  ```

  <br>

- **Remove stopped containers**: Clean up all stopped containers.
  ```bash
  docker container prune
  ```
  This command removes all stopped containers from your system, helping you free up resources and keep your environment clean.

---



## 🧠 Conclusion

Docker images and containers are the **core building blocks** of modern app deployment and DevOps pipelines. Understanding their roles and differences is crucial for every developer.

In this blog, we covered:

* What Docker images and containers are
* How they differ
* All essential Docker CLI commands
* Real-world examples to apply right away

Keep experimenting, build your custom images, and confidently run containers with ease!

---

## 📝 Additional Resources
- [Docker Documentation](https://docs.docker.com/)
- [Docker GitHub Repository](https://github.com/docker)
- [Docker Community Forums](https://forums.docker.com/)

## ✍️ Author’s Note

This blog is part of **“Development Blog by Kuntal”**, where I break down core dev concepts in a simple and beginner-friendly way. If you found this helpful, share it or follow me for more content on **Docker**, **Node.js**, and **Full-Stack Development**.

---

## 💬 Have Questions or Suggestions?

Drop a comment below or connect with me on [LinkedIn](https://www.linkedin.com/in/kuntal-maity-8aa4612a9/) or [GitHub](https://github.com/kuntal-hub). Let’s make apps faster together! 🚀


