[comment]: # (🐳 The Ultimate Docker Command Cheat Sheet for Developers)

## 📌 Introduction

Docker has revolutionized how we build, ship, and run applications — making development faster, more consistent, and scalable. But with so many commands to remember, it’s easy to get lost.

This cheat sheet brings together the most essential Docker commands in one place, so you can work faster, avoid context switching, and focus on building great software.

Whether you're just starting or need a quick refresher — this guide has you covered. 🐳⚙️

🧱 Docker Installation & Setup
- **Windows**: To install Docker in your windows machine you just need to download the Docker Desktop from the official website and follow the installation instructions.<br>
 You can download it from [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop).<br>
  for more information on how to install Docker in Windows, you can refer to the [official documentation](https://docs.docker.com/desktop/windows/install/).

- **Mac**: To install Docker in your mac machine you just need to download the Docker Desktop from the official website and follow the installation instructions. <br>
You can download it from [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop). <br>
for more information on how to install Docker in Mac, you can refer to the [official documentation](https://docs.docker.com/desktop/mac/install/).

- **Linux**: To install Docker in your Linux machine you just need to follow the instructions for your specific distribution. You can find the installation instructions for various Linux distributions in the [official documentation](https://docs.docker.com/engine/install/).  


To check if Docker is installed correctly, you can run the following command in your terminal:

```bash
docker --version
```

Now that you have Docker installed, let's dive into the commands that will help you manage your containers, images, and networks effectively.

## 📦 Working with Docker Images
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



## 🧰 Working with Containers
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

## 🪛Troubleshooting Containers

- **View Container Logs**: Check the logs of a running or stopped container.
  ```bash
  docker logs <container_name or container_id>
    # Example: docker logs myapp_container
  ```
  Here, `<container_name>` is the name of the container whose logs you want to view, or you can use `<container_id>` which is the unique identifier for the container.
  This command displays the standard output and error logs from the container, which can help you diagnose issues or understand what the application is doing.

  <br>

- **View Real-time Logs**: Stream logs from a running container in real-time.
  ```bash
  docker logs -f <container_name or container_id>
    # Example: docker logs -f myapp_container
  ```
  Here, `-f` stands for "follow," allowing you to see new log entries as they are generated. This is useful for monitoring the container's activity in real-time.

    <br>

- **Execute Command in Running Container**: Run a command inside a running container.
  ```bash
  docker exec -it <container_name or container_id> <command>
    # Example: docker exec -it myapp_container /bin/bash
  ```
  Here, `-it` allows you to interact with the container's shell, and `<command>` is the command you want to run inside the container. This is useful for debugging or performing administrative tasks directly within the container.

    <br>

- **Inspect Container**: View detailed information about a container.
  ```bash
  docker inspect <container_name or container_id>
    # Example: docker inspect myapp_container
  ```
  This command provides detailed information about the container, including its configuration, state, network settings, and more. The output is in JSON format, which can be useful for debugging or understanding how the container is set up. 
    You can also use the `--format` option to filter the output and display specific information. For example, to get the container's IP address:
    ```bash
    docker inspect --format '{{ .NetworkSettings.IPAddress }}' <container_name or container_id>
      # Example: docker inspect --format '{{ .NetworkSettings.IPAddress }}' myapp_container
    ```

    <br>

- **Check Container Resource Usage**: View resource usage statistics for a container.
  ```bash
  docker stats <container_name or container_id>
    # Example: docker stats myapp_container
  ```
  This command displays real-time statistics about the container's CPU, memory, network I/O, and disk I/O usage. It helps you monitor the performance of your container and identify any resource bottlenecks.


- **Check Container Health Status**: View the health status of a container.
  ```bash
  docker inspect --format '{{ .State.Health.Status }}' <container_name or container_id>
    # Example: docker inspect --format '{{ .State.Health.Status }}' myapp_container
  ```
  This command checks the health status of a container, which is useful if you have defined a health check in your Dockerfile or when running the container. The health status can be `healthy`, `unhealthy`, or `starting`, indicating whether the container is functioning correctly or experiencing issues.

## 🌐 Working with Docker Networks
- **List Networks**: View all Docker networks on your system.
  ```bash
  docker network ls
  ```
- **Create Network**: Create a new Docker network.
  ```bash
  docker network create <network_name>
    # Example: docker network create my_network
  ```
  This command creates a new network that containers can use to communicate with each other. You can specify additional options such as the network driver (e.g., `bridge`, `overlay`, `null` etc.) using the `--driver` flag:
  ```bash
  docker network create --driver bridge my_network
    # Example: docker network create --driver bridge my_network
  ```

<br>

- **Connect Container to Network**: Attach a container to a specific network.
  ```bash
    docker network connect <network_name> <container_name or container_id>
        # Example: docker network connect my_network myapp_container
    ``` 
    This command connects a running container to a specified network, allowing it to communicate with other containers on that network. If the container is not running, you can use the `--alias` option to assign an alias for the container on the network:
    ```bash
    docker network connect --alias my_alias my_network myapp_container
        # Example: docker network connect --alias my_alias my_network myapp_container
    ```
    This allows you to refer to the container by the alias within the network, making it easier to manage and communicate with multiple containers.


- **Disconnect Container from Network**: Remove a container from a network.
  ```bash
  docker network disconnect <network_name> <container_name or container_id>
    # Example: docker network disconnect my_network myapp_container
  ```
  This command disconnects a running container from a specified network, preventing it from communicating with other containers on that network. If the container is not running, you can still disconnect it using the same command.

  <br>

- **Inspect Network**: View detailed information about a network.
  ```bash
  docker network inspect <network_name>
    # Example: docker network inspect my_network
  ```
  This command provides detailed information about the specified network, including its configuration, connected containers, and IP address ranges. The output is in JSON format, which can be useful for debugging or understanding how the network is set up. You can also use the `--format` option to filter the output and display specific information. For example, to get the list of connected containers:
  ```bash
  docker network inspect --format '{{ range .Containers }}{{ .Name }} {{ end }}' <network_name>
    # Example: docker network inspect --format '{{ range .Containers }}{{ .Name }} {{ end }}' my_network
  ```


  <br>

- **Remove Network**: Delete a Docker network.
  ```bash
  docker network rm <network_name>
    # Example: docker network rm my_network
  ```
  This command removes a specified network from your system. You can only remove networks that are not currently in use by any containers. If you try to remove a network that is still connected to one or more containers, Docker will return an error. To forcefully remove a network and disconnect all connected containers, you can use the `-f` flag:
  ```bash
  docker network rm -f <network_name>
    # Example: docker network rm -f my_network
  ```

  - **List Containers in a Network**: View all containers connected to a specific network.
  ```bash
  docker network inspect <network_name> --format '{{ range .Containers }}{{ .Name }} {{ end }}'
    # Example: docker network inspect my_network --format '{{ range .Containers }}{{ .Name }} {{ end }}'
  ```
  This command lists all containers that are currently connected to the specified network. The output will show the names of the containers, making it easy to see which containers are part of the network. If you want to see more details about each container, you can modify the `--format` option to include additional information, such as the container ID or IP address:
  ```bash
  docker network inspect <network_name> --format '{{ range .Containers }}{{ .Name }} ({{ .ID }}) - {{ .IPv4Address }} {{ end }}'
    # Example: docker network inspect my_network --format '{{ range .Containers }}{{ .Name }} ({{ .ID }}) - {{ .IPv4Address }} {{ end }}'
  ```

  <br>

  - **Remove all unused networks**: Clean up networks that are not in use by any containers.
  ```bash
  docker network prune
  ```
    This command removes all networks that are not currently in use by any containers. It helps keep your Docker environment clean and free of unused resources. Docker will prompt you for confirmation before proceeding with the removal. If you want to skip the confirmation prompt, you can use the `-f` flag:
  ```bash
  docker network prune -f
    ```
    This will forcefully remove all unused networks without asking for confirmation.    


## 💻Docker HUB 

- **Login to Docker Hub**: Authenticate with your Docker Hub account.
  ```bash
  docker login
  ```
  This command prompts you for your Docker Hub username and password, allowing you to push and pull images from your Docker Hub repository. If you have two-factor authentication enabled, you will need to use a personal access token instead of your password.

  <br>

- **Push an Image to Docker Hub**: Upload a local image to your Docker Hub repository.
  ```bash
  docker push <your_dockerhub_username>/<image_name>:<tag>
  ```
  This command pushes a local image to your Docker Hub repository. Make sure to replace `<your_dockerhub_username>`, `<image_name>`, and `<tag>` with your actual Docker Hub username, the name of the image you want to push, and the tag you want to assign to the image. If the image is not tagged, you can tag it using the `docker tag` command before pushing.

  <br>

- **Pull an Image from Docker Hub**: Download an image from Docker Hub repository.
  ```bash
  docker pull <image_name>:<tag>
  ```
    This command pulls an image from Docker Hub to your local machine. If the image is not available locally, Docker will automatically download it from the specified repository. If no tag is specified, Docker defaults to `latest`. You can also pull images from specific repositories by specifying the repository name in the format `<repository_name>/<image_name>:<tag>`.
    ```bash
    docker pull <repository_name>/<image_name>:<tag>
      # Example: docker pull myrepo/myapp:latest
    ```

  <br>

- **Search for Images on Docker Hub**: Find images available on Docker Hub.
  ```bash
  docker search <search_term>
  ```
  This command searches for images on Docker Hub that match the specified search term. It returns a list of images along with their names, descriptions, and star ratings. You can use this command to discover new images or find official images for popular software.

- **Logout from Docker Hub**: Log out of your Docker Hub account.
  ```bash
  docker logout
  ```
  This command logs you out of your Docker Hub account, removing your authentication credentials from the local Docker client. You will need to log in again using `docker login` to push or pull images from your Docker Hub repository.

## 📂 Docker Volumes & Data Management
- **List Volumes**: View all Docker volumes on your system.
  ```bash
  docker volume ls
  ```
  This command lists all Docker volumes on your system, showing their names and other details. Docker volumes are used to persist data generated by and used by Docker containers. By listing the volumes, you can see which ones are available for use or need to be cleaned up.

- **Create Named Volume**: Create a new Docker volume with a specific name.
  ```bash
  docker volume create <volume_name>
    # Example: docker volume create my_volume
  ```
  This command creates a new Docker volume with the specified name. Named volumes are useful for persisting data across container restarts and for sharing data between multiple containers.

  <br>

- **Remove Volume**: Delete a Docker volume.
  ```bash
  docker volume rm <volume_name>
    # Example: docker volume rm my_volume
  ```
  This command removes a specified Docker volume from your system. If the volume is currently in use by any containers, you will need to stop and remove those containers first. If you want to forcefully remove a volume that is in use, you can use the `-f` flag:
  ```bash
  docker volume rm -f <volume_name>
    # Example: docker volume rm -f my_volume
  ```   

    <br>

- **Mount Named volume with running container**: Attach a named volume to a running container.
  ```bash
  docker run -d -v <volume_name>:<container_directory> --name <container_name> <image_name>:<tag>
    # Example: docker run -d -v my_volume:/data --name myapp_container myapp:latest
  ```
  This command mounts a named volume to a specific directory inside the container, allowing the container to read and write data to that volume. `<volume_name>` is the name of the volume you want to mount, and `<container_directory>` is the path inside the container where you want to mount the volume.

  <br>

  - **Mount Named volume with running container using `--mount`**: 
  ```bash
  docker run -d --mount type=volume,source=<volume_name>,target=<container_directory> --name <container_name> <image_name>:<tag>
    # Example: docker run -d --mount type=volume,source=my_volume,target=/data --name myapp_container myapp:latest
  ```
  This command achieves the same result as the previous one but uses the `--mount` flag, which provides a more flexible and explicit way to specify volume mounts. The `type=volume` indicates that you are mounting a Docker volume, `source=<volume_name>` specifies the name of the volume, and `target=<container_directory>` is the path inside the container where the volume will be mounted. This method is often preferred for its clarity and flexibility, especially when dealing with multiple mounts or complex configurations.

  <br>

- **Mount Anonymous volume with running container**: Attach an anonymous volume to a running container.
  ```bash
  docker run -v <container_directory> --name <container_name> <image_name>:<tag>
    # Example: docker run -d -v /data --name myapp_container myapp:latest
  ```
  This command creates an anonymous volume that is automatically managed by Docker and mounts it to the specified directory inside the container. The volume will be removed when the container is deleted, making it suitable for temporary data storage.

  <br>


  - **To create a Bind Mount**: Attach a host directory to a container.
  ```bash
  docker run -d -v <host_directory>:<container_directory> --name <container_name> <image_name>:<tag>
    # Example: docker run -d -v /path/to/host/dir:/data --name myapp_container myapp:latest
  ```
  This command mounts a directory from your host machine into the container, allowing the container to read and write data to that directory. `<host_directory>` is the path on your host machine, and `<container_directory>` is the path inside the container where you want to mount the directory. This is useful for sharing files between the host and the container or for persisting data that should remain available even after the container is removed.

  <br>

  - **To create a Bind Mount using `--mount`**: 
  ```bash
  docker run -d --mount type=bind,source=<host_directory>,target=<container_directory> --name <container_name> <image_name>:<tag>
    # Example: docker run -d --mount type=bind,source=/path/to/host/dir,target=/data --name myapp_container myapp:latest
  ```
  This command achieves the same result as the previous one but uses the `--mount` flag, which provides a more flexible and explicit way to specify bind mounts. The `type=bind` indicates that you are mounting a directory from the host, `source=<host_directory>` specifies the path on the host machine, and `target=<container_directory>` is the path inside the container where the directory will be mounted. This method is often preferred for its clarity and flexibility, especially when dealing with multiple mounts or complex configurations.

  <br>


- **Inspect Volume**: View detailed information about a Docker volume.
  ```bash
  docker volume inspect <volume_name>
    # Example: docker volume inspect my_volume
  ```
  This command provides detailed information about the specified volume, including its name, driver, mount point, and any labels associated with it. The output is in JSON format, which can be useful for debugging or understanding how the volume is set up. You can also use the `--format` option to filter the output and display specific information. For example, to get the mount point of the volume:
  ```bash
  docker volume inspect --format '{{ .Mountpoint }}' <volume_name>
    # Example: docker volume inspect --format '{{ .Mountpoint }}' my_volume
  ``` 

- **Remove all unused volumes**: Clean up volumes that are not in use by any containers.
  ```bash
  docker volume prune
    ```
    This command removes all volumes that are not currently in use by any containers. It helps keep your Docker environment clean and free of unused resources. Docker will prompt you for confirmation before proceeding with the removal. If you want to skip the confirmation prompt, you can use the `-f` flag.


## 🧩 Docker Compose
To manage multi-container applications, Docker Compose allows you to define and run multi-container Docker applications using a simple YAML file.

- **Create a `docker-compose.yml` file**: Define your services, networks, and volumes in a YAML file.
```yaml
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: example
    volumes:
      - db_data:/var/lib/mysql
volumes:
  db_data:
```
This example defines two services: `web` (using the Nginx image) and `db` (using the MySQL image). The `web` service maps port 8080 on the host to port 80 in the container and mounts a local directory `./html` to the Nginx HTML directory. The `db` service sets an environment variable for the MySQL root password and mounts a named volume `db_data` to persist MySQL data. 

<br>

- **Start Services**: Run the services defined in the `docker-compose.yml` file.
```bash
docker-compose up -d
```
This command starts all the services defined in the `docker-compose.yml` file in detached mode (`-d`). If you want to run the services in the foreground, you can omit the `-d` flag:
```bash
docker-compose up
```

<br>

- **Stop Services**: Stop the running services defined in the `docker-compose.yml` file.
```bash
docker-compose down
```
This command stops and removes all the containers defined in the `docker-compose.yml` file, along with their networks and volumes. If you want to remove the volumes as well, you can use the `-v` flag:
```bash
docker-compose down -v
```

<br>

- **Build services**: Build the images for the services defined in the `docker-compose.yml` file.
```bash
docker-compose build
```
This command builds the images for the services defined in the `docker-compose.yml` file. If you have made changes to the Dockerfiles or the context, this command will rebuild the images accordingly.


<br>

- **List running services**: View the status of the services defined in the `docker-compose.yml` file.
```bash
docker-compose ps
```
This command lists all the services defined in the `docker-compose.yml` file along with their current status (running, exited, etc.). It provides a quick overview of the state of your multi-container application.

<br>

- **Execute command in a container**: Run a command inside a specific service container.
```bash
docker-compose exec <service_name> <command>
``` 
For example, to open a shell in the `web` service container:
```bash
docker-compose exec web /bin/bash
```
This command allows you to run a command inside a specific service container defined in the `docker-compose.yml` file. The `exec` command is useful for debugging or performing administrative tasks directly within the container. You can replace `<service_name>` with the name of the service you want to access, and `<command>` with the command you want to run inside that service's container. If you want to run an interactive shell, you can use `-it` flags:
```bash
docker-compose exec -it <service_name> /bin/bash
```


## 🗑️ Cleaning Up Docker Resources
- **Remove all unused volumes**: Clean up volumes that are not in use by any containers.
```bash
docker volume prune
```
  This command removes all volumes that are not currently in use by any containers. It helps free up disk space and keep your Docker environment tidy. Docker will prompt you for confirmation before proceeding with the removal. If you want to skip the confirmation prompt, you can use the `-f` flag:
```bash
docker volume prune -f
```
  This will forcefully remove all unused volumes without asking for confirmation.

  <br>

- **Remove all unused images**: Clean up images that are not tagged or used by any containers.
```bash
docker image prune
```
  This command removes all images that are not tagged or used by any containers. It helps free up disk space and keep your Docker environment tidy. Docker will prompt you for confirmation before proceeding with the removal. If you want to skip the confirmation prompt, you can use the `-f` flag:


  <br>
  

- **Remove all unused containers**: Clean up stopped containers.
```bash
docker container prune
```
  This command removes all stopped containers from your system. It helps free up resources and keep your Docker environment tidy. Docker will prompt you for confirmation before proceeding with the removal. If you want to skip the confirmation prompt, you can use the `-f` flag:

  <br>


## Conclusion
This cheat sheet provides a quick reference to the most commonly used Docker commands, helping you manage your Docker environment efficiently. Whether you're working with images, containers, networks, or volumes, these commands will help you streamline your workflow and focus on building great applications.
Feel free to bookmark this page or print it out for easy access while working with Docker. Happy Dockering! 🐳🚀

## 📝 Additional Resources
- [Docker Documentation](https://docs.docker.com/)
- [Docker GitHub Repository](https://github.com/docker)
- [Docker Community Forums](https://forums.docker.com/)


## 💬 Have Questions or Suggestions?

Drop a comment below or connect with me on [LinkedIn](https://www.linkedin.com/in/kuntal-maity-8aa4612a9/) or [GitHub](https://github.com/kuntal-hub). Let’s make apps faster together! 🚀