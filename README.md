# zk-login-prover

### Dependencies

- [b2sum](https://command-not-found.com/b2sum) (Optional)
- [wget](https://command-not-found.com/wget)
- zkeys
- [Docker & Docker Compose](https://docs.docker.com/compose/install/)

### Steps

1.  Make sure you install all dependencies

2.  Pull the Zkeys to your directory with the command below

    ```console
    wget -O - https://raw.githubusercontent.com/sui-foundation/zklogin-ceremony-contributions/main/download-main-zkey.sh | bash
    ```

3.  Run docker compose
    ```console
    docker-compose up
    ```
4.  Ping the server to make sure the installation was successful
    ```console
    curl http://localhost:8080/ping
    ```
