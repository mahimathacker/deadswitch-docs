# How It Works

DeadSwitch uses a simple five step process.

## Five Steps

**1. Create a Vault**
Deposit ETH or ERC-20 tokens into your personal DeadSwitch vault.

**2. Earn Yield**
Your assets are automatically deposited into Aave V3 and earn interest while you are alive.

**3. Set Your Will**
Choose who gets what — percentages, instant payout or streamed over time.

**4. Check In**
Prove you are alive periodically. You choose the interval — every 30, 60, or 90 days.

**5. If You Stop**
Warning period → Grace period → Automatic distribution to your beneficiaries.

---

## State Machine

Owner checks in every 30 days
|
v
+---------+
|  ACTIVE  | <-- checkIn() resets timer
+----+----+
|  Missed check-in
v
+---------+
| WARNING |  7 days for owner to respond
+----+----+
|  Still no response
v
+--------------+
| GRACE PERIOD |  72 hours, last chance
+------+-------+
|  No response
v
+--------------+
| DISTRIBUTING |  Pulls from Aave, sends to heirs
+------+-------+
|
v
+--------------+
|  COMPLETED   |  Vault empty, all funds distributed
+--------------+

---

## Check-in Intervals

You choose how often you need to check in:

- Every **30 days** — monthly confirmation
- Every **60 days** — every two months
- Every **90 days** — quarterly confirmation

Missing your check-in triggers the Warning state. You still have 7 days plus a 72 hour grace period to respond before distribution begins.

# Architecture

## Contract Overview

DeadSwitch.sol (Main Vault)
|
|-- YieldAdapter.sol ---- Aave V3 Pool (supply/withdraw)
|
|-- WillRegistry.sol ---- Beneficiary storage + 48hr timelock
|
|-- StreamEngine.sol ---- Time-released payments to heirs
|
+-- Chainlink Automation -- Monitors check-ins, triggers state changes
DeadSwitchFactory.sol ---- Deploys individual vaults per user


## Contracts

| Contract | Purpose |
|----------|---------|
| `DeadSwitch.sol` | Core vault — state machine, deposits, withdrawals, check-ins, distribution |
| `YieldAdapter.sol` | Wraps Aave V3 supply() and withdraw() — isolates yield logic |
| `WillRegistry.sol` | Stores beneficiaries with 48-hour timelock on changes |
| `StreamEngine.sol` | Linear payment streams for gradual inheritance distribution |
| `DeadSwitchFactory.sol` | Factory pattern — deploys vaults for users in one transaction |

## Tech Stack

| Layer | Technology |
|-------|------------|
| Smart Contracts | Solidity 0.8.28 |
| Framework | Foundry |
| Yield | Aave V3 on Arbitrum |
| Automation | Chainlink Automation |
| Chain | Arbitrum One |
| Testing | Foundry (unit, fuzz, invariant, fork) |
| Security | Slither, Aderyn |

## Storage Layout

IMMUTABLES (zero cost):  i_yieldAdapter, i_willRegistry, i_streamEngine
CONSTANTS (zero cost):   MAX_BENEFICIARIES, MIN/MAX_CHECKIN_INTERVAL, BASIS_POINTS
SLOT 0 (31 bytes):       s_state (1B) + s_lastCheckIn (6B) + s_stateChangedAt (6B)
+ s_checkInInterval (6B) + s_warningPeriod (6B) + s_gracePeriod (6B)
SLOT 1:                  s_supportedTokens[]
SLOT 2:                  s_tokenExists mapping
TRANSIENT:               Reentrancy lock (100 gas read + 100 gas write)