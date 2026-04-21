# Security

## Security Practices

**CEI Pattern**
Checks-Effects-Interactions on every external function. State is always updated before any external call.

**Transient Reentrancy Guard**
OpenZeppelin's ReentrancyGuardTransient. Uses EIP-1153 transient storage — 200 gas instead of 7,100 gas for the traditional approach.

**SafeERC20**
Handles fee-on-transfer tokens and non-standard return values. Prevents silent failures on token transfers.

**48-Hour Timelock**
Will changes require a 48-hour waiting period before they take effect. Protects against last-minute malicious changes.

**Ownable with Disabled Transfer**
transferOwnership and renounceOwnership are overridden to revert. The vault owner cannot be changed after deployment.

**Custom Errors**
Gas-efficient error handling with descriptive revert reasons. Cheaper than require strings.

**State Machine Enforcement**
Every function validates the current state before execution. Functions that should only run in ACTIVE state cannot run in DISTRIBUTING state.

## Testing

**Static Analysis**
- Slither — detects common vulnerabilities automatically
- Aderyn — Rust-based static analyzer for Solidity

**Fuzz Testing**
Foundry fuzz tests run thousands of random inputs against every function to find edge cases.

**Invariant Testing**
Foundry invariant tests verify that critical properties always hold regardless of what sequence of actions is taken. For example — total distributed amount never exceeds total deposited amount.

**Fork Testing**
Tests run against a fork of live Arbitrum mainnet to verify real Aave V3 integration works correctly.

## Reporting a Vulnerability

If you find a security issue please open a private GitHub issue or contact directly. Do not disclose publicly before a fix is deployed.