# Compose VFlow simplified

This repository contains all the necessary resources for deploying VFlow nodes, including RPC, collator and boot nodes, on both the **testnet** and **mainnet**.

## Project overview

There are three types of nodes that can be deployed:

1. rpc
2. collator
3. boot

All scripts in this repository prompt for selection of the **node type** and the **network** to deploy.

---

## Requirements

* docker
* docker compose
* jq
* gnu-sed for Darwin distribution

---

## Instructions

⚠️ **Please review the `OPTIONAL` steps before manually starting the project after running the `./scripts/init.sh` script.**

Run the [init.sh](./scripts/init.sh) script and follow the instructions to prepare the deployment for the first time.

This script will generate all necessary deployment files under the [deployments](deployments) directory and provide the command to start the project. **However, it will not start the project automatically.**

```shell
./scripts/init.sh
```

### Optional: VFlow Node Data Snapshots

To reduce the time required for a node's startup, **daily snapshots of chain data** are available for:
- Mainnet: https://bootstraps.zkverify.io/
- Testnet: https://bootstraps.zkverify.io/volta

Snapshots are available in two forms:

- **Node snapshot**
- **Archive node snapshot**

Each snapshot is a **.tar.gz** archive containing the **db** directory, intended to replace the **db** directory generated during the initial node run.

You will need to download both the ZKVerify and the VFlow snapshots.

To use a snapshot:

- Stop the running node:
   ```shell
   ./scripts/stop.sh
   ```
- Navigate to the VFlow node's data directory. This may require `sudo` permissions. For an RPC node, the path is:
   ```
   cd /var/lib/docker/volumes/zkverify-rpc_node-data/_data/node/chains/<zkv_mainnet or zkv_testnet>
   ```
- Note the owner and permissions of the existing `db` directory, then delete it.
- Extract the downloaded VFlow snapshot and move its `db` directory into the current directory.
- Ensure the new `db` directory has the same permissions as the original db directory.
- Navigate to the ZKVerify node's data directory. This may require `sudo` permissions. For an RPC node, the path is:
   ```
   cd /var/lib/docker/volumes/zkverify-para-evm_node-data/_data/node/zkv_relay/chains/<zkv_mainnet or zkv_testnet>
   ```
- Note the owner and permissions of the existing `db` directory, then delete it.
- Extract the downloaded ZKVerify snapshot and move its `db` directory into the current directory.
- Ensure the new `db` directory has the same permissions as the original db directory.
- Return to the project directory and start the node:
   ```shell
   ./scripts/start.sh
   ```
- Verify the snapshot is working by checking the logs for `Highest known block at`, which should be close to the current chain height.
- Watch the logs until you can see the block height increasing.

### Optional: VFlow Node Secrets Injection

During the initial deployment **depending on the node type**, if prompted, the script will generate and store **PARA_NODE_KEY** and **PARA_SECRET_PHRASE** values in the `.env` file.

Alternatively, these secrets can be injected at runtime using a custom container entrypoint script to avoid keeping them in plaintext on disk.

Use the following steps to implement this approach:

1. Delete values of **PARA_NODE_KEY** and **PARA_SECRET_PHRASE** under the `deployments/${NODE_TYPE}/${NETWORK}/.env`
    ```bazaar
    PARA_NODE_KEY=""
    PARA_SECRET_PHRASE=""
    ```
2. Create **entrypoint_secrets.sh** file under `deployments/${NODE_TYPE}/${NETWORK}/` directory. For example:
    ```
    #!/usr/bin/env sh
    set -eu
    
    # TODO: Implement logic to inject secrets into the environment
   
    # Run the application entrypoint
    echo "=== 🚀 Starting the application entrypoint now..."
    exec /app/entrypoint.sh "$@"
    ```
3. Modify `deployments/${NODE_TYPE}/${NETWORK}/docker-compose.yml` file to mount and execute **custom entrypoint** script
    ```
    volumes:
      - "node-data:/data:rw"
      - "./entrypoint_secrets.sh:/app/entrypoint_secrets.sh:rw"
    entrypoint: ["/app/entrypoint_secrets.sh"]
    ```
4. Start compose project using the command provided in the end of [init.sh](./scripts/init.sh) script execution.

### Update

To update the project to a new version (e.g., when a new release is available):

1. Pull the latest changes from the repository.
2. Run the [update.sh](./scripts/update.sh) script.

⚠️ If the script prompts to update values in the `.env` file, it is **recommended** to accept all changes, unless there is a specific reason not to.

```shell
./scripts/update.sh
```

### Destroy

Run the [destroy.sh](./scripts/destroy.sh) script to destroy the node stack and all the associated resources. The script will prompt for confirmation before removing any resources.

```shell
./scripts/destroy.sh
```

## Usage Guide

### Start

Run the [start.sh](./scripts/start.sh) script to start the node stack.

```shell
./scripts/start.sh
```

### Stop

Run the [stop.sh](./scripts/stop.sh) script to just stop the node stack.

```shell
./scripts/stop.sh
```

---

## Contributing Guidelines

Please refer to the [CONTRIBUTING.md](CONTRIBUTING.md) file for information on how to contribute to this project.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
