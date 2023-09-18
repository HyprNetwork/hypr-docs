---
description: >-
  The Anemoi Precompile Contract is an all-purpose tool that provides some
  functions to easily calculate elliptic curves on chains.
---

# The Anemoi Precompile Contract

### The problem we solved

Based on elliptic curve cryptography calculations are commonly used in blockchain and bn254 is one of the more well-known and commonly used elliptic curve cryptography algorithms. We provide the more easy bn254 calculation method on the chain, and it can avoid some security and compatibility problems of offline calculation to a certain extent.

### The Solidity APIs for Anemoi Precompile Contract

We provide these functions through precompiled smart contracts, containing bn254 elliptic curve calculation and bn254 salt obtained. These functions can be invoked via Solidity API.\
\
To view the details, please refer to the [Solidity Interface of Anemoi](https://github.com/HyprNetwork/hypr-precompile-interface/blob/main/contracts/interface/IAnemoi.sol) on Github.

| API name           | Description                                                              |
| ------------------ | ------------------------------------------------------------------------ |
| jive\_4()          | Calculating elliptic curve through two points using the BN254 algorithm. |
| jive\_254\_salts() | Obtaining the salt value for the BN254 elliptic curve algorithm.         |



| Solidity Interface         | Contract Address                           |
| -------------------------- | ------------------------------------------ |
| Anemoi Precompile Contract | 0x0000000000000000000000000000000000020002 |
