---
description: >-
  Hyper Poker SDK is an all-purpose tool set that enables game developers to
  easily build any on-chain card games employing zero-knowledge proof
  technology.
---

# Hypr Poker SDK

### The problem we solved

Creating card game protocols on public blockchains poses several challenges. One of the main challenges is the need for players to hide their card values from each other, whereas on-chain information is absolutely transparent due to the nature of blockchain technology. On-chain data encryption or obfuscation can be achieved using cryptographic techniques such as zero-knowledge proofs or secure multi-party computation, but these techniques can be very complicated and require a steep learning curve for game developers. And that is why we’re here to do all the heavy lifting on cryptography in Hyper blockchain and ship a set of well-designed zero-knowledge card game SDK (with complex contained) and tools so game developers can only focus on their games.

### The Solidity APIs for smart contract

After taking a good hard look at how card games work in the real world, we found a bunch of common things that they all need to do. Then the magic zero-knowledge proof tech was used to build a sweet set of APIs for card games that can handle all these common operations. We made it super easy for game developers to use our APIs by packaging them up as precompiled smart contracts that can be plugged right into their Ethereum Virtual Machine (EVM) game contracts. This way, they can focus on making their games awesome without having to worry about reinventing the wheel every time.\
\
To view the details, please refer to the Solidity Interface of [Mental](https://www.google.com/url?q=https://github.com/HyprNetwork/hypr-poker-interface/blob/master/contracts/IMentalPokerExec.sol\&sa=D\&source=editors\&ust=1695158955413170\&usg=AOvVaw2VOhFycdSrJujI1kJKEv3d) Poker Exec on Github:\
[https://github.com/HyprNetwork/hypr-poker-interface/blob/master/contracts/interface/IMentalPokerExec.sol](https://www.google.com/url?q=https://github.com/HyprNetwork/hypr-poker-interface/blob/master/contracts/interface/IMentalPokerExec.sol\&sa=D\&source=editors\&ust=1695158955413527\&usg=AOvVaw25p0uAY32r-YHnBlx-qsK1)\


| API name              | Description                    |
| --------------------- | ------------------------------ |
| computeAggregateKey() | Compute aggregation public key |
| mask()                | Mask a playing card            |
| reveal()              | Reveal a masked playing card   |

| Solidity Interface                                                                                                                                                                                                                                                                                                                                                                                                                           | Contract Address                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| [Mental](https://www.google.com/url?q=https://github.com/HyprNetwork/hypr-poker-interface/blob/master/contracts/IMentalPokerExec.sol\&sa=D\&source=editors\&ust=1695158955416868\&usg=AOvVaw1iEmyA\_yLcG97r9I9DDj-4) Poker [Exec](https://www.google.com/url?q=https://github.com/HyprNetwork/hypr-poker-interface/blob/master/contracts/IMentalPokerExec.sol\&sa=D\&source=editors\&ust=1695158955417190\&usg=AOvVaw2hiQxy5qMpJusAnZzps7PI) | 0x0000000000000000000000000000000000000040 |

To view the details, please refer to the Solidity Interface of [Mental Poker Exec on Github](https://github.com/HyprNetwork/hypr-poker-interface/blob/master/contracts/interface/IMentalPokerVerify.sol).\


| API name             | Description                             |
| -------------------- | --------------------------------------- |
| verifyKeyOwnership() | Verify proof of ownership of a game key |
| verifyShuffle()      | Verify proof of shuffling of deck       |
| verifyReveal()       | Verify proof of reveal token of a card  |

| Solidity Interface                                                                                                                                                                                                                                                                                                                                                                                                                                | Contract Address                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| [Mental](https://www.google.com/url?q=https://github.com/HyprNetwork/hypr-poker-interface/blob/master/contracts/IMentalPokerExec.sol\&sa=D\&source=editors\&ust=1695158955421945\&usg=AOvVaw0BaAnephW\_R23dbrXZNq3W) Poker [Verify](https://www.google.com/url?q=https://github.com/HyprNetwork/hypr-poker-interface/blob/master/contracts/IMentalPokerVerify.sol\&sa=D\&source=editors\&ust=1695158955422303\&usg=AOvVaw3fq6IYGY\_Uw-kwU94NmdIa) | 0x0000000000000000000000000000000000000030 |

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

To view the details, please refer to the WASM Interface on Github:\
[https://github.com/HyprNetwork/hypr-poker-interface/blob/master/scripts/interface/zkcard\_wasm\_interface\_func.d.ts](https://www.google.com/url?q=https://github.com/HyprNetwork/hypr-poker-interface/blob/master/scripts/interface/zkcard\_wasm\_interface\_func.d.ts\&sa=D\&source=editors\&ust=1695158955427828\&usg=AOvVaw1cGVRQ7XxIr2Ls\_bNGrL6H)&#x20;

[https://github.com/HyprNetwork/hypr-poker-interface/blob/master/scripts/interface/zkcard\_wasm\_interface\_type.d.ts](https://www.google.com/url?q=https://github.com/HyprNetwork/hypr-poker-interface/blob/master/scripts/interface/zkcard\_wasm\_interface\_type.d.ts\&sa=D\&source=editors\&ust=1695158955428204\&usg=AOvVaw0Vmc-XGnN5UOSS3fM8i7Y-)

To view the details, please refer to the Wasm Sdk on Github:\
[https://github.com/HyprNetwork/zkcard-wasm](https://www.google.com/url?q=https://github.com/HyprNetwork/zkcard-wasm\&sa=D\&source=editors\&ust=1695158955428561\&usg=AOvVaw3\_wLsmXm6PqCqh-68rJX80)

### What can be built with the zkGaming SDK?

The zkgaming SDK can be applied to various games that have a card model, including but not limited to poker games, card trading games, casino games, board games, and turn-based games.

[Learn more about how to use the SDK in your game](hypr-poker-sdk-usage.md).
