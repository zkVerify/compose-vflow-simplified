## 1.0.4
* node: VFlow version for testnet pinned to `2.0.0-rc2`
* node: added optional **PARA_CONF_PUBLIC_ADDR** variable
* automation: `init.sh` and `update.sh` scripts prompt to set **PARA_CONF_PUBLIC_ADDR**
* automation: `docker compose` version check accepts v2 and newer
* automation: interactive menus list one option per line

## 1.0.3
* boot-node: pin the relay p2p port (**ZKV_CONF_PORT=30335**) so the parachain reclaims `/tcp/30334/ws`; the relay was defaulting to parachain port + 1 (30334) and stealing the parachain's ws listener, breaking the ws/wss bootnode endpoints

## 1.0.2
* collator-node: add **PARA_CONF_BLOCKS_PRUNING=1000** and **ZKV_CONF_BLOCKS_PRUNING=14400**

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
