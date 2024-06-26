# Router v2

{% hint style="warning" %}
PlunderSwap is based on Uniswap v2. Read the [Uniswap v2 documentation](https://docs.uniswap.org/contracts/v2/overview).\
For more in-depth information on the core contract logic, read the [Uniswap v2 Core whitepaper](https://github.com/Uniswap/docs/blob/main/static/whitepaper.pdf).
{% endhint %}

## Contract info

**Contract name:** PlunderSwapRouter

View [PlunderSwapRouter.sol on GitHub](https://github.com/Plunderswap/plunderswap-core/blob/master/contracts/PlunderRouter.sol).
All contracts are verified on [https://sourcify.dev/#/lookup](https://sourcify.dev/#/lookup).  Ziliqas new block explorer (Otterscan, coming soon to mainnet), will also take data from sourcify.

**Zilliqa EVM Mainnet**\
* [Router](https://evmx.zilliqa.com/contract/0x33C6a20D2a605da9Fd1F506ddEd449355f0564fe?tab=transactions)


## Read functions

### getAmountOut

`function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) internal pure returns (uint amountOut);`

### getAmountIn

`function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) internal pure returns (uint amountIn);`

### getAmountsOut

`function getAmountsOut(uint amountIn, address[] memory path) internal view returns (uint[] memory amounts);`

### getAmountsIn

`function getAmountsIn(uint amountOut, address[] memory path) internal view returns (uint[] memory amounts);`

### quote

`function quote(uint amountA, uint reserveA, uint reserveB) internal pure returns (uint amountB);`

## Write functions

### addLiquidity

```
function addLiquidity(
  address tokenA,
  address tokenB,
  uint amountADesired,
  uint amountBDesired,
  uint amountAMin,
  uint amountBMin,
  address to,
  uint deadline
) external returns (uint amountA, uint amountB, uint liquidity);
```

Adds liquidity to a ERC-20/ZRC-2⇄ERC-20/ZRC-2 pool.

| Name           | Type      | Description                                                       |
| -------------- | --------- | ----------------------------------------------------------------- |
| tokenA         | `address` | The contract address of one token from your liquidity pair.       |
| tokenB         | `address` | The contract address of the other token from your liquidity pair. |
| amountADesired | `uint`    | The amount of tokenA you'd like to provide as liquidity.          |
| amountBDesired | `uint`    | The amount of tokenA you'd like to provide as liquidity.          |
| amountAMin     | `uint`    | The minimum amount of tokenA to provide (slippage impact).        |
| amountBMin     | `uint`    | The minimum amount of tokenB to provide (slippage impact).        |
| to             | `address` | Address of LP Token recipient.                                    |
| deadline       | `uint`    | Unix timestamp deadline by which the transaction must confirm.    |

### addLiquidityETH

```
function addLiquidityETH(
  address token,
  uint amountTokenDesired,
  uint amountTokenMin,
  uint amountETHMin,
  address to,
  uint deadline
) external payable returns (uint amountToken, uint amountETH, uint liquidity);
```

Adds liquidity to a ERC-20/ZRC-2⇄WZIL pool.

| Name               | Type      |                                                                |
| ------------------ | --------- | -------------------------------------------------------------- |
| addLiquidityETH    | `uint`    | The payable amount in ZIL.                                     |
| token              | `address` | The contract address of the token to add liquidity.            |
| amountTokenDesired | `uint`    | The amount of the token you'd like to provide as liquidity.    |
| amountTokenMin     | `uint`    | The minimum amount of the token to provide (slippage impact).  |
| amountETHMin       | `uint`    | The minimum amount of ZIL to provide (slippage impact).        |
| to                 | `address` | Address of LP Token recipient.                                 |
| deadline           | `uint`    | Unix timestamp deadline by which the transaction must confirm. |

### removeLiquidity

```
function removeLiquidity(
  address tokenA,
  address tokenB,
  uint liquidity,
  uint amountAMin,
  uint amountBMin,
  address to,
  uint deadline
) external returns (uint amountA, uint amountB);
```

Removes liquidity from a ERC-20/ZRC-2⇄ERC-20/ZRC-2 pool.

| Name       | Type      |                                                                   |
| ---------- | --------- | ----------------------------------------------------------------- |
| tokenA     | `address` | The contract address of one token from your liquidity pair.       |
| tokenB     | `address` | The contract address of the other token from your liquidity pair. |
| liquidity  | `uint`    | The amount of LP Tokens to remove.                                |
| amountAMin | `uint`    | The minimum amount of tokenA to remove (slippage impact).         |
| amountBMin | `uint`    | The minimum amount of tokenB to remove (slippage impact).         |
| to         | `address` | Address of LP Token recipient.                                    |
| deadline   | `uint`    | Unix timestamp deadline by which the transaction must confirm.    |

### removeLiquidityETH

```
function removeLiquidityETH(
  address token,
  uint liquidity,
  uint amountTokenMin,
  uint amountETHMin,
  address to,
  uint deadline
) external returns (uint amountToken, uint amountETH);
```

Removes liquidity from a ERC-20/ZRC-2⇄WZIL pool.

| Name           | Type      |                                                                |
| -------------- | --------- | -------------------------------------------------------------- |
| token          | `address` | The contract address of the token to remove liquidity.         |
| liquidity      | `uint`    | The amount of LP Tokens to remove.                             |
| amountTokenMin | `uint`    | The minimum amount of the token to remove (slippage impact).   |
| amountETHMin   | `uint`    | The minimum amount of ZIL to remove (slippage impact).         |
| to             | `address` | Address of LP Token recipient.                                 |
| deadline       | `uint`    | Unix timestamp deadline by which the transaction must confirm. |

### removeLiquidityETHSupportingFeeOnTransferTokens

```
function removeLiquidityETHSupportingFeeOnTransferTokens(
  address token,
  uint liquidity,
  uint amountTokenMin,
  uint amountETHMin,
  address to,
  uint deadline
) external returns (uint amountETH);
```

Removes liquidity from a ERC-20/ZRC-2⇄WZIL for tokens that take a fee on transfer.

| Name           | Type      |                                                                |
| -------------- | --------- | -------------------------------------------------------------- |
| token          | `address` | The contract address of the token to remove liquidity.         |
| liquidity      | `uint`    | The amount of LP Tokens to remove.                             |
| amountTokenMin | `uint`    | The minimum amount of the token to remove (slippage impact).   |
| amountETHMin   | `uint`    | The minimum amount of ZIL to remove (slippage impact).         |
| to             | `address` | Address of LP Token recipient.                                 |
| deadline       | `uint`    | Unix timestamp deadline by which the transaction must confirm. |

### removeLiquidityETHWithPermit

```
function removeLiquidityETHWithPermit(
  address token,
  uint liquidity,
  uint amountTokenMin,
  uint amountETHMin,
  address to,
  uint deadline,
  bool approveMax, uint8 v, bytes32 r, bytes32 s
) external returns (uint amountToken, uint amountETH);
```

Removes liquidity from a ERC-20/ZRC-2⇄WZIL and receives ZIL, without pre-approval, via permit.

| Name           | Type      |                                                                                     |
| -------------- | --------- | ----------------------------------------------------------------------------------- |
| token          | `address` | The contract address of the token to remove liquidity.                              |
| liquidity      | `uint`    | The amount of LP Tokens to remove.                                                  |
| amountTokenMin | `uint`    | The minimum amount of the token to remove (slippage impact).                        |
| amountETHMin   | `uint`    | The minimum amount of ZIL to remove (slippage impact).                              |
| to             | `address` | Address of LP Token recipient.                                                      |
| deadline       | `uint`    | Unix timestamp deadline by which the transaction must confirm.                      |
| approveMax     | `bool`    | Whether or not the approval amount in the signature is for liquidity or `uint(-1)`. |
| v              | `uint8`   | The v component of the permit signature.                                            |
| r              | `bytes32` | The r component of the permit signature.                                            |
| s              | `bytes32` | The s component of the permit signature.                                            |

### removeLiquidityETHWithPermitSupportingFeeOnTransferTokens

```
function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
  address token,
  uint liquidity,
  uint amountTokenMin,
  uint amountETHMin,
  address to,
  uint deadline,
  bool approveMax, uint8 v, bytes32 r, bytes32 s
) external returns (uint amountETH);
```

Removes liquidity from a ERC-20/ZRC-2⇄WZIL and receives ZIL via permit for tokens that take a fee on transfer.

| Name           | Type      |                                                                                     |
| -------------- | --------- | ----------------------------------------------------------------------------------- |
| token          | `address` | The contract address of the token to remove liquidity.                              |
| liquidity      | `uint`    | The amount of LP Tokens to remove.                                                  |
| amountTokenMin | `uint`    | The minimum amount of the token to remove (slippage impact).                        |
| amountETHMin   | `uint`    | The minimum amount of ZIL to remove (slippage impact).                              |
| to             | `address` | Address of LP Token recipient.                                                      |
| deadline       | `uint`    | Unix timestamp deadline by which the transaction must confirm.                      |
| approveMax     | `bool`    | Whether or not the approval amount in the signature is for liquidity or `uint(-1)`. |
| v              | `uint8`   | The v component of the permit signature.                                            |
| r              | `bytes32` | The r component of the permit signature.                                            |
| s              | `bytes32` | The s component of the permit signature.                                            |

### removeLiquidityWithPermit

```
function removeLiquidityWithPermit(
  address tokenA,
  address tokenB,
  uint liquidity,
  uint amountAMin,
  uint amountBMin,
  address to,
  uint deadline,
  bool approveMax, uint8 v, bytes32 r, bytes32 s
) external returns (uint amountA, uint amountB);
```

Removes liquidity from a ERC-20/ZRC-2⇄ERC-20/ZRC-2, without pre-approval, via permit.

| Name           | Type      |                                                                                     |
| -------------- | --------- | ----------------------------------------------------------------------------------- |
| tokenA         | `address` | The contract address of one token from your liquidity pair.                         |
| tokenB         | `address` | The contract address of the other token from your liquidity pair.                   |
| liquidity      | `uint`    | The amount of LP Tokens to remove.                                                  |
| amountTokenMin | `uint`    | The minimum amount of the token to remove (slippage impact).                        |
| amountETHMin   | `uint`    | The minimum amount of ZIL to remove (slippage impact).                              |
| to             | `address` | Address of LP Token recipient.                                                      |
| deadline       | `uint`    | Unix timestamp deadline by which the transaction must confirm.                      |
| approveMax     | `bool`    | Whether or not the approval amount in the signature is for liquidity or `uint(-1)`. |
| v              | `uint8`   | The v component of the permit signature.                                            |
| r              | `bytes32` | The r component of the permit signature.                                            |
| s              | `bytes32` | The s component of the permit signature.                                            |

### swapETHForExactTokens

```
function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
  external
  payable
  returns (uint[] memory amounts);
```

Receive an exact amount of output tokens for as little ZIL as possible.

| Name                  | Type      |                                                                                                                                      |
| --------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| swapETHForExactTokens | `uint`    | Payable ZIL amount.                                                                                                                  |
| amountOut             | `uint`    | The amount tokens to receive.                                                                                                        |
| path (address\[])     | `address` | An array of token addresses. `path.length` must be >= 2. Pools for each consecutive pair of addresses must exist and have liquidity. |
| to                    | `address` | Address of recipient.                                                                                                                |
| deadline              | `uint`    | Unix timestamp deadline by which the transaction must confirm.                                                                       |

### swapExactETHForTokens

```
function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
  external
  payable
  returns (uint[] memory amounts);
```

Receive as many output tokens as possible for an exact amount of ZIL.

| Name                  | Type      |                                                                                                                                      |
| --------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| swapExactETHForTokens | `uint`    | Payable ZIL amount.                                                                                                                  |
| amountOutMin          | `uint`    | The minimum amount tokens to receive.                                                                                                |
| path (address\[])     | `address` | An array of token addresses. `path.length` must be >= 2. Pools for each consecutive pair of addresses must exist and have liquidity. |
| to                    | `address` | Address of recipient.                                                                                                                |
| deadline              | `uint`    | Unix timestamp deadline by which the transaction must confirm.                                                                       |

### swapExactETHForTokensSupportingFeeOnTransferTokens

```
function swapExactETHForTokensSupportingFeeOnTransferTokens(
  uint amountOutMin,
  address[] calldata path,
  address to,
  uint deadline
) external payable;
```

Receive as many output tokens as possible for an exact amount of ZIL. Supports tokens that take a fee on transfer.

| Name                                               | Type      |                                                                                                                                      |
| -------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| swapExactETHForTokensSupportingFeeOnTransferTokens | `uint`    | Payable ZIL amount.                                                                                                                  |
| amountOutMin                                       | `uint`    | The minimum amount tokens to receive.                                                                                                |
| path (address\[])                                  | `address` | An array of token addresses. `path.length` must be >= 2. Pools for each consecutive pair of addresses must exist and have liquidity. |
| to                                                 | `address` | Address of recipient.                                                                                                                |
| deadline                                           | `uint`    | Unix timestamp deadline by which the transaction must confirm.                                                                       |

### swapExactTokensForETH

```
function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
  external
  returns (uint[] memory amounts);
```

Receive as much ZIL as possible for an exact amount of input tokens.

| Name              | Type      |                                                                                                                                      |
| ----------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| amountIn          | `uint`    | Payable amount of input tokens.                                                                                                      |
| amountOutMin      | `uint`    | The minimum amount tokens to receive.                                                                                                |
| path (address\[]) | `address` | An array of token addresses. `path.length` must be >= 2. Pools for each consecutive pair of addresses must exist and have liquidity. |
| to                | `address` | Address of recipient.                                                                                                                |
| deadline          | `uint`    | Unix timestamp deadline by which the transaction must confirm.                                                                       |

### swapExactTokensForETHSupportingFeeOnTransferTokens

```
function swapExactTokensForETHSupportingFeeOnTransferTokens(
  uint amountIn,
  uint amountOutMin,
  address[] calldata path,
  address to,
  uint deadline
) external;
```

Receive as much ZIL as possible for an exact amount of tokens. Supports tokens that take a fee on transfer.

| Name              | Type      |                                                                                                                                      |
| ----------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| amountIn          | `uint`    | Payable amount of input tokens.                                                                                                      |
| amountOutMin      | `uint`    | The minimum amount tokens to receive.                                                                                                |
| path (address\[]) | `address` | An array of token addresses. `path.length` must be >= 2. Pools for each consecutive pair of addresses must exist and have liquidity. |
| to                | `address` | Address of recipient.                                                                                                                |
| deadline          | `uint`    | Unix timestamp deadline by which the transaction must confirm.                                                                       |

### swapExactTokensForTokens

```
function swapExactTokensForTokens(
  uint amountIn,
  uint amountOutMin,
  address[] calldata path,
  address to,
  uint deadline
) external returns (uint[] memory amounts);
```

Receive as many output tokens as possible for an exact amount of input tokens.

| Name              | Type      |                                                                                                                                      |
| ----------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| amountIn          | `uint`    | Payable amount of input tokens.                                                                                                      |
| amountOutMin      | `uint`    | The minimum amount tokens to receive.                                                                                                |
| path (address\[]) | `address` | An array of token addresses. `path.length` must be >= 2. Pools for each consecutive pair of addresses must exist and have liquidity. |
| to                | `address` | Address of recipient.                                                                                                                |
| deadline          | `uint`    | Unix timestamp deadline by which the transaction must confirm.                                                                       |

### swapExactTokensForTokensSupportingFeeOnTransferTokens

```
function swapExactTokensForTokensSupportingFeeOnTransferTokens(
  uint amountIn,
  uint amountOutMin,
  address[] calldata path,
  address to,
  uint deadline
) external;
```

Receive as many output tokens as possible for an exact amount of input tokens. Supports tokens that take a fee on transfer.

| Name              | Type      |                                                                                                                                      |
| ----------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| amountIn          | `uint`    | Payable amount of input tokens.                                                                                                      |
| amountOutMin      | `uint`    | The minimum amount tokens to receive.                                                                                                |
| path (address\[]) | `address` | An array of token addresses. `path.length` must be >= 2. Pools for each consecutive pair of addresses must exist and have liquidity. |
| to                | `address` | Address of recipient.                                                                                                                |
| deadline          | `uint`    | Unix timestamp deadline by which the transaction must confirm.                                                                       |

### swapTokensForExactETH

```
function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
  external
  returns (uint[] memory amounts);
```

Receive an exact amount of ETH for as few input tokens as possible.

| Name              | Type      |                                                                                                                                      |
| ----------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| amountOut         | `uint`    | Payable amount of input tokens.                                                                                                      |
| amountInMax       | `uint`    | The minimum amount tokens to input.                                                                                                  |
| path (address\[]) | `address` | An array of token addresses. `path.length` must be >= 2. Pools for each consecutive pair of addresses must exist and have liquidity. |
| to                | `address` | Address of recipient.                                                                                                                |
| deadline          | `uint`    | Unix timestamp deadline by which the transaction must confirm.                                                                       |

### swapTokensForExactTokens

```
function swapTokensForExactTokens(
  uint amountOut,
  uint amountInMax,
  address[] calldata path,
  address to,
  uint deadline
) external returns (uint[] memory amounts);
```

Receive an exact amount of output tokens for as few input tokens as possible.

| Name              | Type      |                                                                                                                                      |
| ----------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| amountOut         | `uint`    | Payable amount of input tokens.                                                                                                      |
| amountInMax       | `uint`    | The maximum amount tokens to input.                                                                                                  |
| path (address\[]) | `address` | An array of token addresses. `path.length` must be >= 2. Pools for each consecutive pair of addresses must exist and have liquidity. |
| to                | `address` | Address of recipient.                                                                                                                |
| deadline          | `uint`    | Unix timestamp deadline by which the transaction must confirm.                                                                       |

## Interface

```
import '@uniswap/v2-core/contracts/interfaces/IPlunderSwapRouter.sol';
```

```
pragma solidity >=0.6.2;

interface IPlunderSwapRouter01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

// File: contracts\interfaces\IPlunderSwapRouter02.sol

pragma solidity >=0.6.2;

interface IPlunderSwapRouter02 is IPlunderSwapRouter01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}
```
