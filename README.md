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
