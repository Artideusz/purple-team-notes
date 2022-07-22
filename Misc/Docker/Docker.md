# Docker

## What is Docker?
Docker is a utility that isolates your app from the system and manages it, making it possible to be ran on any device that supports docker. Think of it like a virtual machine, but instead of creating a whole separate operating system to run and manage the app that is compatible to that OS, docker uses your linux kernel (or if you are on windows, a virtualized version of a linux kernel) to "[virtualize](https://en.wikipedia.org/wiki/OS-level_virtualization) and isolate" your app.

## How can we benefit from Docker?
A great thing about Docker is that you do not need to be stressed out if something won't work, because Docker uses only the linux kernel for virtualization and installs the tools in that virtualized environment, which means that any app, java-based, python-based should all work without needing to install the needed dependencies on your operating system. 

Docker can also help you greatly in specific situations, for example, if you have someone else's computer or a new one with you and need to use your hacking toolkit, you can install docker and your image with the toolkit and you are good to go!

Another benefit of using Docker is running Vulnerable Web Apps (like Juice Shop, or DVWA). It allows you to setup vulnerable web apps very easily, so you can focus on studying various web attacks and developing your own scripts and exploits.

## How do I create my own Docker image?
Creating an image is as easy as creating one file (Dockerfile) with steps that create and run your application. A simple example would be:

