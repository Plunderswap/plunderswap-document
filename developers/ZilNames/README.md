# ZilNames: Zilliqa Name Service Documentation

## Introduction to ZilNames

ZilNames is Zilliqa's domain name service, offering human-readable names with the `.zil` extension. Similar to Ethereum's ENS, ZilNames allows users to replace complex wallet addresses with memorable names, making transactions and identification within the Zilliqa ecosystem simpler and less error-prone.

Features include:

- Human-readable names (e.g., `myname.zil`)
- Reverse resolution (wallet address to name)
- Profile avatars
- Cross-application compatibility in the Zilliqa ecosystem

## Contract Details

ZilNames uses L2Resolver contracts deployed at the following addresses:

``` javascript
Zilliqa Mainnet: 0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F
Zilliqa Testnet: 0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e
```

Other contracts for the full set (only need to use these if you want to setup new addresses)  Alternatively you can direct users to https://zilnames.com/

## Testnet contracts

| Contract            | EVM Proxy                                                                                                             |
| ------------------- | --------------------------------------------------------------------------------------------------------------------- |
| BaseRegistrar       | [0x6d6999EB499E7A4aaD0B462045CaE82907A1D30A](https://otterscan.testnet.zilliqa.com/address/0x6d6999EB499E7A4aaD0B462045CaE82907A1D30A) |
| L2Resolver          | [0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e](https://otterscan.testnet.zilliqa.com/address/0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e) |
| RegistrarController | [0xC228acc2f1B177a486abd5be0FA513Dc96F14A6f](https://otterscan.testnet.zilliqa.com/address/0xC228acc2f1B177a486abd5be0FA513Dc96F14A6f) |
| Registry            | [0x716c7e7dC02f7E0FD44343C720233DB57896Fb1b](https://otterscan.testnet.zilliqa.com/address/0x716c7e7dC02f7E0FD44343C720233DB57896Fb1b) |
| ReverseRegistrar    | [0xBA0478582F6B119c8F9EB71b28927467E5e75357](https://otterscan.testnet.zilliqa.com/address/0xBA0478582F6B119c8F9EB71b28927467E5e75357) |

## Mainnet contracts

| Contract            | EVM Proxy                                                                                                             |
| ------------------- | --------------------------------------------------------------------------------------------------------------------- |
| BaseRegistrar       | [0x34f1A58c54B543D6cb82FC63fBc2D39F749099Df](https://otterscan.testnet.zilliqa.com/address/0x34f1A58c54B543D6cb82FC63fBc2D39F749099Df) |
| L2Resolver          | [0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F](https://otterscan.testnet.zilliqa.com/address/0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F) |
| RegistrarController | [0x7c588d2128B8A68DDDEf59fF6E4EdEE5435371EF](https://otterscan.testnet.zilliqa.com/address/0x7c588d2128B8A68DDDEf59fF6E4EdEE5435371EF) |
| Registry            | [0x2196b67Ca97bBcA07C01c7Bdf4f35209CC615389](https://otterscan.testnet.zilliqa.com/address/0x2196b67Ca97bBcA07C01c7Bdf4f35209CC615389) |
| ReverseRegistrar    | [0x228242c33D0D0825a3B31B2f4c2f10b7Cb64B788](https://otterscan.testnet.zilliqa.com/address/0x228242c33D0D0825a3B31B2f4c2f10b7Cb64B788) |
| UniversalResolver   | [0xe32A0F3e3787B535719e8949Eb065F675eB96D25](https://otterscan.testnet.zilliqa.com/address/0xe32A0F3e3787B535719e8949Eb065F675eB96D25) |

## ABIs

Here is links to the ABIs used for Zilnames

<a href="https://static.plunderswap.com/zilnames-abis/BaseRegistrar.abi" download>BaseRegistrar.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/L2Resolver.abi" download>L2Resolver.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/RegistrarController.abi" download>RegistrarController.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/Registry.abi" download>Registry.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/ReverseRegistrar.abi" download>ReverseRegistrar.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/UniversalResolver.abi" download>UniversalResolver.abi</a> 

## Helper Functions

### Namehash Function

While most libraries like viem provide a namehash implementation, here's how the namehash algorithm works:

```javascript
import { keccak256 } from "viem";
import { toUtf8Bytes } from "viem/utils";

// Implementation of ENS namehash algorithm
function namehash(name) {
  let node = '0x0000000000000000000000000000000000000000000000000000000000000000';
  
  if (name) {
    const labels = name.split('.');
    
    for (let i = labels.length - 1; i >= 0; i--) {
      const label = labels[i];
      const labelHash = keccak256(toUtf8Bytes(label));
      node = keccak256(new Uint8Array([...hexToBytes(node), ...hexToBytes(labelHash)]));
    }
  }
  
  return node;
}

// Helper to convert hex string to byte array
function hexToBytes(hex) {
  const bytes = [];
  for (let c = 2; c < hex.length; c += 2) {
    bytes.push(parseInt(hex.substr(c, 2), 16));
  }
  return bytes;
}
```

## ZilNames Basic Integration

### 1. Forward Resolution (Name → Address)

To resolve a `.zil` name to an address, you'll need to:

1. Calculate the namehash of the name
2. Call the `addr` function on the L2Resolver contract

```javascript
import { namehash } from "viem/ens";

// Function to resolve a .zil name to an address
async function resolveNameToAddress(name, client) {
  // Ensure name has .zil suffix
  if (!name.endsWith('.zil') && !name.endsWith('.test.zil')) {
    throw new Error('Invalid name format: Must end with .zil or .test.zil');
  }
  
  // Get appropriate contract address based on name
  const isTestnet = name.endsWith('.test.zil');
  const contractAddress = isTestnet 
    ? "0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e"
    : "0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F";
    
  // Calculate namehash
  const node = namehash(name);
  
  // Call contract
  try {
    const address = await client.readContract({
      address: contractAddress,
      abi: L2ResolverAbi,
      functionName: "addr",
      args: [node],
    });
    
    return address;
  } catch (error) {
    console.error("Error resolving name:", error);
    return null;
  }
}
```

### 2. Reverse Resolution (Address → Name)

Reverse resolution requires converting the address to a special reverse node format:

```javascript
import { encodePacked, keccak256, namehash } from "viem";

// Convert chainId to coin type for reverse resolution
function convertChainIdToCoinType(chainId) {
  // Zilliqa mainnet
  if (chainId === zilliqa.id) {
    return "addr";
  } 
  // Zilliqa testnet
  else if (chainId === zilliqaTestnet.id) {
    return "80002105";
  }
  
  // For other chains, apply BIP44 derivation
  const cointype = (0x80000000 | chainId) >>> 0;
  return cointype.toString(16).toLocaleUpperCase();
}

// Convert address to reverse node
function convertReverseNodeToBytes(address, chainId) {
  const addressFormatted = address.toLowerCase();
  const addressNode = keccak256(addressFormatted.substring(2));
  
  const chainCoinType = convertChainIdToCoinType(chainId);
  const baseReverseNode = namehash(`${chainCoinType.toLocaleUpperCase()}.reverse`);
  
  const addressReverseNode = keccak256(
    encodePacked(["bytes32", "bytes32"], [baseReverseNode, addressNode])
  );
  
  return addressReverseNode;
}

// Resolve address to name
async function resolveAddressToName(address, chainId, client) {
  // Get appropriate contract based on chainId
  const contractAddress = chainId === 33101 
    ? "0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e" 
    : "0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F";
  
  // Calculate reverse node
  const addressReverseNode = convertReverseNodeToBytes(address, chainId);
  
  // Call contract
  try {
    const name = await client.readContract({
      address: contractAddress,
      abi: L2ResolverAbi,
      functionName: "name",
      args: [addressReverseNode],
    });
    
    return name || null;
  } catch (error) {
    console.error("Error resolving address to name:", error);
    return null;
  }
}
```

### 3. Avatar Resolution

ZilNames also supports avatar images stored in the text records:

```javascript
import { namehash } from "viem/ens";

// Function to get avatar for a .zil name
async function getAvatar(name, client) {
  if (!name) return null;
  
  // Get appropriate contract address based on name
  const isTestnet = name.endsWith('.test.zil');
  const contractAddress = isTestnet 
    ? "0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e"
    : "0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F";
  
  try {
    // Get the avatar text record
    const avatar = await client.readContract({
      address: contractAddress,
      abi: L2ResolverAbi,
      functionName: "text",
      args: [namehash(name), "avatar"],
    });
    
    if (!avatar) return null;
    
    // Handle IPFS URLs if needed
    if (avatar.startsWith('ipfs://')) {
      const ipfsGateway = "https://ipfs.io/ipfs/";
      const ipfsPath = avatar.replace('ipfs://', '');
      return `${ipfsGateway}${ipfsPath}`;
    }
    
    return avatar;
  } catch (error) {
    console.error("Error fetching avatar:", error);
    return null;
  }
}
```

## React Integration Example

For React applications, here's how you might implement hooks to use ZilNames:

```jsx
import { useQuery } from '@tanstack/react-query';
import { namehash } from 'viem/ens';
import { Address } from 'viem';

// Hook to resolve wallet address to .zil name
function useZilliqaEnsName({ address, chainId }) {
  const { data, isLoading } = useQuery({
    queryKey: ['zilname', address, chainId],
    queryFn: async () => {
      if (!address || !chainId) return null;
      
      const client = getChainPublicClient(chainId);
      const addressReverseNode = convertReverseNodeToBytes(address, chainId);
      
      try {
        const contractAddress = chainId === 33101 
          ? "0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e" 
          : "0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F";
        
        const zilname = await client.readContract({
          abi: L2ResolverAbi,
          address: contractAddress,
          functionName: "name",
          args: [addressReverseNode],
        });
        
        return zilname || null;
      } catch (error) {
        console.error('Error fetching Zilname:', error);
        return null;
      }
    },
    enabled: !!address && !!chainId,
  });
  
  return {
    name: data,
    isLoading,
  };
}

// Component using the hook
function UserDisplay({ address, chainId }) {
  const { name, isLoading } = useZilliqaEnsName({ address, chainId });
  
  if (isLoading) return <div>Loading...</div>;
  
  return (
    <div>
      {name ? name : `${address.slice(0, 6)}...${address.slice(-4)}`}
    </div>
  );
}
```

## Full L2Resolver Contract Interface

The L2Resolver contract implements the following key functions:

- `name(bytes32 node)` - Gets the name for a reverse record
- `addr(bytes32 node)` - Gets the address for a forward record
- `text(bytes32 node, string key)` - Gets a text record value (e.g., avatar)
- `setPubkey(bytes32 node, bytes32 x, bytes32 y)` - Sets public key
- `setAddr(bytes32 node, address addr)` - Sets forward record
- `setText(bytes32 node, string key, string value)` - Sets text record

Most applications will primarily use the resolution functions (`name` and `addr`), but the contract supports a full suite of ENS-compatible functionality.

## Working with Different Chains

ZilNames operates on both Zilliqa mainnet (for `.zil` domains) and Zilliqa testnet (for `.test.zil` domains). To determine the appropriate chain:

```javascript
function getChainForZilname(username) {
  return username.endsWith('.test.zil')
    ? zilliqaTestnet
    : zilliqa;
}
```

## Conclusion

ZilNames provides a user-friendly naming system for the Zilliqa ecosystem. By integrating ZilNames into your application, you can offer users a more intuitive experience when interacting with blockchain addresses.

For more information, visit [zilnames.com](https://zilnames.com/).
For more information on ENS Contracts, visit [docs.ens.domains/contracts](https://docs.ens.domains/contracts).
