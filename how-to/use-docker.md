# Use Docker

Based on Ubuntu.

## Install

curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

> ### How to fix docker: Got permission denied issue
> A. Use sudo
> B. Without sudo: 
>
> sudo groupadd docker 
> sudo usermod -aG docker $USER
> newgrp docker
>
> For more details: [ref](https://stackoverflow.com/questions/48957195/how-to-fix-docker-got-permission-denied-issue)

### Uninstall

```bash
sudo apt-get purge docker-ce
sudo rm -rf /var/lib/docker
```

## Hello World
 
```bash
docker run ubuntu:15.10 /bin/echo "Hello world"
```

Run a container generated from the image of *ubuntu:15.10*, and excute `/bin/echo "Hello world"` in this container.

```bash
docker run -i -t ubuntu:15.10 /bin/bash
docker run -it ubuntu:15.10 /bin/bash
exit
```

Use `-it` to run this container in a new terminal. Use `exit` to exit.
 
## Use Containers

```bash
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
```

Run a container in the background.

```bash
docker ps
docker logs <id or name>
```

Inspect running containers.

```bash
docker stop <id or name>
```

Stop a running container.

```bash
docker ps -a
docker restart <id or name>
docker rm -f <id or name>
docker container prune
```

Inspect all containers including stopped ones.

Restart or delete stopped containers. Use `docker container prune` to delete all stopped containers.

```bash
docker attach <id or name> 
exit
```

Enter a container and exit it with stopping it.

```bash
docker exec -it <id or name> /bin/bash 
exit
```

Enter a container and exit it WITHOUT stopping it.

## Run a Web Application 

```bash
docker pull training/webapp 
docker run -d -P training/webapp python app.py
docker run -d -p 5000:5000 training/webapp python app.py
```

Use `-P` to get a random port mapping. 

Use `-p 5000:5000` to specify a port.

Use `docker ps` to check ports. A quicker way: `docker port <id or name>`
