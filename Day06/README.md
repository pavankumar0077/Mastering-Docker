![6](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/f4c8a7e7-3ed4-4b26-87ff-c20586ec3727)

### Process before creating a docker image in prod
![image](https://github.com/user-attachments/assets/ff52e250-d1e9-4071-917f-115400a65017)


Google Distroless Images and Docker Multi-Stage Builds

What are Distroless Images?

Distroless images are Docker images built by Google that contain only your application and its runtime dependencies, without a package manager or any additional software. This approach minimizes the attack surface and reduces the size of your container image.

Why Use Distroless Images?

Reduced Attack Surface: Since Distroless images contain only your application and its dependencies, they eliminate unnecessary tools and libraries that could be exploited by attackers.

Smaller Image Size: By excluding unnecessary components, Distroless images are significantly smaller in size compared to traditional Linux distribution-based images. This results in faster image pulls and reduced storage costs.

https://github.com/GoogleContainerTools/distroless

Docker Multi-Stage Builds

Docker multi-stage builds allow you to create more efficient Dockerfiles by using multiple FROM statements. Each stage in the build process can have its own base image and set of instructions, enabling you to compile code, run tests, and package your application without including unnecessary build tools and dependencies in the final image.

Clone https://github.com/spring-projects/spring-petclinic.git

And follow by watching the full video here : https://youtu.be/CMO0MziP2yQ
