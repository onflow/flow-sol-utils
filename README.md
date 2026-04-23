# flow-sol-utils — Solidity Utility Contracts for Flow EVM

[![License](https://img.shields.io/badge/license-Unlicense-blue.svg)](./LICENSE)
[![Release](https://img.shields.io/github/v/release/onflow/flow-sol-utils?include_prereleases&sort=semver)](https://github.com/onflow/flow-sol-utils/releases)
[![Discord](https://img.shields.io/badge/discord-flow-5865F2?logo=discord&logoColor=white)](https://discord.gg/flow)
[![Built on Flow](https://img.shields.io/badge/built%20on-Flow-00EF8B)](https://flow.com)
[![Foundry](https://img.shields.io/badge/built%20with-Foundry-black)](https://book.getfoundry.sh/)
## Overview

This repository contains a collection of Solidity contracts uniquely useful to building on EVM on Flow. You'll find libraries, interface and abstract contracts, as well as contract implementations you may find useful in your own projects.

## Contracts

* **WFLOW** — the equivalent of WETH on Ethereum, a wrapped version of the FLOW token.
  * Deployed to `0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e` on Flow [Testnet](https://evm-testnet.flowscan.io/token/0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e?tab=contract) and [Mainnet](https://evm.flowscan.io/token/0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e?tab=contract).
* **CadenceArchUtils** — a library of utility functions for working with Cadence Arch precompiles.
* **CadenceRandomConsumer** — a contract that assists in implementing secure onchain randomness, intended to make commit-reveal schemes easier to incorporate into your own projects.
* **Xorshift128plus** — an example pseudo-random number generator contract, helping you generate unique random numbers from onchain random sources.

## Installation

### Foundry

To install these contracts using Foundry:

```sh
forge install onflow/flow-sol-utils
```

### Hardhat

To install these contracts using Hardhat:

```sh
npm install @onflow/flow-sol-utils
```
## About Flow

This repo is part of the [Flow network](https://flow.com), a Layer 1 blockchain built for consumer applications, AI Agents, and DeFi at scale.

- Developer docs: https://developers.flow.com
- Cadence language: https://cadence-lang.org
- Community: [Flow Discord](https://discord.gg/flow) · [Flow Forum](https://forum.flow.com)
- Governance: [Flow Improvement Proposals](https://github.com/onflow/flips)
