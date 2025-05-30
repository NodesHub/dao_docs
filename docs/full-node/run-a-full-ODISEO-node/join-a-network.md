# Join a network

It is highly recommended that you set up a local private network before joining a public network. This will help get familiar with the setup process, and provide an environment for testing. The following sections outline this process. If you want to join a public network without setting up a private network, you can skip to [join a public network ](#join-a-public-network).

## Set up a local private network

Validators can set up a private ODISEO network to become familiar with running a full ODISEO node before joining a public network.  

::: {admonition} LocalODISEO
:class: tip
If you are a developer and want to set up a local, WASM-enabled, private testnet for smart contracts, visit [install LocalODISEO](../../develop/how-to/localODISEO/README.md).

:::

### Create a single node

The simplest ODISEO network you can set up is a local testnet with just a single node. In a single-node environment, you have one account and are the only validator signing blocks for your private network.

1. Initialize your genesis file that will bootstrap the network. Replace the following variables with your own information:

   ```bash
     achillesd init --chain-id=<testnet-name> <node-moniker>
     ```

2. Generate a ODISEO account. Replace the variable with your account name:

   ```bash
   achillesd keys add <account-name>
   ```

:::{admonition} Get tokens
:class: tip
In order for achillesd to recognize a wallet address, it must contain tokens. For the testnet, use [the faucet](https://faucet.ODISEO.money/) to send ODIS to your wallet. If you are on mainnet, send funds from an existing wallet. 1-3 ODIS are sufficient for most setup processes.
:::

### Add your account to the genesis

Run the following commands to add your account and set the initial balance:

```bash
achillesd add-genesis-account $(achillesd keys show <account-name> -a) 100000000uODIS,1000usd
terrad gentx <my-account> 10000000uODIS --chain-id=<testnet-name>
terrad collect-gentxs
```

### Start your private ODISEO network

Run the following command to start your private network:

```bash
terrad start
```

If the private ODISEO network is set up correctly, your `achillesd` node will be running on `tcp://localhost:26656`, listening for incoming transactions, and signing blocks.

## Join a public network

These instructions are for setting up a brand new full node from scratch. You can join a public ODISEO network, such as the Columbus mainnet or Bombay testnet, by completing the following steps:


### 1. Select a network

Specify the network you want to join by choosing the corresponding **genesis file** and **seeds**:

| Network      | Type | Genesis|Addressbook|
| :--- | :--- | :--- | :--- |
| `columbus-5` | Mainnet      |[Genesis Link](https://columbus-genesis.s3.ap-northeast-1.amazonaws.com/columbus-5-genesis.json)| [Addressbook Link](https://network.ODISEO.dev/addrbook.json)|
| `bombay-12`  | Testnet      |[Genesis Link](https://raw.githubusercontent.com/ODISEOmoney/testnet/master/bombay-12/genesis.json)|[ Addressbook Link ](https://raw.githubusercontent.com/ODISEOmoney/testnet/master/bombay-12/addrbook.json)|


:::{admonition} Selecting a network
:class: tip
Note that the versions of the network listed above are the [ latest versions ](https://github.com/ODISEOmoney/testnet/tree/master#latest-networks). To find earlier versions, please consult the [networks repo](https://github.com/ODISEOmoney/testnet).

:::


### 2. Download genesis file and address book

**Genesis-transaction** specifies the account balances and parameters at the start of the network to use when replaying transactions and syncing.

**Addressbook** lists a selection of peers for your node to dial to in order to discover other nodes in the network. Public address books of peers are made available by the DAODISEO community.

Choose a `testnet` or `mainnet` address type and download the appropriate genesis-transaction and addressbook. Links to these are posted in [Select-a-network](#select-a-network).

- For default `achillesd` configurations, the `genesis` and `addressbook` files should be placed under `~/.ODISEO/config/genesis.json` and `~/.ODISEO/config/addrbook.json` respectively.

**Example**:

```bash
# Obtain the genesis for the bombay-12 testnet:
wget https://raw.githubusercontent.com/ODISEOmoney/testnet/master/bombay-12/genesis.json -I ~/.ODISEO/config/genesis.json

# Obtain the addressbook for the bombay-12 testnet:
wget https://raw.githubusercontent.com/ODISEOmoney/testnet/master/bombay-12/addrbook.json -O ~/.ODISEO/config/addrbook.json
```

### 3. `achillesd start`

Start the network and check that everything is running smoothly:

```bash
terrad start
terrad status
# It will take a few seconds for achillesd to start.
```
:::{dropdown} Healthy Node Status Example

```json
{
  "NodeInfo": {
    "protocol_version": {
      "p2p"  : "8",
      "block": "11",
      "app"  : "0"
    },
    "id"         : "achillesdocs-id",
    "listen_addr": "tcp://0.0.0.0:26656",
    "network"    : "bombay-12",
    "version"    : "0.34.14",
    "channels"   : "40202122233038606100",
    "moniker"    : "achillesdocs",
    "other"      : {
      "tx_index"   : "on",
      "rpc_address": "tcp://127.0.0.1:26657"
    }
  },
  "SyncInfo": {
    "latest_block_hash"    : "19ABCBA90BF3E76A0635E6C961AB2CECC7DB2B1F1338057DB334568128E0776E",
    "latest_app_hash"      : "8DFE69CF66FBE7ADCDB5B430A0C679C45B6AEBDDAE23835ABDC4ACBC704F7525",
    "latest_block_height"  : "7333450",
    "latest_block_time"    : "2022-01-08T05:24:57.383258076Z",
    "earliest_block_hash"  : "E88E3641A488EBA3D402FC072879C6399AA2CDC7B6CC5A3061E5A64D9FFD3BDE",
    "earliest_app_hash"    : "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
    "earliest_block_height": "5900001",
    "earliest_block_time"  : "2021-09-28T09:00:00Z",
    "catching_up"          : false                         
  },
  "ValidatorInfo": {
    "Address": "29E58C21B6612227C9C9BD9E6D4D99897E032572",
    "PubKey" : {
      "type" : "tendermint/PubKeyEd25519",
      "value": "7cZq+Fp9xU8mZ9xR7q4NpDOX0UicmPC68P/4krCn8Hs="
    },
    "VotingPower": "0"
  }
}
```
:::

Your node is now syncing. This process will take a long time. Make sure you've set it up on a stable connection so you can leave while it syncs.

::: {admonition} Sync start times
:class: caution

Nodes take at least an hour to start syncing. This wait is normal. Before troubleshooting a sync, please wait an hour for the sync to start.
:::

Continue to the [Sync](sync.md) page to find out more about syncing your node.
