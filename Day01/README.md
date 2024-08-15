

Hardware Components:
CPU: Central Processing Unit, the brain of the computer.
RAM: Random Access Memory, temporary storage for data being used.
HD: Hard Drive, permanent storage for data.
Graphic Cards: Hardware for rendering images and video.

Software Components:
Operating System: The main software that manages hardware and software resources.
Applications: Software you use, such as Zen Recorder for recording videos or games.

The Kernel:
The kernel is the core part of an operating system. It acts as a bridge between software and hardware, converting requests into instructions the hardware can understand.

Container Runtimes
There are several runtimes for running containers, including:
Container-D
Docker
CRI-O

# Note: In production environments, Container-D is typically used. Developers often use Docker for local testing. For example, KIND (Kubernetes IN Docker) is used to create Kubernetes clusters in Docker for testing.


# Container vs Virtual machines 

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/980faa67-603b-46d5-bb0b-83d40a22de08)

Virtual Machines (VMs) are like having a complete, separate computer within your computer. Each VM runs its own full operating system, like having another Windows or Linux inside your main system. Because they need to load an entire OS, VMs are big and take a while to start. They are very isolated from each other, making them secure, as each VM operates independently without knowing about the others.

Containers, on the other hand, are like small, self-contained environments inside your computer. They share the main computer’s operating system but have their own space for running applications. Containers are much smaller and start very quickly since they don’t need a full OS. They offer enough isolation to keep applications separate but are less isolated than VMs because they share the same OS.

----

# Docker Architecture 

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/c3b01248-cf68-49d0-86eb-3aea429b986d)

Docker Client :
```
What it is: The Docker client is like the command center for Docker. It’s where you type in commands to tell Docker what to do.
How it works: You use the Docker client to build, run, and manage Docker containers. It talks to the Docker daemon, which does the actual work.
Example commands: docker build, docker run, docker pull.
```

Docker Hub :
```
What it is: Docker Hub is an online service where you can find and store Docker images.
How it works: It’s like an app store but for Docker images. You can download (“pull”) images others have created, or upload (“push”) your own images.
Usage: When you need an image to create a container, you can pull it from Docker Hub.
```

Docker Registry :
```
What it is: A Docker registry is a place to store Docker images. Docker Hub is the most popular public registry, but you can also have private registries.
How it works: Registries store the images you create and make them available for you to pull and run on your Docker client. Private registries are used for images you don’t want to share publicly.
Example: Companies often use private registries to store their internal application images securely.
```

# Installing Docker & Network changes 

curl https://get.docker.com/ | bash 

Netwrok : Bridge - Host - none

Bridge : A Docker bridge network is a private internal network created by Docker on the host machine. It's used to allow containers to communicate with each other within the same host.

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/09f22c6c-743a-480e-8e87-7156d090c65d)

# Find process like docker deamon is running in the background

![image](https://github.com/user-attachments/assets/9f226a13-963a-4f0d-8502-6dc36fedbce5)

```
ps -ef | grep  -i '?'

ps -ef

- Here "?" represents running as a DEAMON
- pts/1 is running a a user for ps -ef
```
```
ping -n 100 www.google.com &
- This will running as background process under user.
```

# Namespaces
```
lsns
```
- List pid
```
lsns -t pid
```
![image](https://github.com/user-attachments/assets/ba765457-b18b-49d9-a374-32af93fbbddd)

### NOTE : Each process has it own NAMESPACE. 

# Run container like 10 or 20 at a time 
--
```
for I in {1..10}; do
docker run -d nginx:latest
done
```
![image](https://github.com/user-attachments/assets/7eca7e5d-f93c-48d5-89b0-bd9884d6c943)

# Stop all the container
```
docker stop $(docker ps -aq)
```

# delete all container
```
docker rm $(docker ps -aq)
```

# Delete entire container even we stop it
```
docker run --rm -d --name app1 nginx:latest
```

# PORT FORTFORWARDING

![image](https://github.com/user-attachments/assets/aabedbf6-db50-43ab-8c92-3f5c23017d95)

- Here there are 3 contianers running nginx server and all are running in the same port,
- Dev 1 wants to use first conatainer and same for dev2 and dev3 as well.
- So we can use like 8000:80 - host port : docker-container port and same for rest of the 2 containers
```
docker run --rm -d --name frontend -p 8000:80 nginx:latest

docker run --rm -d --name frontend -p 8001:80 nginx:latest

docker run --rm -d --name frontend -p 8002:80 nginx:latest
```

# Check LOGS OF THE CONTAINER IN PROD-ENV
```
cd /var/lib/docker/containers/container-id

cat conatiner-id.log | jq
```
- Here "jq" will represent in the formatted way

# Listing namespaces & Running basic docker commands 

Basic Docker Commands
==
```

Check Docker version: docker version

List namespaces: lsns

List PID namespaces: lsns -t pid

Run a container: docker run --name app1 nginx:latest

Run a container in the background: docker -d run --name app1 nginx:latest

Create multiple containers: for i in {1..10}; do docker run -d nginx:latest; done

List running containers: docker ps

Stop all containers: docker stop $(docker ps -aq)

Deploy and remove containers: docker run --rm -d --name app1 nginx:latest

Inspect a container: docker inspect app1
```
Port forwarding: docker run --rm -d --name app1 -p 8000:80 nginx:latest

View logs: docker logs app1 -f


   
