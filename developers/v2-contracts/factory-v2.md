# Factory v2

{% hint style="warning" %}
PlunderSwap is based on Uniswap v2. Read the [Uniswap v2 documentation](https://docs.uniswap.org/contracts/v2/overview).\
For more in-depth information on the core contract logic, read the [Uniswap v2 Core whitepaper](https://uniswap.org/whitepaper.pdf).
{% endhint %}

## Contract info

**Contract name:** PlunderSwapFactory

View [PlunderSwapFactory.sol on GitHub](https://github.com/Plunderswap/plunderswap-core/blob/master/contracts/PlunderFactory.sol).
All contracts are verified on [https://sourcify.dev/#/lookup](https://sourcify.dev/#/lookup).  Ziliqas new block explorer (Otterscan, coming soon to mainnet), will also take data from sourcify.

**Zilliqa EVM MainNet**\
* [Factory](https://evmx.zilliqa.com/contract/0xf42d1058f233329185A36B04B7f96105afa1adD2?tab=transactions)

## Read functions

### getPair

`function getPair(address tokenA, address tokenB) external view returns (address pair);`

Address for `tokenA` and address for `tokenB` return address of pair contract (where one exists).

`tokenA` and `tokenB` order is interchangeable.

Returns `0x0000000000000000000000000000000000000000` as address where no pair exists.

### allPairs

`function allPairs(uint) external view returns (address pair);`

Returns the address of the `n`th pair (`0`-indexed) created through the Factory contract.

Returns `0x0000000000000000000000000000000000000000` where pair has not yet been created.

Begins at `0` for first created pair.

### allPairsLength

`function allPairsLength() external view returns (uint);`

Displays the current number of pairs created through the Factory contract as an integer.

### feeTo

`function feeTo() external view returns (address);`

The address to where non-LP-holder fees are sent.

### feeToSetter

`function feeToSetter() external view returns (address);`

The address with permission to set the feeTo address.

## Write functions

### createPair

function createPair(address tokenA, address tokenB) external returns (address pair);

Creates a pair for `tokenA` and `tokenB` where a pair doesn't already exist.

`tokenA` and `tokenB` order is interchangeable.

Emits `PairCreated` (see Events).

### setFeeTo

Sets address for `feeTo`.

### setFeeToSetter

Sets address for permission to adjust `feeTo`.

## Events

### PairCreated

`event PairCreated(address indexed token0, address indexed token1, address pair, uint);`

Emitted whenever a `createPair` creates a new pair.

`token0` will appear before `token1` in sort order.

The final `uint` log value will be `1` for the first pair created, `2` for the second, etc.

## Interface

```
import '@uniswap/v2-core/contracts/interfaces/IPlunderSwapFactory.sol';
```

```
pragma solidity =0.5.16;


interface IPlunderSwapFactory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}
```
