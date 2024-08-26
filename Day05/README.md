![05](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/dbcf4ade-75da-4556-8d01-78a00427f691)



# Docker Commands Explained

Welcome to this GitHub repository! This guide will help you understand some basic Docker commands used in a Dockerfile. Let's break down the commands used in the Dockerfile for our project.

## Dockerfile Overview

Here's the Dockerfile we are using:

```dockerfile
FROM ubuntu:latest
LABEL name="saikiran"
RUN mkdir /app
RUN groupadd appuser && useradd -r -g appuser appuser 
WORKDIR /app 
USER  appuser
ENV AWS_ACCESS_KEY_ID=DUMMYKEY \
    AWS_SECRET_KEY_ID=DUMMYKEY \
    AWS_DEFAULT_REGION=US-EAST-1A
COPY index.nginx-debian.html /var/www/html/
COPY scorekeeper.js /var/www/html
ADD  style.css /var/www/html
ADD https://releases.hashicorp.com/terraform/1.9.0/terraform_1.9.0_linux_amd64.zip /var/www/html
ARG T_VERSION='1.6.6' \
    P_VERSION='1.8.0'
EXPOSE 80
RUN apt update && apt install -y jq net-tools curl wget unzip \
    && apt install -y nginx iputils-ping 
RUN wget https://releases.hashicorp.com/terraform/${T_VERSION}/terraform_${T_VERSION}_linux_amd64.zip \
    && wget https://releases.hashicorp.com/packer/${P_VERSION}/packer_${P_VERSION}_linux_amd64.zip \
    && unzip terraform_${T_VERSION}_linux_amd64.zip && unzip packer_${P_VERSION}_linux_amd64.zip \
    && chmod 777 terraform && chmod 777 packer \
    && ./terraform version && ./packer version 
USER  appuser
CMD ["nginx","-g","daemon off;"]
```

### Explanation of Docker Commands

#### 1. `FROM`

```dockerfile
FROM ubuntu:latest
```

This command sets the base image for the Docker image. Here, we are using the latest version of Ubuntu.

#### 2. `LABEL`

```dockerfile
LABEL name="saikiran"
```

This command adds metadata to the image. In this case, we are labeling the image with a name.

#### 3. `RUN`

```dockerfile
RUN mkdir /app
RUN groupadd appuser && useradd -r -g appuser appuser 
```

These commands execute commands inside the image. The first command creates a directory named `/app`. The second command creates a new group and a new user.

#### 4. `WORKDIR`

```dockerfile
WORKDIR /app 
```

This command sets the working directory for the subsequent instructions. Here, it sets `/app` as the working directory.

#### 5. `USER`

```dockerfile
USER  appuser
```

This command sets the user for the subsequent instructions. Here, it sets `appuser` as the user.

#### 6. `ENV`

```dockerfile
ENV AWS_ACCESS_KEY_ID=DUMMYKEY \
    AWS_SECRET_KEY_ID=DUMMYKEY \
    AWS_DEFAULT_REGION=US-EAST-1A
```

This command sets environment variables inside the container.

#### 7. `COPY`

```dockerfile
COPY index.nginx-debian.html /var/www/html/
COPY scorekeeper.js /var/www/html
```

These commands copy files from the host machine to the container. The first command copies `index.nginx-debian.html` to `/var/www/html/`, and the second copies `scorekeeper.js` to the same directory.

#### 8. `ADD`

```dockerfile
ADD  style.css /var/www/html
ADD https://releases.hashicorp.com/terraform/1.9.0/terraform_1.9.0_linux_amd64.zip /var/www/html
```

These commands add files from the host machine or a URL to the container. The first command adds `style.css` to `/var/www/html`, and the second downloads and adds a file from a URL.

#### 9. `ARG`

```dockerfile
ARG T_VERSION='1.6.6' \
    P_VERSION='1.8.0'
```

This command defines variables that users can pass at build-time to the builder with the `docker build` command.

#### 10. `EXPOSE`

```dockerfile
EXPOSE 80
```

This command informs Docker that the container will listen on the specified network ports at runtime. Here, it exposes port 80.

#### 11. `CMD`

```dockerfile
CMD ["nginx","-g","daemon off;"]
```


CMD EXAMPLE
--
Ex:
```
CMD ["/usr/bin/ping","-c 4","www.google.com"]
```

This command provides the default command to run when the container starts. Here, it starts the `nginx` server in the foreground.

