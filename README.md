## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

-   **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
-   **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
-   **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
-   **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```
## Importtant Info
vm.startBroadcast() is a cheatcode that should only be called from inside run() or other explicitly controlled parts of the script.

When it's triggered during script construction, Foundry isn’t fully ready to manage transactions yet.

It tries to send the broadcast before Anvil is fully up and running or before gas estimation is complete.

This leads to it getting stuck waiting for receipts, as you saw in the logs.

HelperConfig config = new HelperConfig(); // outside run()
address priceFeed = config.activeNetworkConfig();
This line runs at contract construction time, before run() is even called.

Inside the HelperConfig constructor, you're doing:

if (block.chainid == LOCAL_CHAIN_ID) {
    activeNetworkConfig = getOrCreateLocalNetworkConfig();
}
And that function calls vm.startBroadcast().