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
