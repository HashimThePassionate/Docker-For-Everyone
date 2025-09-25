# 🚀 Sharing Images with Docker Hub

## Step 1: 🆓 Creating a Free Account on Docker Hub

1. **Sign Up**: Go to [Docker Hub](https://hub.docker.com/) and sign up for a free account. You can sign up using Google or other available methods.
   - **In your case**: You signed up using Google and created the username `muhammadhashim24`.

2. **Explore Docker Hub**: After signing up, you’ll be directed to the Docker Hub dashboard. Here, you can explore different public images or manage your own repositories.

## Step 2: 🗂️ Creating a New Repository

1. **Click on New Repository**: On the Docker Hub dashboard, click the **"New Repository"** button to create a new repository.

2. **Repository Details**:
   - **Username**: Select your username, which is `hashimthepassionate` in your case.
   - **Repository Name**: Name your repository. For this example, let's name it **`react-app`**.
   - **Description**: (Optional) You can add a description for your repository.
   - **Create**: Once you’ve filled in the details, click the **"Create"** button.

3. **Repository Created**:
   - Your new repository will now be created, and you will see the following details:

   ```text
   muhammadhashim24/react-app
   Created 1 minute ago
   This repository does not have a description
   INCOMPLETE
   This repository does not have a category
   INCOMPLETE
   ```

## Step 3: 🛠️ Docker Commands for Pushing Images

After creating the repository, Docker Hub will provide you with commands to push your images to this repository. For example:

```text
Docker commands
To push a new tag to this repository:

docker push hashimthepassionate/react-app:tagname
Tags
INCOMPLETE
This repository is empty. Push some images to it to see them appear here.
```

## Step 4: 🔍 Checking Available Images

Before pushing, let’s check the available images on your local machine:

```bash
docker images
```

**Output:**

```bash
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE  
react-app    latest    863701cc7202   About a minute ago   489MB 
ubuntu       latest    35a88802559d   2 months ago         78.1MB
```

## Step 5: 🏷️ Tagging the Image

To push the image to Docker Hub, you need to tag it with your Docker Hub username and repository name:

```bash
docker tag react-app muhammadhashim24/react-app:1
```

**Output:**

```bash
docker images
```

**Output:**

```bash
REPOSITORY                      TAG       IMAGE ID       CREATED          SIZE 
react-app                       latest    863701cc7202   25 minutes ago   489MB
muhammadhashim24/react-app      1         863701cc7202   25 minutes ago   489MB
ubuntu                          latest    35a88802559d   2 months ago     78.1MB
```

Now the image **`react-app`** has been tagged as **`muhammadhashim24/react-app:1`**.

## Step 6: 🔐 Logging into Docker Hub

Before pushing the image to Docker Hub, you need to log in:

```bash
docker login
```

**Prompt:**

```text
Log in with your Docker ID or email address to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com/ to create one.
You can log in with your password or a Personal Access Token (PAT). Using a limited-scope PAT grants better security and is required for organizations using SSO. Learn more at https://docs.docker.com/go/access-tokens/ 

Username: muhammadhashim24
Password: 
Login Succeeded
```

## Step 7: 📤 Pushing the Image to Docker Hub

Now, push the tagged image to your Docker Hub repository:

```bash
docker push muhammadhashim24/react-app:1
```

**Output:**

```text
The push refers to repository [docker.io/muhammadhashim24/react-app]
aae5acb0fcaa: Pushed
a8b4ee0b8518: Pushed
cddaa97fa3de: Pushed
8d7831c063e6: Pushed
7bebba704faf: Pushed
e95e47def415: Mounted from library/node
982ab52e89c8: Mounted from library/node
f1c732704abe: Mounted from library/node
b2d5eeeaba3a: Mounted from library/node
1: digest: sha256:4432bf44001866d57ebb9efb8d84ea694c2f9ee33e385f2725d346a4dcf1ff5a size: 2206
```

The image has now been successfully pushed to Docker Hub under the tag **`react-app:1`**.

## Step 8: ✏️ Making Small Changes and Rebuilding

Now, let’s say you make a small change in your project (e.g., updating the README file). After making the change, you can rebuild the image, give it a new tag, and push it again. This time, the push will be faster since Docker only pushes the layers that have changed.

## 📝 Verifying the New Tagging and Pushing to Docker Hub

After making small changes and creating a new tag, let's check the images and push the updated image to Docker Hub.

### 🔍 Listing the Docker Images

```bash
docker images
```

**Output:**

```bash
REPOSITORY                      TAG       IMAGE ID       CREATED          SIZE
muhammadhashim24/react-app      2         135560fe1d7e   16 seconds ago   489MB
react-app                       latest    135560fe1d7e   16 seconds ago   489MB
hashimthepassionate/react-app   1         863701cc7202   31 minutes ago   489MB
muhammadhashim24/react-app      1         863701cc7202   31 minutes ago   489MB
ubuntu                          latest    35a88802559d   2 months ago     78.1MB
```

Here, you can see that the image has been tagged with **`react-app:2️⃣`** and is ready to be pushed to Docker Hub.

### 📤 Pushing the Updated Image to Docker Hub

Now, let's push the newly tagged image to Docker Hub:

```bash
docker push muhammadhashim24/react-app:2
```

**Output:**

```text
The push refers to repository [docker.io/muhammadhashim24/react-app]
a37a40102900: Pushed
a8b4ee0b8518: Layer already exists
cddaa97fa3de: Layer already exists
8d7831c063e6: Layer already exists
7bebba704faf: Layer already exists
e95e47def415: Layer already exists
982ab52e89c8: Layer already exists
f1c732704abe: Layer already exists
b2d5eeeaba3a: Layer already exists
2: digest: sha256:c3a4782200da4e93b268d3f05cac47018a526d611c74dbc390aa8b6b99e79c7f size: 2206
```

As you can see, Docker has successfully pushed the new image tagged as **`react-app:2️⃣`** to your Docker Hub repository. Notice that most layers are reused (`Layer already exists`), making the push process faster.

## 🏁 Summary

1. **Sign Up**: Create a free Docker Hub account.
2. **Create a Repository**: Set up a new repository on Docker Hub to store your images.
3. **Tag Your Images**: Properly tag your images before pushing them to Docker Hub.
4. **Log In**: Use `docker login` to authenticate your Docker CLI with Docker Hub.
5. **Push the Image**: Push your tagged image to Docker Hub using `docker push`.
6. **Make Changes and Rebuild**: If you update your project, rebuild the image, tag it with a new version, and push it again.
