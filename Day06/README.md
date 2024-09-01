![6](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/f4c8a7e7-3ed4-4b26-87ff-c20586ec3727)

### Process before creating a docker image in prod
![image](https://github.com/user-attachments/assets/ff52e250-d1e9-4071-917f-115400a65017)


Google Distroless Images and Docker Multi-Stage Builds

What are Distroless Images?

Distroless images are Docker images built by Google that contain only your application and its runtime dependencies, without a package manager or any additional software. This approach minimizes the attack surface and reduces the size of your container image.

![image](https://github.com/user-attachments/assets/7a4c89fe-1e21-4a26-afac-4a9a2bc25952)

### DISTROLESS IMAGES DOES NOT HAVE SHELL OR BASH - FOR SECURITY 
### WE CAN NOT LOGIN ANS INSTALL ANY PKG'S INSIDE THE CONTAINER - we can do it by creating a volume and we can do it.

Why Use Distroless Images?

Reduced Attack Surface: Since Distroless images contain only your application and its dependencies, they eliminate unnecessary tools and libraries that could be exploited by attackers.

Smaller Image Size: By excluding unnecessary components, Distroless images are significantly smaller in size compared to traditional Linux distribution-based images. This results in faster image pulls and reduced storage costs.

https://github.com/GoogleContainerTools/distroless

Docker Multi-Stage Builds

### NOTE : We all know we use run instruction in Docker file, but one thing we need to keep in our mind is like if you have more run commands then there will be more layers while creating a Docker file and the image will also be like huge very big size image so the thing we need to keep in mind is like we need to use run command in a proper way like we can write all the required things or required instructions or required like packages that that we want to install in one single run command if possible and make it like minimal size.



Docker multi-stage builds allow you to create more efficient Dockerfiles by using multiple FROM statements. Each stage in the build process can have its own base image and set of instructions, enabling you to compile code, run tests, and package your application without including unnecessary build tools and dependencies in the final image.

### To check the layers and it size of the docker image use this command
``` docker histroy pavankumar0077/open-rfs:V2 ```
![image](https://github.com/user-attachments/assets/258ae381-2bf1-4647-ac11-caec9d9421b8)


Clone https://github.com/spring-projects/spring-petclinic.git

And follow by watching the full video here : https://youtu.be/CMO0MziP2yQ
