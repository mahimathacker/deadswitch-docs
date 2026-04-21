# Getting Started

## Prerequisites

- [Foundry](https://book.getfoundry.sh/getting-started/installation)
- Git

## Installation

```bash
git clone https://github.com/YOUR_USERNAME/deadswitch.git
cd deadswitch
forge install
forge build
```

## Running Tests

```bash
# Unit tests
forge test

# Fork tests against live Arbitrum Aave
forge test --fork-url $ARBITRUM_RPC -vvv

# Fuzz tests
forge test --match-test testFuzz

# Coverage
forge coverage
```

## Deployment

```bash
# Arbitrum Sepolia (testnet)
forge script script/DeployDeadSwitch.s.sol \
  --rpc-url $ARBITRUM_SEPOLIA_RPC \
  --broadcast \
  --verify

# Arbitrum One (mainnet)
forge script script/DeployDeadSwitch.s.sol \
  --rpc-url $ARBITRUM_RPC \
  --broadcast \
  --verify
```

## Project Structure


deadswitch/
├── src/
│   ├── interfaces/
│   │   ├── IDeadSwitch.sol
│   │   ├── IYieldAdapter.sol
│   │   ├── IWillRegistry.sol
│   │   ├── IStreamEngine.sol
│   │   └── IDeadSwitchFactory.sol
│   ├── DeadSwitch.sol
│   ├── YieldAdapter.sol
│   ├── WillRegistry.sol
│   ├── StreamEngine.sol
│   └── DeadSwitchFactory.sol
├── test/
│   ├── unit/
│   ├── fuzz/
│   ├── invariant/
│   └── fork/
├── script/
│   └── DeployDeadSwitch.s.sol
└── foundry.toml

## Deployments

| Network | Contract | Address |
|---------|----------|---------|
| Arbitrum Sepolia | DeadSwitchFactory | TBD |
| Arbitrum Sepolia | DeadSwitch (impl) | TBD |
| Arbitrum One | DeadSwitchFactory | TBD |
| Arbitrum One | DeadSwitch (impl) | TBD |
