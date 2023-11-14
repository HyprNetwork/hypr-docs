---
description: Learn how to use the Hypr Cards SDK
---

# Hypr Cards SDK Usage

### Install modules

Begin by installing and importing typescript packages `hypr-poker-export` and `ethers`

```sh
yarn add hypr-poker-export
yarn add ethers
```

### Import modules

```typescript
import * as precompile from 'hypr-poker-export/precompile';
import * as wasm from 'hypr-poker-export/wasm';
import { Contract, JsonRpcProvider, Wallet, getBytes } from 'ethers';
const IMentalPokerExecAbi = require('hypr-poker-export/abi/IMentalPokerExec.json');
const IMentalPokerVerifyAbi = require('hypr-poker-export/abi/IMentalPokerVerify.json');
```

### Contract Objects

Next, you need to create a contract object of **Poker Exec** and **Poker Verify** precompile contracts.

You will need to update the [provider RPC url ](../../reference/network-settings.md)and a wallet key.

{% hint style="warning" %}
Never hardcode your wallet private key.
{% endhint %}

```typescript
const provider = new JsonRpcProvider('http://testnet-proposer0.hypr.network:8545');
const signer = new Wallet('<your secret key>', provider);

const pokerExec = new Contract(
  '0x0000000000000000000000000000000000000040',
  IMentalPokerExecAbi,
  signer,
) as unknown as precompile.IMentalPokerExec;

const pokerVerify = new Contract(
  '0x0000000000000000000000000000000000000030',
  IMentalPokerVerifyAbi,
  signer,
) as unknown as precompile.IMentalPokerVerify;
```

### Game Parameters

Before beginning the game, you must generate game parameters and cards for on-chain contract computation, player key, and keyproof for off-chain player computation.

```typescript
// Generator game parameter.
const m = 4;
const n = 13;
const rand = wasm.CardRand.buildRand();
const parameter = wasm.setup(rand, m, n);


// Generate origin cards and encoded origin cards.
const cards = wasm.encodeCards(rand, m * n);
const cardsEncoded: wasm.Card[] = [];
const cards_len = cards.len();
for (let i = 0; i < cards_len; i++) {
  cardsEncoded.push(wasm.Card.deserial(cards.pop().serial()));
}


// Generate game keys and keyproof for player Alice.
const Alice = wasm.PlayerName.NewPlayerName('Alice');
const gameKeyAndProofAlice = wasm.keygen(rand, parameter, Alice);
const pubKeyAlice = gameKeyAndProofAlice.getPubKey();
const secKeyAlice = gameKeyAndProofAlice.getSecKey();
const keyownershipProofAlice = gameKeyAndProofAlice.getProof();


// Generate game key and proof for player Peter.
const Peter = wasm.PlayerName.NewPlayerName('Peter');
const gameKeyAndProofPeter = wasm.keygen(rand, parameter, Peter);
const pubKeyPeter = gameKeyAndProofPeter.getPubKey();
const secKeyPeter = gameKeyAndProofPeter.getSecKey();
const keyownershipProofPeter = gameKeyAndProofPeter.getProof();
```

{% hint style="info" %}
Integers `n*m` must equal to the number of cards. e.g. `4 * 13 = 52.`

The need for two variables is due to wasm execution performance optimization.
{% endhint %}

### Shuffle Cards

If you want to shuffle cards on-chain safely, you are able to call function `verifyKeyOwnership()` -> `computeAggregateKey()` -> `mask()` -> `shuffleAndRemask()` -> `verifyShuffle()`

