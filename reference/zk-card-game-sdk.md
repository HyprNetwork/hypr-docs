---
description: >-
  On-chain gaming solution with a zero-knowledge card protocol implemented
  through a precompiled smart contract
---

# ZK Card Game SDK

Introducing the Hypr card game SDK - a versatile toolkit empowering game developers to effortlessly create on-chain card games with zero-knowledge proof technology.

### Features

*   **Define custom cards.**

    Developers have the flexibility to define their desired number of cards, suites, and values.
*   **Hide card values.**

    The card values undergo encryption using the aggregate key, and players conceal these encrypted values from one another.
*   **Zero-knowledge card shuffling.**

    Off-chain, the shuffling process takes place, ensuring fair randomness, as each player utilizes zero-knowledge proofs.
*   **Zero-knowledge card revealling.**

    Card values remain concealed from any subset of players, unless unanimous agreement is reached among all players to reveal the cards.
* **Cheating prevention.**\
  Every confidential off-chain computation is verified through a zero-knowledge proof and validated by the on-chain smart contract.

### Use Cases

The protocol facilitates a wide range of on-chain card-like games, including but not limited to:

* Poker games
* Casino games
* Board games
* Turn-based games
* Card trading games
* Any game that involves card models

### SDK Functions

Developers only require six game functions to construct card games on the Hypr platform. For further information, please refer to the Solidity interface.

```solidity
// Verify proof of ownership of a game key
function verifyKeyOwnership(
    bytes params,
    bytes pubKey,
    bytes memo,
    bytes keyProof
) external view returns (bool);

// Compute aggregation public key
function computeAggregateKey(
    bytes[] pubKeys
) external view returns (bytes memory);

// Mask a playing card
function mask(
    bytes params,
    bytes sharedKey,
    bytes encoded
) external pure returns (bytes memory);

// Verify proof of shuffling of deck
function verifyShuffle(
    bytes params,
    bytes sharedKey,
    bytes[] curDeck,
    bytes[] newDeck,
    bytes shuffleProof
) external view returns (bool);

// Verify proof of reveal token of a card
function verifyReveal(
    bytes params,
    bytes pubKey,
    bytes revealToken,
    bytes masked,
    bytes revealProof
) external view returns (bool);

// Reveal a masked playing card
function reveal(
    bytes[] revealTokens,
    bytes masked
) external view returns (bytes memory);
```

### Contract Address

Above APIs will be provided via precompiled smart contract on Hypr.

<table><thead><tr><th width="191">Network</th><th>Contract Address</th></tr></thead><tbody><tr><td>Hypr Mainnet</td><td><code>0x0000000000000000000000000000000000003000</code></td></tr><tr><td>Hypr Testnet</td><td><code>0x0000000000000000000000000000000000003000</code></td></tr></tbody></table>

### Examples

Build your games on top of example game contracts to save time and effort:

<table><thead><tr><th width="273">Contract</th><th>Description</th></tr></thead><tbody><tr><td>GameInstance.sol</td><td>An example of a fundamental game contract suitable for a generic card game.</td></tr><tr><td>OneTimeDrawInstance.sol</td><td>A sample game contract that allows players to draw all their hands simultaneously, similar to Texas Hold'em.</td></tr><tr><td>TexasHoldemController.sol</td><td>A minimal implementation of Texas Hold'em using <code>OneTimeDrawInstance</code></td></tr></tbody></table>
