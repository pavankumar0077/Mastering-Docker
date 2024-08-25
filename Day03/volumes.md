# DOCKER MOUNS & VOLUMES

## BIND MOUNTS
- NONE network
- Docker Socket

![image](https://github.com/user-attachments/assets/9f74b5ea-1336-4d26-b722-04d32ac44419)

# SOCK
- Docker sock will go and retrieve the data from the host machine where Docker is running

![image](https://github.com/user-attachments/assets/045ebfd5-2b11-41b8-baa6-c4362c3253d9)

# /var/run/docker.sock
- This will show docker images and docker containers inside the docker container 

```
3. Internal Working of /var/run/docker.sock
Daemon Initialization: When the Docker daemon starts, it creates the socket file at /var/run/docker.sock. The daemon listens on this socket for incoming requests from Docker clients.

Permissions and Access Control: The socket file typically belongs to the root user and the docker group. Users who are part of the docker group can interact with the Docker daemon through this socket without requiring root privileges. This is why adding a user to the docker group allows them to run Docker commands without using sudo.

Communication Flow:

Client Sends Request: When you run a Docker command (e.g., docker run), the Docker CLI constructs an HTTP request that conforms to the Docker API. This request is then sent to the Docker daemon via the Unix socket at /var/run/docker.sock.

Daemon Processes Request: The Docker daemon receives the request and processes it. For example, if the request is to run a container, the daemon will pull the necessary image (if not already available), create the container, and start it.

Daemon Sends Response: After processing the request, the daemon sends a response back to the Docker CLI through the same socket. The CLI then displays the result to the user.

Example: If you run docker ps, the CLI sends an HTTP GET request to the /containers/json endpoint via /var/run/docker.sock. The daemon processes this request, retrieves the list of running containers, and sends the data back through the socket. The CLI then formats and displays the output.

```

# PORTAINER.IO
- Sometimes like we want to see like what are the containers and logs all the stuff related to Docker right so we all know in Ubuntu operating system when we took like any easy two instances something like that we don't have any UI for that so to get the UI we use PORTAINER.IO like this kind of things will like show all the stuff that is related to Docker and networks all this stuff related to Docker all that things so that is the only reason why we are using PORTAINER.IO here

```
docker run --rm -d --name app1 -v /var/run/docker.sock:/var/run/docker.sock --network  none kiran2361993/troubleshootingtools:v1

docker create volume create portainerdata

docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
        --restart=always \
        -v /var/run/docker.sock:/var/run/docker.sock \
        -v portainer_data:/data \
        portainer/portainer-ce:2.11.1
```
- Here as you can see like B as two volumes one is like Docker Sock which has all the access to the host mission where Docker is running and another volume is like portainer to store the portainer data like logs all that stuff.
