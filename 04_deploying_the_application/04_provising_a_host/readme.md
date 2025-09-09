# Provisioning a Host on DigitalOcean 🛠️

Provisioning a host on DigitalOcean allows you to deploy your Docker applications on a cloud server (Droplet) with ease. This guide will walk you through the steps to create a DigitalOcean Droplet using Docker Machine on a Windows system.

## Step 1: Generate a DigitalOcean API Key 🔑

To interact with DigitalOcean programmatically, you need an API key.

1. **Login to DigitalOcean**: Go to [DigitalOcean](https://www.digitalocean.com/) and log in to your account.
2. **Generate an API Key**:
   - Navigate to the "API" section from the DigitalOcean dashboard.
   - Click on "Generate New Token."
   - Give your token a name (e.g., `docker-machine`).
   - Set the permissions to "Read/Write."
   - Click "Generate Token."
   - **Copy the API key** to your clipboard or save it securely. You won’t be able to see it again.

## Step 2: Provision a Host Using Docker Machine 🚀

With your API key in hand, you can now create a new DigitalOcean Droplet using Docker Machine. Open your terminal (Command Prompt or PowerShell) and run the following command:

```bash
docker-machine create --driver digitalocean --digitalocean-access-token <API_KEY> --digitalocean-image "ubuntu-20-04-x64" --engine-install-url "https://releases.rancher.com/install-docker/19.03.9.sh" vidly2
```

### Explanation of the Command:

- **`docker-machine create`**: Creates a new Docker Machine instance.
- **`--driver digitalocean`**: Specifies that DigitalOcean will be the cloud provider.
- **`--digitalocean-access-token <API_KEY>`**: Uses the API key you generated to authenticate with DigitalOcean. Replace `<API_KEY>` with your actual API key.
- **`--digitalocean-image "ubuntu-20-04-x64"`**: Specifies the operating system image to use for the Droplet (Ubuntu 20.04 in this case).
- **`--engine-install-url "https://releases.rancher.com/install-docker/19.03.9.sh"`**: Specifies the URL for installing Docker on the Droplet.
- **`vidly2`**: The name of the Docker Machine instance you’re creating.

### Output of the Command:

Once you run the command, Docker Machine will start provisioning the host on DigitalOcean. You should see output similar to this:

```plaintext
Running pre-create checks...
Creating machine...
(vidly2) Creating SSH key...
(vidly2) Creating Digital Ocean droplet...
(vidly2) Waiting for IP address to be assigned to the Droplet...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
```

### Final Message:

At the end of the process, you’ll see the following message:

```plaintext
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: C:\Users\aaaa\bin\docker-machine.exe env vidly2
```

This confirms that Docker has been successfully installed on your new DigitalOcean Droplet, and the Droplet is up and running.

## Step 3: Connecting to Your Docker Machine Instance 🌐

To connect your local Docker client to the Docker Engine running on your newly created Droplet, you need to configure your environment:

1. **Run the following command in your terminal**:
   ```bash
   C:\Users\aaaa\bin\docker-machine.exe env vidly2
   ```
   This command will output the environment variables needed to connect to the Docker Engine on the Droplet.

2. **Configure your terminal to use these settings**:
   - Run the command provided in the output, usually something like:
     ```bash
     eval $(docker-machine env vidly2)
     ```
   - This configures your terminal to use the remote Docker Engine.

3. **Verify the connection**:
   - Run:
     ```bash
     docker ps
     ```
   - This should list the running containers on your DigitalOcean Droplet, confirming that you are connected to the remote Docker Engine.

## Conclusion 🎯

By following these steps, you have successfully provisioned a DigitalOcean Droplet using Docker Machine, installed Docker on it, and connected your local Docker client to the remote Docker Engine. This setup allows you to manage your Docker containers on DigitalOcean as if they were running locally, making it easy to deploy, monitor, and scale your applications in the cloud.
