# Docker 101

This will be a simple outline for things to cover when teaching a beginner how to
use Docker. This will assume the student has Docker already installed and configured. 

*Requirements*
- Docker

## Hello World
*This is more of a test that `Docker` is configured correctly than anything else.*
- Run the container: `docker run hello-world`
- See the latest created container: `docker ps -l`
- See running containers: `docker ps`
- See all local docker images: `docker image ls`

## Docker Run
- Pull the Ubuntu 18.04 image: `docker pull ubuntu:18.04` *Note: See [Dockerhub](https://hub.docker.com/_/ubuntu?tab=tags) for other versions of Ubuntu*
- Run a program from a Ubuntu container. `docker run --rm ubuntu cat /etc/os-release` *Note: --rm will remove the container when done*
- Run a Ubuntu container: `docker run -it --name my-ubuntu -v $(pwd):/example --env EXAMPLE_ENV=Hello -p 8080:8080 ubuntu bash`
- Start a stopped container: `docker start my-ubuntu`
- Get a shell inside a running container: `docker exec -it my-ubuntu /bin/bash`

## Dockerfile
*[Dockerfile](file:./Dockerfile)*
```docker
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP app.py
ENV FLASK_RUN_HOST 0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
COPY ./HelloFlask.py ./app.py
CMD ["flask", "run"]
```

- Build a Docker image from a Dockerfile: `docker build -t my-awesome-image .` 
- Each `RUN` command is a new layer. *More `RUN` commands `==` **BIGGER** image size!*
- `ARG` are build time environment variables not accessible at runtime. 
- `ENV` are environment variables accessible at both build time and runtime. 

## Container Orchestration
Docker containers can be launched together with other [containers](containers) to form something called a Docker Stack. All containers
in a stack will share the same network by default. This allows for things like a database, an api, and a front-end 
to be started, each in their own container. 

```
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```


## Miscellaneous 
- [Portainer](https://www.portainer.io/): Portainer makes managing multiple containers pretty simply by providing a nice web interface to Docker. 
- [Traefik](https://containo.us/traefik/): Remembering ports is a pain, `myapp.host.lan` is much easier! [docker-compose.yml](file:./docker-compose.traefik.yml)

