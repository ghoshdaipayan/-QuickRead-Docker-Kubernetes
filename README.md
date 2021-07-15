# DOCKER 
A quick guide on Docker

# Check installed version
```bash
docker --version
# or
docker -v
```

```bash
# for help
docker --help

# for help for a command
docker <command> --help
```

# Docker First Level Commands

By **first level commands**, i mean commands in Docker (which are written like: "`docker <cmd>`") via which we can create and maintain docker related resources like images, containers, volumes, network etc.  

```bash
docker build
docker run
docker image
docker container
docker volume
docker network
```


# Basic Steps

1. Create a **Dockerfile** file
2. To **build** a docker image
    ```bash
    # basic build command
    docker build .
    ```
3. To **run** a container for the image

    ```bash
    # basic run command
    docker run <image-id>

    # if you need to work with a exposed port number
    docker run -p 3000:80 15e39aabf3d848db3fd0484b143ac09864bdfb765a7ccca3c89376b491477ecf
    ```
4. To **list** all cotainers
    ```bash
    # show all running containers
    docker ps

    # show all (running + stopped) containers
    docker ps -a
    ```
5. To **stop** a running container
    ```bash
    docker stop <container-name>
    ```

# To run a container from an official docker image

For python, docker already has an official image. To _run a container_ for python in our system even when we don't have an image for python in our system, we can use the **run** command. Below command downloads the python image from Docker HUB
```bash
docker run python
```
Alternatively, we can also _pull the official_ container for python from Docker HUB using the **pull** command
```bash
docker pull python
```

**Important Note** : 
* Whenever we need a container, we first need to have an Image
* In most scenarios, our application (app that we have written) would be dependent on some image(s) like _Python_, _Django_, _MongoDB_
* So in order to create an image for our application, we need to have the images of these dependencies in our system also
* To download these dependencies (images), we can pull a image from Docker Hub directly or create a **Dockerfile** where we list down these depencies and when we run a **build** command, Docker will pull these dependencies for us automatically

# Writing a simple Dockerfile

```bash
# layer 1 --> pull dependent image
FROM node
# layer 2 --> declare a working directory
WORKDIR /app
# layer 3 --> copy everything from current dir to /app dir in image
COPY . /app
# layer 4 -->run npm install in image
RUN npm install
# layer 5 --> expose port 80
EXPOSE 80
# layer 6 --> run command <npm server.js> when container is executed
CMD ["node", "server.js"]
```

