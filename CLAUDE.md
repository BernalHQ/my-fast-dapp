# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Hardhat v3 dapp project with TypeScript and viem toolbox for smart contract development.

## Tech Stack

- **Hardhat v3.1.4** - Smart contract development framework
- **Solidity 0.8.28** - Smart contract language
- **TypeScript** - Project language
- **Viem** - Ethereum interactions (via hardhat-toolbox-viem)
- **Node.js v22 LTS** - Required runtime (use `nvm use 22`)

## Commands

```bash
# Use correct Node version
nvm use 22

# Compile contracts
npx hardhat compile

# Run tests
npx hardhat test

# Start local node
npx hardhat node

# Deploy (example)
npx hardhat ignition deploy ./ignition/modules/<module>.ts
```

## Project Structure

```
├── contracts/          # Solidity smart contracts
├── scripts/            # Deployment and utility scripts
├── test/               # Contract tests
├── ignition/modules/   # Hardhat Ignition deployment modules
├── docs/               # Documentation
├── hardhat.config.ts   # Hardhat configuration
├── tsconfig.json       # TypeScript configuration
└── package.json        # Dependencies and scripts
```

## Development Notes

- Project uses ESM modules (`"type": "module"` in package.json)
- Hardhat toolbox uses viem instead of ethers.js
- Environment variables go in `.env` file (gitignored)

## Code Style

- Use TypeScript for all scripts and tests
- Follow Solidity style guide for smart contracts
- Use descriptive variable and function names
