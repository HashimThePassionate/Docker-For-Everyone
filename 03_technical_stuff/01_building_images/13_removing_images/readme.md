# Removing Images in Docker 🐳

## Understanding Docker Images and Layers

When you run `docker images`, you see a list of images that Docker has stored on your system. Each image can have a repository name, a tag, and an ID. However, some images may appear without a name or tag—these are known as **dangling images**.

### Docker Images Command Output

```bash
# docker images
```

**Output:**

```bash
REPOSITORY                                                                         TAG       
IMAGE ID       CREATED             SIZE
react-app                                                                          latest    
7bbe58d0a548   About an hour ago   489MB
<none>                                                                             <none>    
defbc1fc22d8   About an hour ago   489MB
<none>                                                                             <none>    
343534f3dd16   2 hours ago         488MB
<none>                                                                             <none>    
2464fbc2c9e2   2 hours ago         488MB
<none>                                                                             <none>    
9902c2db5dad   2 hours ago         488MB
<none>                                                                             <none>    
1b12b148ab3c   3 hours ago         488MB
<none>                                                                             <none>    
c956abdd2bc6   3 hours ago         488MB
<none>                                                                             <none>    
eb96803fcb71   26 hours ago        481MB
<none>                                                                             <none>    
fdafe3f03a74   43 hours ago        481MB
<none>                                                                             <none>    
5b6a2f3f5f13   43 hours ago        481MB
01_docker_helloworld-web                                                           latest    
886692bd4305   45 hours ago        171MB
docker-helloworld                                                                  latest    
7dff67261a5e   45 hours ago        171MB
<none>                                                                             <none>    
892afa4b32d0   45 hours ago        171MB
alpine                                                                             latest    
324bc02ae123   2 weeks ago         7.8MB
vsc-test_poetry-e50cb80d86e2834da63bbe4e56804d21480ceae298d3d759a71e5451ac5c76e1   latest    
c610e9b4537c   3 months ago        157MB
ubuntu                                                                             latest    
bf3dc08bfed0   3 months ago        76.2MB
hello-world                                                                        latest    
d2c94e258dcb   15 months ago       13.3kB
```

### Identifying Dangling Images 🕸️

- **Dangling Images**: These are images that have no name and no tag (indicated as `<none>`). They are often the result of building an image without tagging it or when a new image is built and the old image layers are left behind without references. 
- **Tangling Images**: They have no relationship with any tagged images and can be considered "orphaned" layers. They consume space but are not directly associated with any useful image.

**Example**:
- The image with ID `defbc1fc22d8` created about an hour ago is a dangling image because it has `<none>` for both the repository and tag.

### Removing Dangling Images

To clean up these dangling images, you can use the `docker image prune` command.

```bash
docker image prune
```

**Output:**

```bash
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N] y    
Deleted Images:
deleted: sha256:892afa4b32d01d7903b164cd1d9f0c719ceecdfbdce9660d6280f2b7df19453c
deleted: sha256:defbc1fc22d8f38b661087f394cc7056ba0b0314233e174fcd6220d0395a4339
Total reclaimed space: 0B
```

### Understanding the Pruning Output

- **Deleted Images**: Docker deletes the dangling images by their SHA256 digest (e.g., `sha256:892afa4b32d01d7903b164cd1d9f0c719ceecdfbdce9660d6280f2b7df19453c`).
- **Reclaimed Space**: Shows the amount of disk space reclaimed by removing these images. In this case, it reclaimed `0B`, likely because the deleted images were just untagged layers and not taking additional space.

### Pruning Containers Associated with Tangling Images

After pruning images, you may still have containers associated with these dangling images. To remove these stopped containers, use the `docker container prune` command.

```bash
docker container prune
```

**Output:**

```bash
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
ce8141d01446ed837e81300d90ec60735ab0faaeab162a79ce45dba6773143e3
b47af4bbffa3c52feb890663461a662c799a457cc79ad1bdc2d5baf70e558ff4
...
Total reclaimed space: 437.3MB
```

### Explanation of Docker Image and Container Pruning

