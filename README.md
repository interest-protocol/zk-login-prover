# zk-login-prover

This repository is based on [ZkLogin Docs](https://docs.sui.io/concepts/cryptography/zklogin#run-the-proving-service-in-your-backend). Please refer to it to use the latest images or the devnet keys.

### Dependencies

- [b2sum](https://command-not-found.com/b2sum) (Optional)
- [Git LFS](https://git-lfs.com/)
- [wget](https://command-not-found.com/wget)
- [zkeys](https://github.com/sui-foundation/zklogin-ceremony-contributions)
- [Docker & Docker Compose](https://docs.docker.com/compose/install/)

### Steps

1.  Make sure you install all dependencies

2.  Pull the Zkeys to your directory with the command below

    ```console
    wget -O - https://raw.githubusercontent.com/sui-foundation/zklogin-ceremony-contributions/main/download-main-zkey.sh | bash
    ```

3.  Run b2sum inside the zklogin-ceremony-contributions directory to make sure you get the following hash `060beb961802568ac9ac7f14de0fbcd55e373e8f5ec7cc32189e26fb65700aa4e36f5604f868022c765e634d14ea1cd58bd4d79cef8f3cf9693510696bcbcbce`

    ```console
      b2sum zkLogin-main.zkey
    ```

4.  Start the Docker daemon and run docker compose
    ```console
    docker-compose up
    ```
5.  Wait a 1 minute until both services are up and ping the server to make sure the installation was successful
    ```console
    curl http://localhost:8001/ping
    ```