Dockerfile in my [example node server:](https://hub.docker.com/r/artideusz/test-node-server)
```
FROM node:14.9

COPY ./src/ /opt/node-server

WORKDIR /opt/node-server

RUN npm install

EXPOSE 8080

ENTRYPOINT ["node","/opt/node-server/index.js"]
```

1. `FROM node`
    - First, you need a base image that the app will run on. Since my app is a node application, I chose this as the base image. I could've chosen an OS, like Arch Linux, but node has that included and optimized so it is as lightweight as possible. You can use web servers like "Apache" or "nginx" and even distros too, for example Ubuntu, Linux Mint, Debian and [much more](https://hub.docker.com/search?source=verified&type=image&category=base&image_filter=official&operating_system=linux%2Cwindows)!
    - FROM command installs a base image that you can use to install the needed dependancies for an application or run it out-of-the-box.
2. `COPY . /opt/node-server`
    - Here, I basically copy all the application files in my current working directory and paste them inside the container's `/opt/node-server` directory.
3. `WORKDIR /opt/node-server`
    - Pretty self-explanatory. This command changes the current working directory, just like the `cd` command.
3. `RUN npm install`
    - The next step after installing a base image is to update all the packages and install the needed dependancies for an application. For Ubuntu, you will use the `apt` command to install and update dependancies. Node already has npm in its arsenal of preinstalled tools, but if a base image lacks this package, simply install it using the base images package manager.
    - RUN command runs a command via the base image that has been installed through the FROM command.

4. `ENTRYPOINT ["node", "/opt/node-server"]`
    - The last step is to run a command upon running a container. There are 2 types of commands that allow running commands on startup of a container. First, there is `CMD <command> <param1> ...`. This command can be overwritten if a command is specified.

A full list of commands can be found [here.](https://docs.docker.com/engine/reference/builder/)

## What are docker volumes?

Docker volumes are a way to persist data generated and used by a container. When removing a container, data saved will be lost. It is a great way to prevent data from being lost by making an "active" copy on your host machine that is managed by docker. To create a volume, you type in the following command: `docker volume create <name_of_the_volume>`. You do not need to add a size to a volume because volumes have a dynamic size. To attach and use a volume, you type in: `docker run -v <bind_mount || volume_name>:<folder_in_container_to_be_persisted> <image>`.
You can also use a **bind mount**, which is basically a folder on the host system that you create to share files with docker containers (works the same as volumes, but you can manually edit bind mounts on your host machine).

## What are docker networks?

Docker networks are networks that you can connect multiple applications with. It is very helpful if you have for example a website that uses mysql, but doesnt have mysql in the image. Thanks to this functionality you can access one container from another container.

You can create a docker network using the `docker network create` command. Add `-h` for arguement descirption.

You can connect a docker container to a specific network using the `--network` flag inside `docker run`.


## What is docker-compose?

Docker-compose is a tool to automate, manage and run multiple containers in a single application. You use a yaml file that defines what should be set up for your application, on what network and with what environment variables.

## What is a docker-compose.yaml file?

This file defines how docker-compose should run your application. An example docker-compose.yaml file can be:

```yaml
services:
 wordpress:
  depends_on:
  - db
  image: wordpress
  restart: always
  ports:
  - 8080:80
  environment:
   WORDPRESS_DB_HOST: db
   WORDPRESS_DB_USER: user
   WORDPRESS_DB_PASSWORD: password
   WORDPRESS_DB_NAME: wordpress_test
  volumes:
  - "./html:/var/www/html"
  networks:
  - test_network
 db:
  image: mysql
  restart: always
  environment:
   MYSQL_USER: user
   MYSQL_PASSWORD: password
   MYSQL_ROOT_PASSWORD: secretpassword
   MYSQL_DATABASE: wordpress_test
  volumes:
  - "db_volume:/var/lib/mysql"
  networks:
  - test_network

networks:
 test_network:

volumes:
 db_volume:
```

What does this all mean? Let me explain every command step-by-step.

1. `services`
    - The `services` key is simply a block where you define all your services, like your application and database. Values inside `services` are names of your services.

2. `wordpress`
    - You can call this anything you like. This `wordpress` key is also a block that contains information about this particular service. The same goes for the `db` key.

3. `depends_on`
    - This key basically means that the `wordpress` service won't start until `db` doesn't start. Bear in mind this key starts the service when `db` starts, not when `db` is ready for use.

4. `image`
    - This key defines the base image that should be run. You have a lot of images available on docker hub. You can also use your own images that are stored locally on your computer.

5. `restart`
    - This key defines if the container should restart upon exiting. Full list [here.](https://docs.docker.com/config/containers/start-containers-automatically/)

6. `ports`
    - The `ports` key is basically the `-p` argument in docker.

7. `environment`
    - The `environment` variable is simply the `-e` flag.

8. `service -> volumes`
    - The `volumes` flag defines the volumes that should be mounted on a service. In `wordpress`, the data is synchronized with my current directory + `/html/`, which is a `bind mount`. If a volume doesn't exist, it is created for you.

9. `service -> networks`
    - The `networks` key defines the networks that the services should be connected to. If the network doesn't exist, it is created.

10. `volumes`
    - The `volumes` key defines what volumes should be loaded or created. The volumes used in services should also be defined here. The same thing with `networks`.

## List of docker commands
- `docker run <...FLAGS> <image_id || image_name>:<TAG=latest> <COMMAND>` - Runs a docker container if present. If not present, docker pulls the specified container image from the docker hub and immediately runs it.
    - `-d` - Detached mode. Runs container in the background.
    - `-i` - Interactive mode. Keeps STDIN open.
    - `--rm` - Remove container until all processes are finished.
    - `-t` - Allocate a pseudo TTY (terminal). Helpful for formatting the terminal.
    - `-v` - Map volume to a container.
    - `-p` - Map a host port to a container port, so you can access it via host machine.
    - `-e` - Set an environment variable. Useful if you want to change something without changing the code in an image.
    - `--name <name_of_container>` -  Set the name of the container. You can use this name to manage the container without typing the id.
    - `--link <>:<>` - 
- `docker ps -a` - Lists all docker containers that are currently running. You can also use the `-a` flag to list all recently closed docker containers.
- `docker stop <id || name>` - Stops the specific container.
- `docker rm <id || name>` - Removes a container specified. (NOT THE IMAGE)
- `docker images` - Lists all available images.
- `docker rmi <image_name>` - Removes an image.
- `docker pull <image_name>` - Downloads an image from docker hub.
- `docker attach <id || name>` - Attaches a container to the terminal.
- `docker inspect <id || name>` - Shows details about a container, like the current network of the container.
- `docker logs <id || name>` - Logs information as if you ran a container from the terminal unattached.
- `docker commit <container> <new_image>` - Creates a new image using a container.
- `docker cp <file || folder> <container>:<destination_file || folder>` - Copy a file or folder to a destination file or folder in the container.
- `docker build <project_directory>` - Build an image using a Dockerfile inside a project.
    - `-t` - Tag name of the image.
- `docker container rm -f $(docker container ls –aq)` - Stops and removes all containers
- `docker image prune -a` - Removes all images that do not have at least one container running.
- `docker inspect <container>` - inspect settings of container, like ip address, network.
- `docker network inspect <network_name>` - inspect settings of network, like the subnet and ip range.
- `docker network create --subnet=<cidr_ip_address> <network_name>` - creates a network.


### Useful resources
- [Kodekloud Docker Course - FREE (To Be Checked)](https://kodekloud.com/p/docker-labs)
- [YouTube - FreeCodeCamp Docker Tutorial for Beginners](https://www.youtube.com/watch?v=fqMOX6JJhGo)
- [Docker Volumes](https://docs.docker.com/storage/volumes/)
- [YouTube - Docker Volumes](https://www.youtube.com/watch?v=VOK06Q4QqvE)
- [Docker for Hackers - A Pentesters Guide](https://www.pentestpartners.com/security-blog/docker-for-hackers-a-pen-testers-guide/)

#### ToDo:
- Docker registry