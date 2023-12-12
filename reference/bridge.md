---
description: >-
  The Hypr bridge allows users to move assets between Ethereum (L1) and Hypr
  (L2) networks.
---

# Bridge

| Testnet                                                                      | Mainnet                                                      |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------ |
| [https://testnet-bridge.hypr.network/](https://testnet-bridge.hypr.network/) | [https://bridge.hypr.network/](https://bridge.hypr.network/) |

{% hint style="info" %}
Please note. Bridging assets to and from Hypr requires gas fees in $ETH. Be sure to have sufficient $ETH in your wallet to cover the cost of the fees.
{% endhint %}

## Deposit: Moving assets from Ethereum -> Hypr

Moving assets from Ethereum network to Hypr network is straightforward. Simply connect your Metamask wallet, then select the Deposit tab, select the asset type, enter the amount and click the Deposit button. Here is an example of moving 0.01 $ETH from Ethereum over to Hypr:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Moving $ETH from Ethereum to Hypr</p></figcaption></figure>

You can use the asset dropdown to select other asset types. Here's an example showing how to move 150 $HYPR from Ethereum over to Hypr:

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>Moving $HYPR from Ethereum to Hypr</p></figcaption></figure>

## Withdraw: Moving assets from Hypr -> Ethereum

The withdrawal process is the same as deposits, in that you simply select the Withdraw tab, select the asset type, enter the amout to withdraw, and click the Withdraw button. This will move your selected assets from the Hypr network over to the Ethereum network after the **7-day** challenge period.

There are a few extra steps required to complete your withdrawal process:

1. Enter the amount, click the Withdraw button and sign the transaction with Metamask.
2. Go to "View Withdrawals" and find your transaction (Up to 30 minutes later).
3. Once confirmed, press the "Prove" button.
4. Wait for 7-day challenge period.
5. After 7 days, press the "Claim" button to complete the transaction and receive your funds.

{% hint style="warning" %}
It is very important to complete all steps above to claim your funds - especially step 5.
{% endhint %}

## Understanding the 7-day Challenge Period

The one week challenge period is part of the OP Stack design:

> One of the most important things to understand about L1 ⇔ L2 interaction is that mainnet messages sent from Layer 2 to Layer 1 cannot be relayed for at least one week. This means that any messages you send from Layer 2 will only be received on Layer 1 after this one week period has elapsed. We call this period of time the "challenge period" because it is the time during which a transaction can be challenged with a fault proof.

You can learn more about this process here:

{% embed url="https://community.optimism.io/docs/developers/bridge/messaging/#understanding-the-challenge-period" %}
Understanding the Challenge Period
{% endembed %}
