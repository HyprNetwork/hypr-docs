---
description: >-
  The Anonymous Precompile Contract is an all-purpose tool that provides some
  functions to verify some on-chain transactions using zero-knowledge proof
  technology.
---

# The Anonymous Precompile Contract

### The problem we solved

Based on the Ethereum L2 requirements of  the security and correctness of transactions, we provide a more convenient transaction verification strategy to meet the growing number of transactions on Ethereum L2. Through the transaction validation scheme based on the zero-knowledge proof that we provide, the correctness and legitimacy of the transaction can be verified faster and more safely.

### The Solidity APIs for Anonymous Precompile Contract

We provide zero-knowledge proof verification functions for deposit, transfer and withdraw transaction types through precompiled smart contracts. These functions can be invoked via Solidity API.\
\
To view the details, please refer to the [Solidity Interface of Anonymous](https://github.com/HyprNetwork/hypr-precompile-interface/blob/main/contracts/interface/IAnonymous.sol) on Github.

| API name    | Description                                          |
| ----------- | ---------------------------------------------------- |
| deposit()   | Verifying a deposit type transaction through proof.  |
| transfer()  | Verifying a transfer type transaction through proof. |
| ownership() | Verifying a withdraw type transaction through proof. |



| Solidity Interface            | Contract Address                           |
| ----------------------------- | ------------------------------------------ |
| Anonymous Precompile Contract | 0x0000000000000000000000000000000000020001 |
