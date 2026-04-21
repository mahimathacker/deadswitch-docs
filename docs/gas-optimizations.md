# Gas Optimizations

DeadSwitch is optimized to minimize gas costs for users. Here is every optimization applied and why.

## Optimizations

| Optimization | Technique | Savings |
|-------------|-----------|---------|
| Storage packing | 31 bytes packed into single Slot 0 | 1 SLOAD instead of 6 |
| Immutables | Contract references stored in bytecode | 0 gas vs 2,100 gas per read |
| Transient reentrancy | ReentrancyGuardTransient (EIP-1153) | 200 gas vs 7,100 gas |
| Custom errors | if/revert pattern | Cheaper than require strings |
| Unchecked increments | unchecked { ++i; } in loops | Saves overflow check gas |
| SafeERC20 | Handles non-standard ERC-20 tokens | Prevents silent failures |

## Storage Packing Explained

Ethereum charges 20,000 gas to write to a new storage slot and 2,100 gas to read from one. Packing multiple values into a single slot saves significant gas.

DeadSwitch packs 31 bytes of state variables into a single Slot 0:


s_state          1 byte   — current vault state (ACTIVE/WARNING/GRACE/DISTRIBUTING/COMPLETED)
s_lastCheckIn    6 bytes  — timestamp of last check-in
s_stateChangedAt 6 bytes  — timestamp of last state change
s_checkInInterval 6 bytes — how often owner must check in
s_warningPeriod  6 bytes  — duration of warning period
s_gracePeriod    6 bytes  — duration of grace period

Total: 31 bytes. Fits in one 32-byte storage slot. One SLOAD reads all of them.

## Immutables Explained

Contract references like i_yieldAdapter, i_willRegistry, and i_streamEngine are declared as immutable. They are stored in the contract bytecode rather than storage. Reading them costs 0 gas instead of 2,100 gas per read.

## Transient Storage Explained

EIP-1153 introduced transient storage — storage that only lasts for one transaction and is automatically cleared. OpenZeppelin's ReentrancyGuardTransient uses this for the reentrancy lock. It costs 200 gas instead of the traditional 7,100 gas approach.