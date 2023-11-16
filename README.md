# zk-login-prover

This repository is dedicated to the implementation of the zk-login proving service, and it heavily relies on the guidance provided in the [ZkLogin Docs](https://docs.sui.io/concepts/cryptography/zklogin#run-the-proving-service-in-your-backend). To ensure you are using the latest images or the most up-to-date devnet keys, please refer to the official documentation.

### Dependencies

Make sure you have the following dependencies installed on your system:

- [b2sum](https://command-not-found.com/b2sum) (Optional)
- [Git LFS](https://git-lfs.com/)
- [wget](https://command-not-found.com/wget)
- [zkeys](https://github.com/sui-foundation/zklogin-ceremony-contributions)
- [Docker & Docker Compose](https://docs.docker.com/compose/install/)

### Steps

1. **Install Dependencies:**
   Ensure that all the required dependencies are installed on your system. This includes `b2sum`, `Git LFS`, `wget`, `zkeys`, and Docker with Docker Compose.

2. **Pull Zkeys:**
   Use the following command to download the Zkeys to your working directory:

    ```console
    wget -O - https://raw.githubusercontent.com/sui-foundation/zklogin-ceremony-contributions/main/download-main-zkey.sh | bash
    ```

3. **Verify Zkey Hash:**
   Run `b2sum` inside the `zklogin-ceremony-contributions` directory to verify the integrity of the downloaded Zkey. The expected hash should be `060beb961802568ac9ac7f14de0fbcd55e373e8f5ec7cc32189e26fb65700aa4e36f5604f868022c765e634d14ea1cd58bd4d79cef8f3cf9693510696bcbcbce`.

    ```console
    b2sum zkLogin-main.zkey
    ```

4. **Start Docker Services:**
   Ensure the Docker daemon is running, and then initiate the Docker Compose setup:

    ```console
    docker-compose up
    ```

5. **Check Service Status:**
   Wait for approximately 1 minute until both services are up and running. Verify the installation's success by pinging the prover-fe:

    ```console
    curl http://localhost:8001/ping
    ```

   If the installation was successful, you should receive a response indicating the service is operational.

Feel free to refer to the [ZkLogin Docs](https://docs.sui.io/concepts/cryptography/zklogin#run-the-proving-service-in-your-backend) for any additional information or updates related to images and devnet keys.

### Explanation of the Configuration File

The provided configuration file is written in YAML and is designed for Docker Compose. It orchestrates the setup and deployment of the zk-login proving service. Let's break down the key components of the file:

#### Version:

```yaml
version: '3'
```

This specifies the version of the Docker Compose file format being used. In this case, it's version 3.

#### Services:

The `services` section defines the different components of the application:

##### Prover Service:

```yaml
  prover:
    container_name: prover
    image: mysten/zklogin:prover-a66971815c15ba10c699203c5e3826a18eabc4ee
    environment:
      - ZKEY=/app/binaries/zkLogin.zkey
      - WITNESS_BINARIES=/app/binaries
    ports:
      - '8000:8080'
    volumes:
      - ./zklogin-ceremony-contributions/zkLogin-main.zkey:/app/binaries/zkLogin.zkey
```

- `container_name`: Specifies the name of the container.
- `image`: Specifies the Docker image to be used for the prover service. In this case, it's pulling the image `mysten/zklogin:prover-a66971815c15ba10c699203c5e3826a18eabc4ee`.
- `environment`: Sets environment variables required by the prover service, such as the path to the Zkey and witness binaries.
- `ports`: Maps the host machine's port `8000` to the container's port `8080`.
- `volumes`: Mounts the Zkey file from the host machine into the container.

##### Prover Frontend Service:

```yaml
  prover-fe:
    image: mysten/zklogin:prover-fe-a66971815c15ba10c699203c5e3826a18eabc4ee
    environment:
      - PROVER_URI=http://prover:8080/input
      - NODE_ENV=production
      - DEBUG=zkLogin:info,jwks
    ports:
      - '8001:8080'
    depends_on:
      - prover
```

- `image`: Specifies the Docker image for the prover frontend service.
- `environment`: Sets environment variables for the prover frontend, such as the URI for the prover service, the node environment, and debug information.
- `ports`: Maps the host machine's port `8001` to the container's port `8080`.
- `depends_on`: Ensures that the prover frontend service starts only after the prover service is up and running.

This configuration file essentially defines how the prover and its frontend components should be configured, connected, and deployed using Docker Compose. It simplifies the deployment process and ensures that the necessary dependencies and configurations are in place.

### Configuring Environment Variables with an env file:

To simplify the management of environment variables and avoid hardcoding them directly in the Docker Compose file, you can use an env file. Here's how you can modify the existing Docker Compose file to utilize an env file:

1. **Create an env file:**

   Create a file named `.env` in the same directory as your Docker Compose file. This file will store your environment variables.

   ```dotenv
   # .env file

   # Prover Service
   ZKEY=/app/binaries/zkLogin.zkey
   WITNESS_BINARIES=/app/binaries

   # Prover Frontend Service
   PROVER_URI=http://prover:8080/input
   NODE_ENV=production
   DEBUG=zkLogin:info,jwks
   ```

   Adjust the values according to your specific configuration.

2. **Modify Docker Compose file to use env file:**

   Update your Docker Compose file to reference the env file:

   ```yaml
   version: '3'

   services:
     prover:
       container_name: prover
       image: mysten/zklogin:prover-a66971815c15ba10c699203c5e3826a18eabc4ee
       env_file:
         - .env
       ports:
         - '8000:8080'
       volumes:
         - ./zklogin-ceremony-contributions/zkLogin-main.zkey:/app/binaries/zkLogin.zkey

     prover-fe:
       image: mysten/zklogin:prover-fe-a66971815c15ba10c699203c5e3826a18eabc4ee
       env_file:
         - .env
       ports:
         - '8001:8080'
       depends_on:
         - prover
   ```

   The `env_file` key is added under each service, specifying the path to the env file.

3. **Run Docker Compose with env file:**

   Start your services using Docker Compose with the `-f` option to specify the Docker Compose file:

   ```bash
   docker-compose -f your-docker-compose-file.yml up
   ```

   Docker Compose will now read the environment variables from the specified env file, making it easier to manage and update configuration settings.

   Certainly! Deploying a Docker Compose setup on Digital Ocean involves a few key steps. Below is a guide on how you can do this:

### Deploying Docker Compose on Digital Ocean

1. **Create a Digital Ocean Account:**
   If you don't have a Digital Ocean account, sign up for one [here](https://www.digitalocean.com/).

2. **Create a Droplet:**
   - Log in to your Digital Ocean account.
   - Click on "Create" and select "Droplets."
   - Choose a distribution (e.g., Ubuntu), select an appropriate plan, and configure other settings as needed.
   - In the "Authentication" section, add your SSH key or use a password for authentication.
   - Click "Create Droplet."

3. **Access Your Droplet:**
   Use SSH to connect to your newly created Droplet. You can find the IP address of your Droplet on the Digital Ocean dashboard.

   ```bash
   ssh root@your_droplet_ip
   ```

4. **Install Docker and Docker Compose:**
   Inside your Droplet, install Docker and Docker Compose.

   ```bash
   # Update package lists
   sudo apt update

   # Install Docker
   sudo apt install docker.io

   # Install Docker Compose
   sudo apt install docker-compose
   ```

5. **Copy Docker Compose Files:**
   Copy your Docker Compose files and the `.env` file to your Droplet. You can use `scp` to securely copy files from your local machine to the Droplet.

   ```bash
   scp -r /path/to/your/docker/project root@your_droplet_ip:/path/on/droplet
   ```

6. **Navigate to Project Directory:**
   SSH into your Droplet and navigate to the directory where your Docker Compose files are located.

   ```bash
   ssh root@your_droplet_ip
   cd /path/on/droplet
   ```

7. **Start Docker Services:**
   Run Docker Compose to start your services.

   ```bash
   docker-compose up -d
   ```

   The `-d` flag runs the services in the background.

8. **Verify Deployment:**
   Check if your services are running on your Droplet.

   ```bash
   docker ps
   ```

   This command should list the containers that are currently running.

9. **Configure Digital Ocean Firewall:**
   In the Digital Ocean dashboard, navigate to the "Networking" section and configure the firewall to allow traffic on the necessary ports (e.g., 8000 and 8001).

10. **Access Services from Browser:**
    Open your web browser and navigate to `http://your_droplet_ip:8001/ping`. If everything is set up correctly, you should receive a response indicating that the service is operational.

### Notes:
- Ensure that your Digital Ocean Droplet has sufficient resources for your services.
- Make sure to secure your Droplet by following best practices for SSH and firewall settings.
- Regularly monitor your services and update the system to address any security vulnerabilities.

This guide should help you deploy your Docker Compose setup on Digital Ocean. Adjust the steps based on your specific project requirements and configurations.
