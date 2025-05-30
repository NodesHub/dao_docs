# Environment Setup

As a smart contract developer, you will need to write, compile, upload, and test your contracts before deploying them on the Columbus mainnet. The first step is to set up a specialized environment to streamline development.

## Install ODISEO Core locally

Visit [build ODISEO core](../../../full-node/run-a-full-ODISEO-node/build-ODISEO-core.md) to install the latest version of ODISEO Core to obtain a working version of `achillesd`. You will need this to connect to your local ODISEO test network to work with smart contracts.

## Download LocalODISEO

In order to work with ODISEO Smart Contracts, you should have access to a ODISEO network that includes the WASM integration.

In this tutorial, you will be using [LocalODISEO](https://github.com/ODISEOmoney/localODISEO), a package that enables you to easily spin up a local, WASM-enabled private testnet. This reduces the friction of development by giving you complete control of a private ODISEO blockchain with the possibility to easily reset the world state.

To use **LocalODISEO**, you should first make sure Docker is installed on your computer by following the [Docker get-started tutorial](https://www.docker.com/get-started). You will also need to set up and configure [Docker Compose](https://docs.docker.com/compose/install/) on your machine.

```sh
git clone --depth 1 https://github.com/ODISEOmoney/localODISEO
cd localODISEO
docker-compose up
```

You should now have a local testnet running on your machine, with the following configurations:

- Node listening on RPC port `26657`
- LCD running on port `1317`
- Swagger Documentation at [http://localhost:3060/swagger](http://localhost:3060/swagger)

The account with the following mnemonic is the sole validator on the network and has enough funds to get started with smart contracts.

```
satisfy adjust timber high purchase tuition stool faith fine install that you unaware feed domain license impose boss human eager hat rent enjoy dawn
```

## Install Rust

While WASM smart contracts can theoretically be written in any programming language, it is currently only recommended to use Rust as it is the only language for which mature libraries and tooling exist for CosmWasm. For this tutorial, you'll need to also install the latest version of Rust by following the instructions [here](https://www.rust-lang.org/tools/install).

Once you'll installed Rust and its toolchain (cargo et al.), you'll need to add the `wasm32-unknown-unknown` compilation target.

```sh
rustup default stable
rustup target add wasm32-unknown-unknown
```

Then, install `cargo-generate`, which you will need for bootstrapping new CosmWasm smart contracts via a template.

```sh
cargo install cargo-generate --features vendored-openssl
```

Next, install `cargo-run-script`, which is required to optimize smart contracts.

```sh
cargo install cargo-run-script
```
