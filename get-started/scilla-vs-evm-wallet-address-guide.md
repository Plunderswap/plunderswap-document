# Understanding Your Zilliqa Addresses: Scilla (zil1) vs. EVM (0x)

Zilliqa operates with two distinct environments, or "sides," and understanding how your wallet addresses work across them is key to managing your assets smoothly. This guide will explain the differences between `zil1` (Scilla) and `0x` (EVM) addresses and how they interact.

## 1. Zilliqa's Two Worlds: Scilla & EVM

When you create a Zilliqa wallet using a single private key, you essentially get a "home base" on two sides of the Zilliqa network:

*   **Scilla Network:** This is the original Zilliqa environment using the Scilla smart contract language. Addresses here are in the `zil1` format (e.g., `zil1abcdef...`). While Scilla still functions, it's often referred to as the "Legacy" side as future development may focus more on EVM. You'll use this side for interacting with traditional Zilliqa dApps and tokens.
*   **Zilliqa EVM (Ethereum Virtual Machine) Network:** This side is compatible with Ethereum. Addresses here are in the `0x` format (e.g., `0x123456...`), just like on Ethereum. This allows you to use Ethereum-style dApps and tokens on Zilliqa.

## 2. Your Two Main "Operating" Addresses

Your single private key generates two primary addresses that you will use to *initiate transactions* (like sending tokens or interacting with smart contracts):

*   **Your Main Scilla Address:** A `zil1...` address. This is what you use to operate on the Scilla network (e.g., with ZilPay for Scilla-native dApps).
*   **Your Main EVM Address:** An `0x...` address. This is what you use to operate on the Zilliqa EVM network (e.g., with MetaMask for EVM-compatible dApps like PlunderSwap).

**Think of these as your active accounts for each respective side of the chain.**

## 3. "Twin" Addresses: Just for Receiving Funds

Here's where it gets interesting and is crucial to understand:

*   **Your main `zil1` (Scilla) address has a `0x` "twin" representation.**
    *   **Purpose:** This `0x` twin is *only* for receiving ZIL or ZRC-2 tokens *from* the EVM side into your Scilla wallet.
    *   **Limitation:** You **cannot** use this `0x` twin to start transactions or interact with dApps on the EVM side.
*   **Your main `0x` (EVM) address has a `zil1` "twin" representation.**
    *   **Purpose:** This `zil1` twin is *only* for receiving ZIL or ZRC-2 tokens *from* the Scilla side (or from Centralized Exchanges like Binance, KuCoin, etc., that use `zil1` addresses for Zilliqa withdrawals) into your EVM wallet.
    *   **Limitation:** You **cannot** use this `zil1` twin to start transactions or interact with dApps on the Scilla side.

**Think of these "twin" addresses as mailboxes: they can receive packages (tokens) from the other side, but you can't send mail (initiate transactions) from them on that other side.**

## 4. The Golden Rule: Where You Can "Do Stuff" (Transact)

This is the most important concept:

*   **To "do stuff" (send tokens, interact with dApps, vote) on the Scilla network, you MUST use your main `zil1...` Scilla address.**
*   **To "do stuff" on the Zilliqa EVM network, you MUST use your main `0x...` EVM address.**

You cannot use the `0x` representation of your Scilla address to transact on EVM, nor can you use the `zil1` representation of your EVM address to transact on Scilla.

## 5. Sending Tokens Between Your Two Worlds

With the above in mind, here's how you move assets:

*   **From Scilla (`zil1` wallet like ZilPay) to EVM (`0x` wallet like MetaMask):**
    *   Send ZIL or ZRC-2 tokens from your main `zil1` Scilla address.
    *   The **destination address** will be the `zil1` "twin" representation of your main `0x` EVM address.
    *   The tokens will then appear in your main `0x` EVM wallet.
    *   *(See guide: [Transfer Tokens to Zil EVM](transfer-to-EVM.md))* 
*   **From EVM (`0x` wallet like MetaMask) to Scilla (`zil1` wallet like ZilPay):**
    *   Send ZIL from your main `0x` EVM address.
    *   The **destination address** will be the `0x` "twin" representation of your main `zil1` Scilla address.
    *   The ZIL will then appear in your main `zil1` Scilla wallet.
    *   *(See guide: [Transfer ZRC Tokens from Zil EVM](transfer-from-EVM.md))* 

*(See the special case for ZRC-2 tokens on EVM below)*

## 6. Special Case: Using ZRC-2 Tokens (Scilla-native) on the EVM Side

ZRC-2 tokens (like $zUSDT, $gZIL, or other Scilla-native tokens) can be used on EVM dApps (like PlunderSwap), but they need a little help:

1.  **Send to EVM:** First, you send the ZRC-2 token from your Scilla wallet (using your main `zil1` address) to the `zil1` "twin" address of your EVM wallet. The tokens are now technically on the EVM side, associated with your main `0x` address.
2.  **The ZRC-2 Proxy Contract:** To actually *use* or *see* these ZRC-2 tokens in your EVM wallet (like MetaMask) or on an EVM dApp, you need to interact with them via a **ZRC-2 Proxy Contract**.
    *   Each ZRC-2 token that's usable on EVM has a specific proxy contract address (an `0x...` address).
    *   You add this specific proxy contract's `0x` address as a custom token in your EVM wallet (MetaMask).
    *   Once added, your EVM wallet will display your balance of that ZRC-2 token, and EVM dApps can interact with it.

**Key takeaway:** You don't just convert the ZRC-2's own `zil1` address to an `0x` to use it on EVM. You send it to your EVM wallet and then interact with it via its designated `0x` proxy contract address. (See the list of vetted [ZRC-2 Proxy Contracts](../../developers/zrc2-proxy-contracts/README.md)).

## 7. CRITICAL WARNING: ERC-20 Tokens (EVM-native)

*   **NEVER, EVER SEND `0x` ERC-20 TOKENS (tokens native to EVM chains like Ethereum, BNB Chain, Polygon, or Zilliqa EVM itself) TO A `zil1...` SCILLA ADDRESS.**
*   If you do this, your ERC-20 tokens will be **LOST FOREVER**. There is no mechanism on the Scilla side to recognize or recover them.

## 8. How Dual-Support Wallets Work (e.g., Torch, ZilPay 2.0 EVM Mobile)

Newer wallets that support *both* Scilla and Zilliqa EVM (like Torch Browser Wallet or the ZilPay 2.0 EVM Mobile App) aim to simplify this. When you import your single private key:

*   The wallet essentially manages your two "sides" or "sub-wallets" for you.
*   **EVM Mode:** When you select the EVM network or EVM mode in the wallet, it will display and use your **main `0x...` EVM address** for transactions.
*   **Scilla Mode (or "Legacy Mode"):** When you switch to the Scilla network, it will display and use your **main `zil1...` Scilla address** for transactions.

These wallets usually handle showing you the correct *operating* address for the network you've chosen. They might not always explicitly show the "twin" transfer-only addresses, but those addresses (e.g., the `zil1` representation of your EVM address) still exist and can be used for receiving funds from the other side as described above.

---

By understanding these distinctions, especially the difference between an *operating* address and a *receive-only twin* address, you can confidently navigate both sides of the Zilliqa ecosystem!
