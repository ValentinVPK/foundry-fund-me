# Fund Me Smart Contract

This project is part of the Foundry Fundamentals course, where we learn about smart contract development using Foundry - a blazing fast, portable, and modular toolkit for Ethereum application development written in Rust.

## Project Overview

This is a decentralized crowdfunding platform where users can:

- Fund the contract with ETH
- Track funding amounts per address
- Withdraw funds (owner only)
- Get real-time ETH/USD price feed using Chainlink

The project demonstrates key concepts in smart contract development:

- Solidity fundamentals
- Foundry testing framework
- Gas optimization techniques
- Chainlink price feeds integration
- Contract deployment and verification
- Local development with Anvil

## Dependencies

This project uses several key dependencies:

- **chainlink-brownie-contracts**: Provides Chainlink price feed interfaces and contracts for real-time ETH/USD price data
- **foundry-devops**: Tools for deployment and contract verification
- **forge-std**: Standard library for Foundry testing and development

## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

- **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
- **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
- **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
- **Chisel**: Fast, utilitarian, and verbose solidity REPL.

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
