# zk-login-prover

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

3.  Run b2sum to make sure you get the following hash `060beb961802568ac9ac7f14de0fbcd55e373e8f5ec7cc32189e26fb65700aa4e36f5604f868022c765e634d14ea1cd58bd4d79cef8f3cf9693510696bcbcbce`

    ```console
      b2sum zkLogin-main.zkey
    ```

4.  Run docker compose
    ```console
    docker-compose up
    ```
5.  Ping the server to make sure the installation was successful
    ```console
    curl http://localhost:8080/ping
    ```
