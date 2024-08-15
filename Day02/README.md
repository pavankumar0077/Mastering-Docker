![02](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/7d18cf0c-2c71-4d4a-bff9-aed6fab5d8d7)

# Why We Need Separate Utilization for Docker Data
The default directory for Docker is /var/lib/docker. As you continue downloading images and generating logs, this directory will consume more space and eventually get busy. To prevent this, we can store all our Docker data in a separate directory.

Default storage of docker
--
```
cd /var/lib/docker/containers
```
- In general if we store the docker images in the ec2 instance then root volume of that instance will be increased.
- INSTEAD WE CAN CREATE A NEW VOLUME AND ATTACH THAT VOLUME TO THE EC2 INSTANCE.
- ![image](https://github.com/user-attachments/assets/508c0c53-a742-4bf2-8672-14c4c57f916b)
- ![image](https://github.com/user-attachments/assets/88d7a7f1-beb2-48d9-9a66-5af5d3e90939)
- ![image](https://github.com/user-attachments/assets/67538b64-18c0-45c4-ba67-3264e7fb0a79)

Format the newly attached volume
--
```
lsblk
```
![image](https://github.com/user-attachments/assets/49a43738-2610-4768-bdc4-4c801211bc15)

```
fdisk /dev/xvdf
```
- Select p, Primary
- then we to save
- ![image](https://github.com/user-attachments/assets/9d45fc23-d488-4482-ae71-06344a4611b3)
- again check ``` lsblk ```

# Now add a file system for it
```
lsblk
```
- file system
```
```
mkfs.ext4 /dev/xvdf1
```
![image](https://github.com/user-attachments/assets/5ebc429e-f998-4524-b4be-2db8624f4128)
- COPY UUID
```

# Now create a directory
```
mkdir dockerdata
```

- Open FSTAB
```
sudo nano /etc/fstab
```
- Add UUICD and dirname and filesytem extension
![image](https://github.com/user-attachments/assets/3ba17736-b40e-47b5-8eaa-7cd34130098d)

- Mount it
```
mount -a
```

# Check dh -h
```
df -h
```

# Let's test files are saving or not

![image](https://github.com/user-attachments/assets/c4fd2213-44ff-4156-a30d-45e9944cbc8f)


![image](https://github.com/user-attachments/assets/71d3268b-4f70-4adb-94a5-45547defc72d)

# Now change the server dir as well

- Day02/dockerServiceDir-Change


## IMP STUFF
- IN REALTIME WE ALL KNOW THAT DOCKER CONTAINER WILL NOT HAVE Troubleshooting tools like ifconfig, net-tools and etc. SO THERE WILL BE ANOTHER DOCKER IMAGE WE USE THAT IMAGE AND DO THE STUFF 

# Steps To Create EBS Volume and Attaching it to the instance
Create EBS volume GP2 and atatch it to Instance

lsblk 

fdisk /dev/xvdf  

n > p > w > 

lsblk 

mkfs.ext4 /dev/xvdf1 

copy UUID Somewhere 

mkdir /dockerdata 

nano /etc/fstab 

Paste Like below

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/1ad08bf8-593e-4579-921c-0f7d8938c8ee)

# Why We Need a Custom Network for Containers

With the default bridge network, containers can communicate with each other using IP addresses but not with container names. To enable communication using container names, we need to create a custom network.

First create a customer network 

Docker network create myapp --driver bridge

Docker network inspect myapp

Now run containers with including the custom network

docker run –rm -d –name app3 -p 8001 –network myapp  kiran2361993:troubleshootingtools:v1

docker run –rm -d –name app4 -p 8002 –network myapp  kiran2361993:troubleshootingtools:v1

If you login to the container with docker exec -it containername bash and you can ping with the container name instead of IP, It should work. 

# Using the HOST Network Mode

The HOST network mode means the container shares the host's IP address. This eliminates the need for port forwarding when running the container. For example, when using Prometheus with Node Exporter, it utilizes the host IP for metric collection, making it easier to access the metrics.

Example Command

docker run --rm -d --name node-exporter --network host prom/node-exporter

You can access the Node Exporter from the host's public IP on port 9100. To inspect the Docker image and understand the default settings, use the following command:

docker image inspect prom/node-exporter:latest

# Using the NONE Network Mode

The NONE network mode means the container has no network interfaces enabled except for a loopback device. This is useful for highly isolated containers that do not require any network access.

Example Command

docker run --rm -d --name isolated-container --network none busybox

This command runs a BusyBox container with no network connectivity, providing complete network isolation.



