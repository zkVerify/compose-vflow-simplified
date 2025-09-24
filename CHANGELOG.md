## 1.0.1
node: added **PARA_CONF_NO_PRIVATE_IP** and **PARA_CONF_NO_MDNS** as failsafe mechanism to prevent network abuse from the node

## 1.0.0
* node: VFlow version set to `latest`
* general: support for mainnet added
* automation: added support for setting optional --pool-limit and --pool-kbytes parameters for RPC node
* automation: added functionality to preserve optional variables during upgrade process
* compose: added **RUST_LOG** and **PARA_CONF_LOG** to env files

## 0.2.2
* node: VFlow version set to `0.2.2-1.0.0`

ENVIRONMENT VARIABLE CHANGES:
* `EVM_*` → replaced by `PARA_*`
* new mandatory variable `ZKV_CONF_CHAIN`


## 0.2.1
* node: VFlow version set to `0.2.1-0.2.0`