```typescript
// Verify player Alice keyproof.
if(!await pokerVerify.verifyKeyOwnership(
  parameter.serial(), 
  pubKeyAlice.serial(), 
  Alice.serial(), 
  keyownershipProofAlice.serial()
  )) {
    throw 'poker verify ownership is failed';
}


// Verify player Peter keyproof.
if(!await pokerVerify.verifyKeyOwnership(
  parameter.serial(), 
  pubKeyPeter.serial(), 
  Peter.serial(), 
  keyownershipProofPeter.serial()
  )) {
    throw 'poker verify ownership is failed';
}


// Compute aggregate key.
const pubKeys : Uint8Array[] = [];
pubKeys.push(pubKeyAlice.serial());
pubKeys.push(pubKeyPeter.serial());

const aggregatedKey = wasm.AggregatePublicKey.deserial(
  getBytes(await pokerExec.computeAggregateKey(pubKeys)),
);


const uint8ArrayEquals = (a: Uint8Array, b: Uint8Array) =>
  a.length === b.length && a.every((v, i) => v === b[i]);

// Mask cards.
const maskedCards = wasm.VMaskedCard.newVMaskedCard();
for (let i = 0; i < cardsEncoded.length; i++) {
  // mask() through precompile on-chain.
  const maskedCardOnChain = getBytes(await pokerExec.mask(
      parameter.serial(),
      aggregatedKey.serial(),
      cardsEncoded[i].serial(),
  ));
  // mask() through wasm off-chain.
  const maskedCardOffChain = wasm
    .mask(parameter, aggregatedKey, cardsEncoded[i]).serial();

  if(!uint8ArrayEquals(maskedCardOnChain, maskedCardOffChain)) {
    throw 'poker mask error';
  }
  maskedCards.push(wasm.MaskedCard.deserial(maskedCardOnChain));
}


// Player Alice shuffle cards and verify shuffled cards.
const randAlice = wasm.CardRand.buildRand();
const permutationAlice = wasm.Permutation.newPermutation(randAlice, m * n);
const shuffledCardsAndShuffleProofAlice = wasm.shuffleAndRemask(
  randAlice,
  parameter,
  aggregatedKey,
  maskedCards,
  permutationAlice,
);

const maskedCardsSerialed: Uint8Array[] = [];
const maskedCards_len = maskedCards.len();
for (let i = 0; i < maskedCards_len; i++) {
  maskedCardsSerialed.push(maskedCards.pop().serial());
}

let shuffledCardsAlice = shuffledCardsAndShuffleProofAlice.getMaskedCards();
const shuffleProofAlice = shuffledCardsAndShuffleProofAlice.getShuffleProof();

const shuffledCardsAliceSerial: Uint8Array[] = [];
const shuffledCardsAlice_len = shuffledCardsAlice.len();
for (let i = 0; i < shuffledCardsAlice_len; i++) {
  shuffledCardsAliceSerial.push(shuffledCardsAlice.pop().serial());
}

shuffledCardsAlice = shuffledCardsAndShuffleProofAlice.getMaskedCards();

if(!await pokerVerify.verifyShuffle(
  parameter.serial(),
  aggregatedKey.serial(),
  maskedCardsSerialed,
  shuffledCardsAliceSerial,
  shuffleProofAlice.serial(),
)) {
  throw 'poker verify shuffle failed';
}


// Player Peter shuffle cards and verify shuffled cards.
const randPeter = wasm.CardRand.buildRand();
const permutationPeter = wasm.Permutation.newPermutation(randPeter, m * n);
const shuffledCardsAndShuffleProofPeter = wasm.shuffleAndRemask(
  randPeter,
  parameter,
  aggregatedKey,
  shuffledCardsAlice,
  permutationPeter,
);

let shuffledCardsPeter = shuffledCardsAndShuffleProofPeter.getMaskedCards();
const shuffleProofPeter = shuffledCardsAndShuffleProofPeter.getShuffleProof();

const shuffledCardsPeterSerial: Uint8Array[] = [];
const shuffledCardsPeter_len = shuffledCardsPeter.len();
for (let i = 0; i < shuffledCardsPeter_len; i++) {
  shuffledCardsPeterSerial.push(shuffledCardsPeter.pop().serial());
}

shuffledCardsPeter = shuffledCardsAndShuffleProofPeter.getMaskedCards();

if(!await pokerVerify.verifyShuffle(
  parameter.serial(),
  aggregatedKey.serial(),
  shuffledCardsAliceSerial,
  shuffledCardsPeterSerial,
  shuffleProofPeter.serial(),
)) {
  throw 'poker verify shuffle failed';
}
```

### Reaveal Cards

If you want to reveal cards from mask-shuffled cards on-chain safely, you are able to call function `computeRevealToken()` -> `verifyReveal()` -> `reveal()`

```typescript
// Get one of mask-shuffled cards, used for reveal.
const shuffledCard = shuffledCardsPeter.pop();

// Compute reveal token for Alice.
const randRevealAlice = wasm.CardRand.buildRand();
const revealTokenAndProofAlice = wasm.computeRevealToken(
  randRevealAlice,
  parameter,
  secKeyAlice,
  pubKeyAlice,
  shuffledCard,
);
const revealTokenAlice = revealTokenAndProofAlice.getRevealToken();
const revealProofAlice = revealTokenAndProofAlice.getRevealProof();

// Compute reveal token for Peter
const randRevealPeter = wasm.CardRand.buildRand();
const revealTokenAndProofPeter = wasm.computeRevealToken(
  randRevealPeter,
  parameter,
  secKeyPeter,
  pubKeyPeter,
  shuffledCard,
);
const revealTokenPeter = revealTokenAndProofPeter.getRevealToken();
const revealProofPeter = revealTokenAndProofPeter.getRevealProof();


// Verify reveal token of Alice can reveal specified mask-shuffled card.
if(!await pokerVerify.verifyReveal(
    parameter.serial(),
    pubKeyAlice.serial(),
    revealTokenAlice.serial(),
    shuffledCard.serial(),
    revealProofAlice.serial(),
)) {
  throw 'poker verify reveal failed';
}

// Verify reveal token of Peter can reveal specified mask-shuffled card.
if(!await pokerVerify.verifyReveal(
    parameter.serial(),
    pubKeyPeter.serial(),
    revealTokenPeter.serial(),
    shuffledCard.serial(),
    revealProofPeter.serial(),
)) {
  throw 'poker verify reveal failed';
}


// Reveal mask-shuffled card with reveal tokens.
const revealTokens = wasm.VRevealToken.newVRevealToken();
revealTokens.push(revealTokenAlice);
revealTokens.push(revealTokenPeter);

const revealTokensSerial = [];
revealTokensSerial.push(revealTokenAlice.serial());
revealTokensSerial.push(revealTokenPeter.serial());

const cards1 = wasm.reveal(revealTokens, shuffledCard).serial();
const cards2 = getBytes(
  await pokerExec.reveal(
    revealTokensSerial,
    shuffledCard.serial()
));
if(!uint8ArrayEquals(cards1, cards2)) {
    throw 'poker reveal error';
}
```

{% hint style="warning" %}
Note: The function `shuffleAndRemask()` has to be called **after** `mask()`.
{% endhint %}
