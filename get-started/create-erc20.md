# Create an ERC20 Token on Zil EVM Testnet

This guide will walk you through creating your own ERC20 token on the Zilliqa EVM Testnet using OpenZeppelin and Remix IDE.

{% embed url="https://youtu.be/sG-HA3sm0nc" %}
Quick video overview of the process
{% endembed %}

## Prerequisites

1. MetaMask wallet installed and connected to Zilliqa EVM Testnet
2. Some testnet ZIL for deployment (get them from the [Zilliqa Testnet Faucet](https://dev-wallet.zilliqa.com/faucet?network=testnet))

## Steps to Create Your Token

### 1. Generate Token Contract

1. Visit [OpenZeppelin Contract Wizard](https://wizard.openzeppelin.com/)
2. Select "ERC20" from the top menu
3. Configure your token:
   - Enter your token name
   - Enter your token symbol
   - Enter your token initial supply
   - Select desired features (e.g., Mintable, Burnable, Pausable)
   - Leave "Access Control" as "Ownable" for simple projects
4. Click "Open in Remix" button on the top right

### 2. Deploy Using Remix

1. In Remix IDE:
   - Ensure your contract is selected in the file explorer
   - Go to the "Solidity Compiler" tab
   - Set compiler version to 0.8.20 or later
   - **Important**: Under advanced configurations, set EVM Version to "London"
   - Click "Compile"

2. Deploy the contract:
   - Go to the "Deploy & Run Transactions" tab
   - Set environment to "Injected Provider - MetaMask"
   - Ensure your MetaMask is connected to Zilliqa EVM Testnet
   - Select your contract from the dropdown
   - Fill in constructor arguments if any (e.g., initial supply)
   - Click "Deploy"
   - Confirm the transaction in MetaMask

### 3. Verify Your Token

After deployment:
1. Save your contract address - it will be in the bottom left of the Remix IDE
2. You can view your token on block explorers - [Otterscan](https://otterscan.testnet.zilliqa.com/)
3. Test by adding the token to MetaMask using the contract address

## Common Issues and Solutions

- If deployment fails, ensure:
  - You have enough testnet ZIL for gas
  - EVM version is set to "London"
  - You're connected to Zilliqa EVM Testnet

## Next Steps

After creating your token, you might want to:
- Add liquidity to PlunderSwap [here](products/plunderswap-exchange/liquidity-guide.md)
- Test token transfers [here](products/plunderswap-exchange/trade.md)
- You can add a custom token to the Token List on Plunderswap by doing the following:
  - Make sure you are connected to the Zilliqa Testnet
  - On the swap page, use the dropdown to select a token
  - Click Manage Tokens
  - Click on the Tokens tab, then paste your token address in the "Custom Token" section.  It should show you the Token Name and Symbol.
  - Click Import.
  - You will get a warning that the token is not verified.  Click I Understand, then Import.
  - You should now see your token in the dropdown.