# Layers
* In the [Writing a simple Dockerfile](#writing-a-simple-dockerfile) section, we have 6 steps. Each step is a layer and layer makes up a Image
* Docker keeps a cache for the result of each layer during build phase. So when we execute the _build_ command again, if docker identifies that there will be no change in the result for a layer, _then it uses the cache_ for that layer
* If docker identifies that there will be a change in the result for a layer, then for that layer and for the subsequent layers, docker will execute them again and not use cache
* So we can keep these points in mind to __optimize__ the execution time for _build_ command by writing our Dockerfile in such a way that layers which don't usually change are kept at top and rest at bottom

# Important Commands
1. **build**
    ```bash
    # build a image
    docker build .
    ```
2. **run**
    ```bash
    # run a image i.e. create a container
    docker run <image-id>
    ```
3. **To see all images and containers** --> 
    > There is better way to do the below activities using the image & container management commands. Check point **12**
    ```bash
    # list images
    docker images
    docker image ls
    # show all images including intermediate images
    docker image ls -a


    # list running containers
    docker ps
    docker container ls

    # list all containers
    docker ps -a
    docker container ls -a
    ```
4. **stop**
    ```bash
    # stop a container
    docker stop <container-name>
    ```
5. **start**
    ```bash
    # to re-start a container
    docker start <container-name>
    ```
6. **rm** & **rmi**
    ```bash
    # remove a single container
    docker rm <container-name>
    docker container rm <container-name>

    # remove multiple containers
    docker rm <container-name-1> <container-name-2> ... <container-name-n>
    docker container rm <container-name-1> <container-name-2> ... <container-name-n>
    
    # remove a single image
    docker rmi <image-id>/<image-tag>
    docker image rm <image-id>/<image-tag>

    # remove multiple images
    docker rmi <image-id-1> <image-id-2> ... <image-id-n>
    docker image rm <image-id-1> <image-id-2> ... <image-id-n>


    # remove container after it has been stopped (--rm)
    docker run -p 3000:80 --rm <image-id>
    ```

7. **detached and attached mode**
    > **run** starts container in attached mode by default & **start** starts container in detached mode by default. We can modify this by using **-d** & **-a** flags 
    ```bash
    # to start a container using run command in detached mode
    docker run -d <image-id>

    # to start a container using start command in attached mode
    docker start -a <container-name>
    ```
8. **attach**
    ```bash
    # to attach to detached running container
    docker attach <container-name>
    ```
9. **logs**
    ```bash
    # to see the logs generated by the container
    docker logs <container-name>

    # to see the time-stamp also
    docker logs <container-name> -t

    # to see fixed line in logs (in this example its 2 lines)
    docker logs <container-name> -n 2

    # to tail logs of a running container
    docker logs <container-name> -f
    ```
10. **See all Images and Containers**
    ```bash
    # to see all Images
    docker image ls

    # to see running containers
    docker container ls

    # to see all (running + stopped) containers
    docker container ls -a
    ```
11. **Remove Image and Container**
    ```bash
    # remove one or multiple image(s)
    docker image rm <image-id-1> ... <image-id-n>

    # remove all images
    docker image prune

    # remove one or multiple container(s)
    docker container rm <container-name-1> ... <container-name-n>

    # remove all containers
    docker container prune
    ```
12. **Interactive Mode**
    ```bash
    # run container in interactive mode
    docker run -it <image-id>

    # start container in interactive mode
    docker start <container-name> -a -i
    ```

13. **inspect** & **history**
    ```bash
    docker image inspect <image-id>
    docker image history <image-id>
    ```
14. **cp**
    ```bash
    # copy files to container
    docker container cp <src-path> <container-name:dest-path>

    # copy files from container
    docker container cp <container-name:dest-path> <src-path>    
    ```
15. **naming/re-naming container** & **tagging images**
    ```bash
    # naming a container
    docker run --name <some-name> <image-id>/<image-tag>

    # re-naming a container
    docker container rename <old-name> <new-name>

    # tagging image
    docker build -t <name:tag> .
    
    # using image manager
    docker image tag <old-image-tag> <new-image-tag>
    ```
# PUSH & PULL

* **PUSH :** We can push our images to Docker Hub or other repos using _push_ command
    * **Steps :**
        1. Login to docker hub from console
            ```bash
            docker login
            Username: <enter-username>
            Password: <enter-password>
            ```
        2. Login to docker hub from browser and create a repo
        3. In your terminal, tag your image with the name of your repo
        4. Enter in your console
            ```bash
            docker push <image-tag>
            ```
* **PULL :** We can pull our images from Docker Hub or official images from Docker Hub using _pull_ command
    * **Steps :**
       1. Enter in your console
            ```bash
            docker pull <image-tag>/<repo-name>
            ```
# VOLUMES

### Types:
1. **Anonymous Volumes :** Gets deleted as soon as container is deleted
    ```bash
    # via Dockerfile
    FROM node
    WORKDIR /app
    COPY . /app
    RUN npm install
    EXPOSE 80
    # below specify the path of directory for which you need a volume
    VOLUME ["/app/feedback"]
    CMD ["node", "server.js"]

    # or via run command
    docker run -p 3000:80 -d --rm -v <target-dir-path> <image-tag> 
    ```
2. **Named Volume :** Persist even after a container is deleted. We need to manually remove a volume
    ```bash
    # when runnig container, specify the named volume
    docker run -p 3000:80 -d --rm -v <volume-name>:<target-dir-path> <image-tag>
    ```
3. **Bind Mounts/Volume :**
    ```bash
    # when runnig container, specify the volume
    docker run -p 3000:80 -d --rm -v <host-path>:<target-dir-path> <image-tag>
    ```
4. **Setting Permission on Volumes**\
    By default, volumes have read + write permission i.e. they can read aswell as write data on the host machine

    ```bash
    # we can change the volume from default read + write mode to only readonly by writing :ro after volume declaration
    docker run -p 3000:80 -d --rm -v <host-path>:<target-dir-path>:ro <image-tag>
    ```
### Managing Volume:
```bash
    docker volume --help

    :'
    Usage:  docker volume COMMAND

    Commands:
    create      Create a volume
    inspect     Display detailed information on one or more volumes
    ls          List volumes
    prune       Remove all unused local volumes
    rm          Remove one or more volumes
    '
```
# .dockerignore
A **.dockerignore** file in concept is similar to a **.gitignore** file. We can mention any files and folders that we would want to ignore while **COPY** command in our **Dockerfile** gets executed

Sample .dockerignore
```bash
Dockerfile  # to ignore Dockerfile while copying
.git        # to ignore .git directory while copying
```

# Environment Variable
Sample Dockerfile
```bash
FROM node:14
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
# setting a default port number which we can change in run command
ENV PORT 80
EXPOSE $PORT
CMD [ "npm", "start" ]
```
In order to use environment variable, we use **`run`** command as below
```bash
docker run -p 3000:8000 --env PORT=8000
# or
docker run -p 3000:8000 -e PORT=8000
```
Using a .env file. Sample .env file below
```bash
# .env file
PORT=8000

# run command in this case
docker run -p 3000:8000 --env-file <path-to-env-file>
```

# Arguments
Sample Dockerfile
```bash
FROM node:14
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
ARG DEFAULT_PORT=80
ENV PORT $DEFAULT_PORT
EXPOSE $PORT
CMD [ "npm", "start" ]
```
In order to use arguments, we use **`build`** command as below
```bash
docker build -t <image-tag> --build-arg DEFAULT_PORT=8000 .
```
# Network
1. Outbound HTTP request from a docker container happens by default.
2. For communication with services running on localhost, we need to use **`host.docker.internal`** in place of **`localhost`**
3. For cross container communication:
    * We can hard code the ip of the container to which we want to communicate. To get the ip, we can run **`docker container inspect <cotainer-name>`** and check the IP field.
    * Another way is to create a **docker network** using **`docker network create <network-name>`** and put every container in the same network by running the run command like **`docker run --network/--net <network-name> <image-tag>`**
        ```bash
        docker network --help

        :'
        Usage:  docker network COMMAND

        Commands:
        connect     Connect a container to a network
        create      Create a network
        disconnect  Disconnect a container from a network
        inspect     Display detailed information on one or more networks
        ls          List networks
        prune       Remove all unused networks
        rm          Remove one or more networks
        '
        ```
4. **Network Drivers**
    * **`bridge`** (default)
    * **`host`**  (use host machine networking)
    * **`overlay`** (multi-host communication)
    * **`macvlan`** (allows assigning mac-address to a container)
    * **`none`**  (no networking)
    * **`Network plugins`** (third-party plugin)

    Refer to [Docker Networks](https://docs.docker.com/network/) to learn more on drivers.
