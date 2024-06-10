# Liquidity Pools

![](../../.gitbook/assets/PS_Liquidity.png)

PlunderSwap is launching v3, implementing the Concentrated Liquidity Automated Market Maker model pioneered by Uniswap. This iteration of PlunderSwap incorporates on-chain liquidity farming within the v3 framework, resolving the difficulties of profitable liquidity provisioning by providing token incentive to the in-range liquidity positions. This model greatly improves liquidity provisioning by allowing liquidity providers to supply assets within specific price ranges, thereby enhancing capital efficiency and facilitating deeper liquidity for traders.

## Exchange V3 

In the new Exchange V3, liquidity will be managed in the form of non-fungible positions. You will still earn a share in the fees while providing liquidity.

When you add your token to a Liquidity Pool you will receive Liquidity Provider NFT tokens and share in the fees.

### **Non-fungible liquidity positions**

![](<../../.gitbook/assets/v3liquidityposition.png>)

In V3, liquidity providers now have more control over what price range they want to deploy their liquidity. So, when you add your token to a Liquidity Pool in V3, you will create a new non-fungible liquidity position with its unique settings.

Therefore, in V3, liquidity positions are NFTs. Please note that these NFTs are transferable, and they represent the ownership of the underlying assets and the trading fees they earned.

In V3, trading fees will no longer be automatically compounded in the position. You can manually claim them on each of the position detail pages.

You can redeem your funds at any time by removing your liquidity.

### **Active liquidity and price ranges**

In V3, liquidity providers can configure their positions to only provide liquidity when the price is within a certain range. If the trading price moves out of the range, the position will consist of only one type of token in the pair and become inactive.

Inactive liquidity positions will not participate in trading or earn any trading fees.

### **Concentrated liquidity**

In V3, because of liquidity providers can concentrate their token deposits to provide liquidity only within a specific price range. With the same amount of underlying assets, V3 can support a much bigger trade.

It results in a much higher relative liquidity level when compared to V2. And liquidity providers can earn more trading fees with the same amount of capital.

Here is an example:

> Pugwash and Swashbuckler both provided liquidity in ZIL/ZUSDT pool with $1,000 USD worth of token assets. The current price of ZIL is 1 USDT.
>
> Similar to PlunderSwap v2, Pugwash provided his liquidity across the entire price range. Therefore he deposited all of his capital, 500 ZUSDT and 500 ZIL.
>
> Swashbuckler utilize the new concentrated liquidity feature in PlunderSwap v3 and created a position with a price range of 0.75 to 1.5 ZUSDT per ZIL. She deposited 200 ZUSDT and 200 ZIL, worth a total of $400. She is now able to spend the remaining $600 elsewhere.
>
> As long as ZIL stays within the price range of 0.75 to 1.5, both Pugwash and Swashbuckler will receive the same amount of trading fee rewards while Swashbuckler deposited way less capital to the liquidity pool.

### **Earning trading fees**

Providing liquidity gives you a reward in the form of trading fees when people use your liquidity pool to complete swaps.

Whenever someone trades on PlunderSwap, for each hop (swap) in each Exchange V3 liquidity pool, depending on the liquidity pool fee tier, the trader pays a fee ranging from 0.01% to 1%. Their fee rates and fee breakdowns are shown as follows:

| -                       | 0.01% | 0.05% | 0.25% | 1%  |
| ----------------------- | ----- | ----- | ----- | --- |
| Liquidity Provider      | 67%   | 66%   | 68%   | 68% |
| Treasury                | 33%   | 34%   | 32%   | 32% |

For example, in a 0.25% fee tier pool:

* Among all the active (in-range) liquidity positions, there are a total of 100 ZUSDT and 100 ZIL tokens.
* Someone trades 1 ZUSDT for 1 ZIL.
* Someone else trades 1 ZIL for 1 ZUSDT.
* The liquidity providers who are in the range providing active liquidity earned a total of 0.0017 ZUSDT and 0.0017 ZIL from the trades.
* Positions with price ranges that are not covering the current price, therefore being inactive, will not contribute to trading or earn any fees.

## Exchange V2

### LP Tokens

As an example, if you deposited **ZIL** and **ZUSDT** into a Liquidity Pool, you'd receive **ZIL-ZUSDT LP** tokens.

The number of LP tokens you receive represents your portion of the ZIL-ZUSDT Liquidity Pool.

You can also redeem your funds at any time by removing your liquidity.

### **Earning trading fees**

Whenever someone trades on PlunderSwap, for each hop (swap) in each liquidity pool, the trader pays a fixed 0.35% fee, **of which 0.20%** is added back to the Liquidity Pool in a form of trading fees.  The remaining 0.15% fee is sent to the Plunderswap treasury to keep the vessel sailing smoothly into the future!

##

## Impermanent Loss

Providing liquidity is not without risk, as you may be exposed to impermanent loss.

[“Simply put, impermanent loss is the difference between holding tokens in an AMM and holding them in your wallet.” - Nate Hindman](https://blog.bancor.network/beginners-guide-to-getting-rekt-by-impermanent-loss-7c9510cb2f22)
