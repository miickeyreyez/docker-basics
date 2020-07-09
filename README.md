# Docker

- Virtualización basada en contenedores
- Un kernel para todos los contenedores
- Virtualización a nivel S.O.
- Aislan entornos de ejecución (tiempo de ejecución)
- Portabilidad
- Arquitectura cliente/servidor a través de docker daemon


# Commands
- docker images
- docker run image cmd
- docker ps -a
- docker run --rm image sleep miliseconds
- docker run --name docker_name image
- docker inspect container_id # Información del registro
- docker run -p host_port : container_port
- docker logs container_id

# Docker layers
- docker history
- Base layer is only read mode, the attached layers are editable layers, a set of containers can share the same base.

# Create a custom image
- Git
  - Start the base image
  - Install git into the image
  - docker commit container_id repository:tag

# Dockerfile
  - Create a DockerFile
    ```
    FROM debian:jessie
    RUN echo "Hello world"
    CMD ["echo", "helllo world"]
    COPY origin_path docker_path
    ADD 
    ```
-  ADD: let us download internet files, and unzip compressed files  
 - docker build -t tagged_image .
 - docker build -t tagged_image . -f path
 - Docker executes each command in a new container (new image layer), it's better to nest intstructions

# Docker cache
- If the instruction is the same, use docker cache to build faster
- to avoid cache: docker build -t image . --no-cache=true

# Uplaoding docker images
- docker tag container_id tag
- docker login --username=user
- docker push image_name:tag

# Links
- Let us share information/resources between containers
- docker run -p port:port --link another_container container
- docker inspect container_id | grep IP
- Advantages:
  - Microservices approach
  - Secure tunnel is created between contianers, not exposing externally any port

# Docker compose
- Link multiple containers, aoviding manually linking
```
version: '3'
services:
  dockerapp:
   build: .
   ports:
    - "5000":"5000" # host:container
   depends_on:
     - redis
  redis:
   image: redis:latest
```
- docker-compose up
- docker-compose ps
- docker-compose logs 
- docker-compose -f # output register
- docker-compose logs container_name
- docker-compose stop
- docker-compose rm
- docker-compose build

# Networks
- Interface: docker0
- docker network ls
- docker network inspect bridge
- --net net_type || net_name
- None network
  - Without internet (isolated network)
  - Loopback interface (only localhost)
  - Protection max level
- Bridge network
  - Default network
  - Loopback and ethernet interfaces
  - Bridge network cannot access to another bridge network
  - create network: docker network create --driver bridge bridge_network
  - To connect with another network: docker network connect bridge container_name
  - docker network disconnect bridge container_name
  - Improve external connectivity
- Host network
  - Less protected network model
  - Open contianer
  - --net host
  - All interfaces are available
  - Less security level
  - Performance improves
- Overlay network
  - Host distributed network
  - Docker engine must be running in Swarm model
  - Key/value storage service is required
- Docker Compose
  - Default is a bridge network
  ```
    services:
      docker_app:
        ..,
        networks:
          - my_net
    networks:
      my_net:
        driver: bridge
  ```

# Unit tests
  - Test environment
  - docker-compose run service_name python test.py
  - isolated environments, built on demand

# Docker in production
  - Scalability
  - AWS/Google use Virtual Machines internally
  - Docker Machine creates virtual machines
  - docker-machine create
  - docker-machine ls 
  - Docker swarm: Group some docker engines (Cluster)
  - Admin has sevelar workstations (nodes)
