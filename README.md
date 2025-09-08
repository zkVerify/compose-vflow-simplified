# Compose zkverify evm parachain simplified

This repository contains all the resources for deploying a zkverify evm PARACHAIN rpc, collator, or boot node.


## Project overview

There are three types of nodes that can be deployed:

1. rpc
2. collator
3. boot

When using any of the scripts provided in this repository, it will be requested to select **node type** and the **network** to run on.

---

## Requirements

* docker
* docker compose
* jq
* gnu-sed for Darwin distribution

---

## Installation instructions

Run the [init.sh](./scripts/init.sh) script and follow the instructions in order to prepare the deployment for the first time.

```shell
./scripts/init.sh
```

The script will generate the required deployment files under the [deployments](deployments) directory.

## Optional: ZKV Node Data Snapshots _(Work in Progress)_

To reduce the time required for a node's startup, **daily snapshots of chain data** are available here (PLACEHOLDER).

Snapshots are available in two forms:

- **Node snapshot**
- **Archive node snapshot**

Each snapshot is a **.tar.gz** archive containing the **db** directory, intended to replace the **db** directory generated during the initial node run.

### Optional: ZKV Node Secrets Injection

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

When a new version of the node is released this project will be updated with the new version modified in the `.env.*.template` files.

There may also be other changes to environment variables or configuration files that may need to be updated.

In order to update the project to the new version:

1. Pull the latest changes from the repository.
2. Run the [update.sh](./scripts/update.sh) script.

```shell
./scripts/update.sh
```

Should the script prompt you to update some of the values in .env file, it is recommended to accept all the changes
unless you know what you are doing.

### Destroy

Run the [destroy.sh](./scripts/destroy.sh) script to destroy the node stack and all the associated resources.

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