![image](https://github.com/user-attachments/assets/14cd5ddb-ab7c-4fd4-bd59-6d7e422afb33)
- Here as you can see like in the Docker file we have mentioned ping - C4 www.google.com
```
docker run kiran236/cmd:v1 ping www.youtube.com
```
- As you can see we have highlighted something and we have given the same cmd command by overriding while running the container with www .youtube.com and we didnt mention any - C over there so you can see the result in the ever screenshot

ENTRYPOINT
--
```
ENTRYPOINT ["/usr/bin/ping","-c 4","www.google.com"]
```
- This will work as usual let CMD but the main difference is like CMD weekend override while running the container but entry point we cannot override.
- ![image](https://github.com/user-attachments/assets/735f95a4-fe50-456a-ba7d-fdd3d68669c2)


## NOTE :
- In organizations they follow two types of approaches the thing is like first they'll keep the entry point and then they'll keep the cmd part in the entry point they will give the fixed values that should not be changed but in the cmd they'll give values that can be overridden:

```
ENTRYPOINT ["/usr/bin/ping","-c4"]

CMD ["www.google.com"]
```

- Like as I mentioned above we can change the CMD value here is an example for that
- ![image](https://github.com/user-attachments/assets/564e47dc-84cd-422f-a585-0f4295fa16e3)

```
docker run --rm --entrypoint ping pavankumar0077/custom:v5 -c 4 www.google.com
```

## Difference between entry point and C M D
```
Purpose:

ENTRYPOINT: Defines the main executable that should always run inside the container. It’s intended to be the primary command for the container.
CMD: Provides default arguments to the ENTRYPOINT. If no arguments are provided at runtime, Docker uses CMD. If an ENTRYPOINT is not specified, CMD can act as the main command.
Override Behavior:

CMD: Easily overridden by providing a command at runtime (e.g., docker run myimage command). This replaces the CMD in the Dockerfile.
ENTRYPOINT: Overridden only by using the --entrypoint flag. If overridden, CMD is ignored unless explicitly passed as an argument to the new ENTRYPOINT.
Usage Pattern:

ENTRYPOINT is for specifying the main action the container should perform (e.g., running a script or an application).
CMD is for providing default arguments to the main action or command, which can be overridden.
```
```
Real-Time Scenarios
1. Microservices Deployment
ENTRYPOINT: Set to the microservice’s executable (e.g., /usr/bin/myservice).

CMD: Provide default environment configurations (e.g., --env=production).

Example: The microservice container always starts with the service executable, but you can override environment configurations using CMD at runtime.

2. Database Containers
ENTRYPOINT: Set to the database startup command (e.g., postgres or mysqld).

CMD: Provide default database initialization arguments (e.g., --init-file=/init.sql).

Example: The database always starts, but you can override initialization scripts or settings.
```
```
Example 1: Using CMD and ENTRYPOINT Together
Dockerfile Example:
Dockerfile

# Base image
FROM ubuntu:latest

# Set ENTRYPOINT to always run "ping"
ENTRYPOINT ["ping"]

# Set CMD to provide default arguments (e.g., "-c 4 www.google.com")
CMD ["-c", "4", "www.google.com"]
Scenario 1: Running the Container without Override

docker run pavankumar0077/custom:v5
Explanation:

ENTRYPOINT is ping.
CMD is -c 4 www.google.com.
Result:
The container runs ping -c 4 www.google.com. The CMD provides the default arguments to the ENTRYPOINT.

Scenario 2: Overriding CMD at Runtime

docker run pavankumar0077/custom:v5 www.example.com
Explanation:

ENTRYPOINT is still ping.
CMD is overridden with www.example.com.
Result:
The container runs ping www.example.com. The runtime argument www.example.com replaces the default CMD.

Scenario 3: Overriding ENTRYPOINT at Runtime

docker run --entrypoint ls pavankumar0077/custom:v5
Explanation:

ENTRYPOINT is overridden with ls.
CMD (-c 4 www.google.com) is ignored.
Result:
The container runs ls, ignoring both ENTRYPOINT and CMD.

Example 2: ENTRYPOINT with Hard-Coded Commands
Dockerfile Example:
Dockerfile

# Base image
FROM ubuntu:latest

# Set ENTRYPOINT to a fixed command (e.g., starting a web server)
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

# Set CMD to a configuration flag (e.g., a config file)
CMD ["-f", "/etc/apache2/apache2.conf"]
Scenario 1: Default Behavior

docker run myapacheimage
Result:
The container runs /usr/sbin/apache2ctl -D FOREGROUND -f /etc/apache2/apache2.conf.

Scenario 2: Overriding CMD

docker run myapacheimage -f /custom/path/apache2.conf
Result:
The container runs /usr/sbin/apache2ctl -D FOREGROUND -f /custom/path/apache2.conf.

Scenario 3: Overriding ENTRYPOINT

docker run --entrypoint bash myapacheimage
Result:
The container starts a bash shell, ignoring the Apache entry point.
```

## RESTRICTING CONTAINER WITH ROOT ACCESS
- 


### Building and Running the Docker Image

1. **Build the Docker Image:**

   ```bash
   docker build -t my-app .
   ```

2. **Run the Docker Container:**

   ```bash
   docker run -p 80:80 my-app
   ```

Now, your application should be up and running on port 80!

For a detailed explanation, you can watch the video on YouTube: [Docker Commands Explained](https://youtu.be/orQV059Zu8E?si=z1EHwd2s8p-q8jFL).
