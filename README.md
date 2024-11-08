## Flow Solidity Utilities

This repository contains a collection of Solidity contracts uniquely useful to building on EVM on Flow. You'll find
libraries, interface and abstract contracts, as well as contract implementations you may find useful in your own
projects.

### Contracts

* WFLOW - the equivalent of WETH on Ethereum, a wrapped version of FLOW token.
  * Deployed to `0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e` on Flow [Testnet](https://evm-testnet.flowscan.io/token/0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e?tab=contract) & [Mainnet](https://evm.flowscan.io/token/0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e?tab=contract)
* CadenceArchUtils - a library of utility functions for working with Cadence Arch precompiles.
* CadenceRandomConsumer - a contract that assists in the implementation of secure onchain randomness, intended to make
  the implementation of commit-reveal schemes easier to incorporate into your own projects.
* Xorshift128plus - an example pseudo-random number generator contract, helping you generate unique random numbers from
  onchain random sources.

### Installation

#### Foundry

To install these contracts using Foundry, you can use the following command:

```sh
foundry install onflow/flow-sol-utils
```

#### Hardhat

To install these contracts using Hardhat, you can use the following command:

```sh
npm install @onflow/flow-sol-utils
```