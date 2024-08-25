## Dangling Images in Docker

```
Dangling images are a type of Docker image that is not associated with any tag. They often appear after building or pulling images when old versions of images are replaced with newer ones. These dangling images take up space on the system but are not used by any containers.

How Dangling Images Are Created:
When you build a Docker image, each build layer is stored as an image layer. Over time, as you rebuild images with updated code or dependencies, some of the older layers may no longer be needed, but they still remain on your system.
If you pull a new version of an image from a repository, the older version might become "dangling."

Identifying Dangling Images:
You can identify dangling images using the following Docker command:

```
```
docker images -f "dangling=true"
```

This will list all images that are not tagged or associated with any containers.

Cleaning Up Dangling Images:
To remove dangling images and free up space, you can run:

```
docker image prune
```

![image](https://github.com/user-attachments/assets/b97a0326-0695-44bb-a9f4-fd3e82f129dd)