- **Image Pruning**: Removes all images that do not have a repository or tag associated with them, effectively cleaning up unused layers.
- **Container Pruning**: Removes all stopped containers, including those that were using the pruned dangling images.

### Running Docker Image Prune Again

Running the prune command again ensures that all remaining dangling images are removed.

```bash
docker image prune
```

**Output:**

```bash
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N] y
Deleted Images:
deleted: sha256:eb96803fcb7111db0feef85cc7010986ac849aa66b5be49782ee0aa81adf8d84
deleted: sha256:2464fbc2c9e27d4d7ebc00bfc24f966fbb603b0ceede874992ecc67c59d95d1f
...
Total reclaimed space: 0B
```

### Checking Remaining Images

After pruning, you can check which images remain using `docker images`.

```bash
docker images
```

**Output:**

```bash
REPOSITORY                                                                         TAG       
IMAGE ID       CREATED             SIZE
react-app                                                                          latest    
7bbe58d0a548   About an hour ago   489MB
01_docker_helloworld-web                                                           latest    
886692bd4305   45 hours ago        171MB
docker-helloworld                                                                  latest    
7dff67261a5e   45 hours ago        171MB
alpine                                                                             latest    
324bc02ae123   2 weeks ago         7.8MB
vsc-test_poetry-e50cb80d86e2834da63bbe4e56804d21480ceae298d3d759a71e5451ac5c76e1   latest    
c610e9b4537c   3 months ago        157MB
ubuntu                                                                             latest    
bf3dc08bfed0   3 months ago        76.2MB
hello-world                                                                        latest    
d2c94e258dcb   15 months ago       13.3kB
```

### Removing Specific Images by Name or ID

You can remove specific images by their name or image ID using `docker image rm`.

```bash
docker image rm 01_docker_helloworld-web c610e9b4537c hello-world
```

**Output:**

```bash
Untagged: 01_docker_helloworld-web:latest
Deleted: sha256:886692bd4305e441bb41d4f6ec5e45021bc4395671000c56d0af9b30afcdc81c
Untagged: vsc-test_poetry-e50cb80d86e2834da63bbe4e56804d21480ceae298d3d759a71e5451ac5c76e1:latest
Deleted: sha256:c610e9b4537c198a3b3deb22943c039f04b91befec06ef9495c539eee7a447cd
Untagged: hello-world:latest
Untagged: hello-world@sha256:1408fec50309afee38f3535383f5b09419e6dc0925bc69891e79d84cc4cdcec6
Deleted: sha256:d2c94e258dcb3c5ac2798d32e1249e42ef01cba4841c2234249495f87264ac5a
Deleted: sha256:ac28800ec8bb38d5c35b49d45a6ac4777544941199075dff8c4eb63e093aa81e
```

### Final Check of Remaining Images

After removing the specific images, check the remaining images again:

```bash
docker images
```

**Output:**

```bash
REPOSITORY          TAG       IMAGE ID

       CREATED             SIZE  
react-app           latest    7bbe58d0a548   About an hour ago   489MB 
docker-helloworld   latest    7dff67261a5e   45 hours ago        171MB 
alpine              latest    324bc02ae123   2 weeks ago         7.8MB 
ubuntu
```

### Docker Image Command Overview

The `docker image` command offers various options to manage images:

```bash
docker image
```

**Output:**

```bash
Usage:  docker image COMMAND

Manage images

Commands:
  build       Build an image from a Dockerfile
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Display detailed information on one or more images
  load        Load an image from a tar archive or STDIN
  ls          List images
  prune       Remove unused images
  pull        Download an image from a registry
  push        Upload an image to a registry
  rm          Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Run 'docker image COMMAND --help' for more information on a command.
```

### Summary 📝

- **Dangling Images**: These are images without a repository or tag, often leftover from builds.
- **Prune Commands**: `docker image prune` and `docker container prune` help clean up unused images and containers.
- **Image Removal**: You can remove specific images by name or ID using `docker image rm`.
- **Best Practices**: Regularly prune and manage your images to keep your Docker environment clean and efficient.
