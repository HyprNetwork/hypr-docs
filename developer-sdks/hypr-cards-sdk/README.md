---
description: >-
  Hyper Cards SDK is an all-purpose tool set that enables game developers to
  easily build any on-chain card games employing zero-knowledge proof
  technology.
---

# ♠ Hypr Cards SDK

### The problem we solved

Creating card game protocols on public blockchains poses several challenges. One of the main challenges is the need for players to hide their card values from each other, whereas on-chain information is absolutely transparent due to the nature of blockchain technology. On-chain data encryption or obfuscation can be achieved using cryptographic techniques such as zero-knowledge proofs or secure multi-party computation, but these techniques can be very complicated and require a steep learning curve for game developers. And that is why we’re here to do all the heavy lifting on cryptography in Hyper blockchain and ship a set of well-designed zero-knowledge card game SDK (with complex contained) and tools so game developers can only focus on their games.

### The Solidity APIs for smart contract

After taking a good hard look at how card games work in the real world, we found a bunch of common things that they all need to do. Then the magic zero-knowledge proof tech was used to build a sweet set of APIs for card games that can handle all these common operations. We made it super easy for game developers to use our APIs by packaging them up as precompiled smart contracts that can be plugged right into their Ethereum Virtual Machine (EVM) game contracts. This way, they can focus on making their games awesome without having to worry about reinventing the wheel every time.

Dive in and look at a sample or continue reading below.

{% content-ref url="hypr-cards-sdk-usage.md" %}
[hypr-cards-sdk-usage.md](hypr-cards-sdk-usage.md)
{% endcontent-ref %}

| API name              | Description                    |
| --------------------- | ------------------------------ |
| computeAggregateKey() | Compute aggregation public key |
| mask()                | Mask a playing card            |
| reveal()              | Reveal a masked playing card   |

| Solidity Interface | Contract Address                           |
| ------------------ | ------------------------------------------ |
| Mental Poker Exec  | 0x0000000000000000000000000000000000000040 |



| API name             | Description                             |
| -------------------- | --------------------------------------- |
| verifyKeyOwnership() | Verify proof of ownership of a game key |
| verifyShuffle()      | Verify proof of shuffling of deck       |
| verifyReveal()       | Verify proof of reveal token of a card  |

| Solidity Interface  | Contract Address                           |
| ------------------- | ------------------------------------------ |
| Mental Poker Verify | 0x0000000000000000000000000000000000000030 |

The Typescript/Javascript API for game client

| API name             | Description                                                                |
| -------------------- | -------------------------------------------------------------------------- |
| keygen()             | Create a random game key pair                                              |
| setup()              | Create a public parameter to setup game instance                           |
| encodeCards()        | Generate an initial encoded deck of playing cards                          |
| mask()               | Mask an encoded card                                                       |
| shuffleAndRemask()   | Shuffle and remask the deck of cards, off-chain, with a random permutation |
| computeRevealToken() | Compute a reveal token for a given masked card                             |
| reveal()             | Reveal a masked card to see its original value                             |



### What can be built with the zkGaming SDK?

The zkgaming SDK can be applied to various games that have a card model, including but not limited to poker games, card trading games, casino games, board games, and turn-based games.

{% content-ref url="hypr-cards-sdk-usage.md" %}
[hypr-cards-sdk-usage.md](hypr-cards-sdk-usage.md)
{% endcontent-ref %}
