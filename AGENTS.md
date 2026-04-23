# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Codex, Cursor, Copilot, and others) working in this repository. It is loaded automatically on attach — keep it concise.

## Overview

`flow-sol-utils` is a Solidity utility library for Flow EVM, distributed as both a Foundry package (`forge install onflow/flow-sol-utils`) and an npm package (`@onflow/flow-sol-utils`). It provides a wrapped-FLOW token, helpers for the Cadence Arch pre-compile, and a commit-reveal randomness consumer backed by Flow's protocol-native randomness. The repository itself is a Foundry project — there is no in-repo Hardhat configuration; npm distribution only ships `src/**/*.sol` (see `package.json` `files`) for downstream Hardhat/other consumers.

## Build and Test Commands

Foundry is the only build system in this repo. Initialize the `forge-std` submodule on first checkout (`git submodule update --init --recursive`), then:

- `forge build` — compile contracts to `out/`.
- `forge build --sizes` — compile and print contract sizes (CI equivalent).
- `forge test -vvv` — run the Foundry test suite with verbose traces (CI equivalent).
- `forge fmt` — format Solidity sources; `forge fmt --check` is enforced in CI.
- `forge install <pkg>` — add a new library submodule under `lib/`.

CI (`.github/workflows/test.yml`) runs `forge fmt --check`, `forge build --sizes`, and `forge test -vvv` under `FOUNDRY_PROFILE=ci` on nightly Foundry.

## Architecture

Foundry layout, defined in `foundry.toml` (`src = "src"`, `out = "out"`, `libs = ["lib"]`):

- `src/wflow/WETH9.sol` — Wrapped FLOW (WFLOW) ERC-20. Contract is named `WETH9` (forked from Dapphub WETH9, GPL-3) with `name = "Wrapped Flow"` / `symbol = "WFLOW"`. Deployed at `0xd3bF53DAC106A0290B0483EcBC89d40FcC961f3e` on Flow EVM Mainnet and Testnet (per `README.md`). Pragma `>=0.4.22 <0.6` — do not import from newer-pragma sources; integrate via its deployed address.
- `src/cadence-arch/CadenceArchUtils.sol` — library wrapping `staticcall` to the Cadence Arch pre-compile at `0x0000000000000000000000010000000000000001` (`flowBlockHeight()`, `getRandomSource(uint64)`, `revertibleRandom()`).
- `src/random/CadenceRandomConsumer.sol` — abstract contract implementing a commit-reveal scheme over `CadenceArchUtils`; consumers inherit and call `_requestRandomness` / `_fulfillRandomRequest`.
- `src/random/Xorshift128plus.sol` — `Xorshift128plus` PRG library seeded from a source of randomness plus salt.
- `src/random/test/TestCadenceRandomConsumer.sol` — in-`src/` test harness that exposes `CadenceRandomConsumer`'s internal functions. It ships to npm consumers because `package.json` `files` is `src/**/*.sol`.
- `test/random/CadenceRandomConsumer.t.sol` — forge-std test using `vm.mockCall` to stub the three Cadence Arch entry points.
- `lib/forge-std` — git submodule (`foundry-rs/forge-std`, see `.gitmodules`); the only external dependency.
- `flow.json` — Flow CLI config with emulator/testnet/mainnet endpoints; `contracts` and `deployments` are empty (this repo ships Solidity, not Cadence).

## Conventions and Gotchas

- All contracts except `WETH9.sol` use `pragma solidity ^0.8.19` under SPDX `Unlicense`. `WETH9.sol` is GPL-3 with pragma `>=0.4.22 <0.6` — treat it as a vendored artifact.
- `forge fmt --check` is a CI gate; run `forge fmt` before committing Solidity changes.
- The Cadence Arch pre-compile address lives in `CadenceArchUtils.cadenceArch` — do not hardcode it elsewhere.
- `revertibleRandom()` is revertible by callers; use `CadenceRandomConsumer`'s commit-reveal flow when the caller isn't trusted (see NatSpec on `CadenceRandomConsumer` and `CadenceArchUtils._revertibleRandom`).
- Tests must mock all three Arch calls (`flowBlockHeight()`, `getRandomSource(uint64)`, `revertibleRandom()`) — see `setUp()` in `test/random/CadenceRandomConsumer.t.sol`.
- `src/random/test/TestCadenceRandomConsumer.sol` is published to npm by design; if you move it to `test/`, also update `package.json` `files`.
- Commit messages: imperative mood, subject ≤72 chars (`CONTRIBUTING.md`).

## Files Not to Modify

- `lib/forge-std/**` — git submodule; update via `forge update forge-std`.
- `src/wflow/WETH9.sol` — vendored Dapphub WETH9; the deployed address is canonical on Flow EVM.
- `out/`, `cache/` — Foundry build artifacts (gitignored).
