# BitStack Secure Rollup

**A secure and compliant Layer 2 Rollup protocol for the Stacks blockchain**, leveraging Bitcoin-level finality and robust fraud-proof mechanisms to ensure scalability without compromising on trustlessness.

## Overview

**BitStack Secure Rollup** is a smart contract written in [Clarity](https://docs.stacks.co/docs/write-smart-contracts/clarity-overview/) that implements an *optimistic rollup* model for the [Stacks blockchain](https://www.stacks.co/). It is designed to:

* **Batch transactions off-chain** to improve throughput and reduce costs.
* **Maintain integrity and security** via on-chain state commitments and challenge periods.
* **Allow trustless deposits and withdrawals** through Merkle proof validation.
* **Enable efficient internal transfers** within the rollup system.

All while maintaining **Bitcoin-level security guarantees** thanks to the underlying design of the Stacks chain.

## Features

* **Operator Management**

  * Only authorized operators can submit state commitments.
  * Registration is limited to the contract owner for security.

* **Optimistic Rollup Mechanics**

  * State commitments are submitted with metadata (transactions, value, Merkle root).
  * Fraud is mitigated through a public challenge process.

* **Secure Deposits & Withdrawals**

  * STX tokens can be deposited into the rollup contract.
  * Withdrawals require Merkle proof validation.

* **On-Chain Challenges**

  * Anyone can challenge a suspicious state commitment.
  * Challengers post a bond, which can be slashed if the challenge is invalid.

* **Internal Token Transfers**

  * Users can transfer balances within the rollup without on-chain settlement.

## Architecture

* **State Commitments**: Off-chain batches are committed on-chain with a Merkle root and metadata.
* **Challenge Period**: Allows users to dispute invalid batches before they become final.
* **User Balances**: Managed via maps, scoped by user and token type.
* **Security Bonds**: Used to incentivize honest behavior from operators and challengers.

## Contract Structure

### Maps

| Map                 | Purpose                                      |
| ------------------- | -------------------------------------------- |
| `operators`         | Tracks active rollup operators.              |
| `state-commitments` | Stores off-chain batch commitments.          |
| `user-balances`     | Tracks per-user balances by token.           |
| `challenges`        | Stores open challenges to state commitments. |

### Core Functions

#### Public

* `register-operator`: Only callable by the contract owner.
* `submit-state-commitment`: Submit a new batch with a bond.
* `challenge-commitment`: Submit fraud proof with bond.
* `resolve-challenge`: Validate or reject challenged commitment.
* `deposit`: Add funds to user rollup balance.
* `withdraw`: Withdraw funds using Merkle proof.
* `transfer-in-rollup`: Move balances within rollup.

#### Read-Only

* `get-user-balance`: Returns balance for a given user/token pair.

## Security Considerations

* **Challenge Mechanism**: Deterrent against fraudulent state submissions.
* **Merkle Verification**: Ensures correctness of withdrawals.
* **Stake Requirements**: Operators and challengers must lock value to participate.
* **Ownership Restriction**: Only contract owner can register operators, limiting attack surface.

## Installation & Deployment

1. **Clone this repository**:

   ```bash
   git clone https://github.com/your-org/bitstack.git
   cd bitstack
   ```

2. **Compile the contract**:

   ```bash
   clarinet check
   ```

3. **Deploy with Clarinet**:

   ```bash
   clarinet deploy
   ```

4. **Test locally**:

   ```bash
   clarinet test
   ```

## Example Usage

```clojure
;; Deposit 1000 STX to token ID 1
(deposit u1000 u1)

;; Transfer 250 STX within the rollup
(transfer-in-rollup tx-sender recipient u250 u1)

;; Withdraw 500 STX with a Merkle proof
(withdraw u500 u1 proof-buff)
```

## Contributing

We welcome contributions! Please open issues or pull requests if you'd like to improve the protocol or report bugs.
