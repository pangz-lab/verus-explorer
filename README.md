# verus-explorer
- Setup and run Verus Coin / PbaaS coin web explorer locally.
- This enables running an web explorer quickly using the newly developed API and updated UI(based from insight-ui) built using docker for a seemless, scalable deployment.

## Prerequisite:
1. Verus Node 
2. [Docker](https://docs.docker.com/engine/install/) - `v27.0.3+`


## Setup
1. Update the chain configurations; `VRSC.conf` or `<pbaas_id>.conf`
 - stop node
 - add the explorer related configs
```bash
# insight explorer
addressindex=1
txindex=1
timestampindex=1
idindex=1
insightexplorer=1
spentindex=1
```
 - enable the ZMQ server configs
```bash
#ZMQ server
zmqpubrawblock=tcp://127.0.0.1:99000
zmqpubrawtx=tcp://127.0.0.1:99000
zmqpubhashtx=tcp://127.0.0.1:99000
zmqpubhashblock=tcp://127.0.0.1:99000
```
- restart node

2. Update each `.env.*` files based on local `VRSC.conf` or `<pbaas_id>.conf` configurations.(see the following)

### RPC Server
*Dotenv file*: `.env.rpc_server`
| Evironment Variable | Config Key  | Example     |
|---------------------|-------------|-------------|
|`RPC_NODE_URL`       | `<rpchost>:<rpcport>"` | `RPC_NODE_URL="localhost:27486"` |
|`RPC_NODE_USER`      | `<rpcuser>`            | `RPC_NODE_USER="user1679505996"` |
|`RPC_NODE_PASSWORD`  | `<rpcpassword>`        | `RPC_NODE_PASSWORD="password"`   |

### Explorer API
*Dotenv file*: `.env.api`
- Values of `zmqpubrawblock`, `zmqpubrawtx`, `zmqpubhashtx`, `zmqpubhashblock` are expected to be the same.
- Get the `host` and the `port` from the configuration. Example is `tcp://127.0.0.1:99000`

| Evironment Variable   | Data        | Example     |
|-----------------------|-------------|-------------|
|`NODE_ZMQ_ADDRESS`     | `"127.0.0.1"` | `NODE_ZMQ_ADDRESS="127.0.0.1"` |
|`NODE_ZMQ_PORT`        | `"99000"`     | `NODE_ZMQ_PORT="99000"`        |

### Explorer UI
*Dotenv file*: `.env.ui`
- Get your local IP address.
```bash
hostname -I
```
- Replace `localhost` value found in keys `ENV_API_SERVER` and `ENV_WS_SERVER`

## Run
- Make sure your local node is `100%` synced, otherwise you'll encounter an issue.
- Run `docker compose` to deploy
```bash
sudo docker compose up -d
```
- If everything goes well, you can now access your explorer locally.
    - `VRSC`  ⇨ <a href='http://localhost:2221'>http://localhost:2221</a>
    - `vARRR` ⇨ <a href='http://localhost:3331'>http://localhost:3331</a>
- Sometimes, initial load takes some seconds to fully show up.

## Issues/Error
- It's possible that you might have a conflicting port or keys.
- Update the values accordingly.

| `.env.rpc_server`             | `.env.api`                                           | `.env.ui`   |
|-------------------------------|------------------------------------------------------|-------------|
|`SERVER_ADDRESS`.`SERVER_PORT` | `EXT_API_HOST=http://<SERVER_ADDRESS>.<SERVER_PORT>` | `-`         |
|`-`                            | `LOCAL_SERVER_PORT`                                  | `ENV_API_SERVER=http://192.168.2.105:<LOCAL_SERVER_PORT>`         |
|`-`                            | `LOCAL_SERVER_PORT`                                  | `ENV_WS_SERVER=ws://192.168.2.105:<LOCAL_SERVER_PORT>/verus/wss ` |
|`-`                            | `LOCAL_SERVER_API_KEY`                               | `ENV_API_TOKEN`                                                   |

# References

## [![Docker](https://skillicons.dev/icons?i=github)](https://github.com/pangz-lab/) Github 
1. **verus-explorer-ui** - https://github.com/pangz-lab/verus-explorer-ui
2. **verus-explorer-api** - https://github.com/pangz-lab/verus-explorer-api
3. **verus-rpc-server** - https://github.com/pangz-lab/rust_verusd_rpc_server

## [![Docker](https://skillicons.dev/icons?i=docker)](https://docs.docker.com/get-docker/) Docker Hub
1. **verus-explorer-ui** - https://hub.docker.com/repository/docker/pangzlab/verus-explorer-ui/general
2. **verus-explorer-api** - https://hub.docker.com/repository/docker/pangzlab/verus-explorer-api/general
3. **verus-rpc-server** - https://hub.docker.com/repository/docker/pangzlab/verus-rpc-server/general
