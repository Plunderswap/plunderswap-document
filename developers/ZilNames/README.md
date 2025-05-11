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

## Advanced React Integration Example

The following example demonstrates a more complete implementation based on the [sample-next-js](https://github.com/Plunderswap/sample-next-js) repository, which provides a live demo at [sample-next-js-gray.vercel.app](https://sample-next-js-gray.vercel.app/).

### 1. Setting up the hooks

First, create three custom hooks to handle different aspects of ZilNames resolution:

**useZilliqaEnsName.ts** - for resolving addresses to names (reverse resolution):

```typescript
import { Address, isAddress } from "viem";
import { useQuery } from "@tanstack/react-query";
import { convertReverseNodeToBytes } from "../utils/convertReverseNodeToBytes";
import { getChainPublicClient } from "../utils/getChainPublicClient";
import L2ResolverAbi from "../abis/L2ResolverAbi";
import { zilliqa, zilliqaTestnet } from "../../config/chains";

export type UseZilliqaEnsNameProps = {
  address?: Address;
  chainId?: number;
};

export type ZilliqaEnsNameData = {
  name: string | null;
  isLoading: boolean;
  isFetching: boolean;
};

export default function useZilliqaEnsName({ address, chainId }: UseZilliqaEnsNameProps): ZilliqaEnsNameData {
  const { data, isLoading, isFetching } = useQuery({
    queryKey: ['zilname', address, chainId],
    queryFn: async () => {
      if (!address || !chainId) return null;
      
      // Get the appropriate chain configuration
      const chain = chainId === zilliqa.id ? zilliqa : 
                   chainId === zilliqaTestnet.id ? zilliqaTestnet : 
                   null;
      
      if (!chain) {
        console.log('Chain not supported:', chainId);
        return null;
      }
      
      const client = getChainPublicClient(chain);
      
      const addressReverseNode = convertReverseNodeToBytes(address, chainId);

      try {
        const contractAddress = chainId === zilliqa.id 
          ? "0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F" 
          : "0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e";

        const zilname = await client.readContract({
          abi: L2ResolverAbi,
          address: contractAddress,
          functionName: "name",
          args: [addressReverseNode],
        }) as string;

        if (zilname && zilname.length > 0) {
          return zilname;
        }

        return null;
      } catch (error) {
        console.error('Error fetching Zilname:', error);
        return null;
      }
    },
    enabled: !!address && !!chainId && isAddress(address),
  });

  return {
    name: data ?? null,
    isLoading,
    isFetching,
  };
}
```

**useZilliqaEnsAddress.ts** - for resolving names to addresses (forward resolution):

```typescript
import { Address } from "viem";
import { useQuery } from "@tanstack/react-query";
import { getChainPublicClient } from "../utils/getChainPublicClient";
import L2ResolverAbi from "../abis/L2ResolverAbi";
import { zilliqa, zilliqaTestnet } from "../../config/chains";
import { namehash } from "viem/ens";

export type UseZilliqaEnsAddressProps = {
  name?: string;
  chainId?: number;
};

export type ZilliqaEnsAddressData = {
  address: string | null;
  isLoading: boolean;
  isFetching: boolean;
  error: Error | null;
};

export default function useZilliqaEnsAddress({ name, chainId }: UseZilliqaEnsAddressProps): ZilliqaEnsAddressData {
  const { data, isLoading, isFetching, error } = useQuery({
    queryKey: ['ziladdress', name, chainId],
    queryFn: async () => {
      if (!name || !chainId) return null;
      
      // Get the appropriate chain configuration
      const chain = chainId === zilliqa.id ? zilliqa : 
                   chainId === zilliqaTestnet.id ? zilliqaTestnet : 
                   null;
      
      if (!chain) {
        console.log('Chain not supported:', chainId);
        return null;
      }
      
      const client = getChainPublicClient(chain);
      
      try {
        const contractAddress = chainId === zilliqa.id 
          ? "0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F" 
          : "0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e";

        // Use namehash to convert the name to the node format expected by the contract
        const nameNode = namehash(name);

        // Call the addr function of the L2 resolver contract
        const address = await client.readContract({
          abi: L2ResolverAbi,
          address: contractAddress,
          functionName: "addr",
          args: [nameNode],
        }) as Address;
        
        if (address) {
          return address;
        }

        return null;
      } catch (error) {
        console.error('Error fetching address:', error);
        throw error; // Let the error be caught by the error handler
      }
    },
    enabled: !!name && !!chainId && name.trim().length > 0,
  });

  return {
    address: data ?? null,
    isLoading,
    isFetching,
    error: error as Error | null,
  };
}
```

**useZilEnsAvatar.ts** - for fetching avatar images:

```typescript
import { useQuery } from "@tanstack/react-query";
import { getChainPublicClient } from "../utils/getChainPublicClient";
import { zilliqa, zilliqaTestnet } from "../../config/chains";
import L2ResolverAbi from "../abis/L2ResolverAbi";
import { Address } from "viem";
import useZilliqaEnsName from "./useZilliqaEnsName";
import { namehash } from "viem/ens";

export type UseZilEnsAvatarProps = {
  address?: Address;
  chainId?: number;
};

export type ZilEnsAvatarData = {
  avatar: string | null;
  isLoading: boolean;
  isFetching: boolean;
};

export default function useZilEnsAvatar({ address, chainId }: UseZilEnsAvatarProps): ZilEnsAvatarData {
  // First get the name associated with this address
  const { name, isLoading: isNameLoading } = useZilliqaEnsName({ address, chainId });

  const { data, isLoading, isFetching } = useQuery({
    queryKey: ['zilavatar', name, chainId],
    queryFn: async () => {
      if (!name || !chainId) return null;
      
      // Get the appropriate chain configuration
      const chain = chainId === zilliqa.id ? zilliqa : 
                   chainId === zilliqaTestnet.id ? zilliqaTestnet : 
                   null;
      
      if (!chain) {
        console.log('Chain not supported:', chainId);
        return null;
      }
      
      const client = getChainPublicClient(chain);
      
      try {
        const contractAddress = chainId === zilliqa.id 
          ? "0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F" 
          : "0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e";
        
        // Get the avatar text record using the name's namehash
        const avatar = await client.readContract({
          abi: L2ResolverAbi,
          address: contractAddress,
          functionName: "text",
          args: [namehash(name), "avatar"],
        }) as string;

        if (!avatar) {
          return null;
        }

        // Handle IPFS URLs
        if (avatar.startsWith('ipfs://')) {
          const ipfsGateway = "https://ipfs.io/ipfs/";
          const ipfsPath = avatar.replace('ipfs://', '');
          return `${ipfsGateway}${ipfsPath}`;
        }

        // Return the avatar URL directly if it's not IPFS
        return avatar;
      } catch (error) {
        console.error('Error fetching avatar:', error);
        return null;
      }
    },
    enabled: !!name && !isNameLoading && !!chainId,
  });

  return {
    avatar: data ?? null,
    isLoading: isLoading || isNameLoading,
    isFetching,
  };
}
```

### 2. Utility functions

You'll need some utility functions referenced in the hooks:

**convertReverseNodeToBytes.ts**:

```typescript
import { Address, encodePacked, keccak256, namehash } from "viem";
import { zilliqa, zilliqaTestnet } from "../../config/chains";

// Convert chainId to coin type for reverse resolution
export function convertChainIdToCoinType(chainId: number): string {
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
export function convertReverseNodeToBytes(address: Address, chainId: number): `0x${string}` {
  const addressFormatted = address.toLowerCase();
  const addressNode = keccak256(addressFormatted.substring(2));
  
  const chainCoinType = convertChainIdToCoinType(chainId);
  const baseReverseNode = namehash(`${chainCoinType.toLocaleUpperCase()}.reverse`);
  
  const addressReverseNode = keccak256(
    encodePacked(["bytes32", "bytes32"], [baseReverseNode, addressNode])
  );
  
  return addressReverseNode;
}
```

**getChainPublicClient.ts**:

```typescript
import { Chain, createPublicClient, http } from "viem";

export function getChainPublicClient(chain: Chain) {
  return createPublicClient({
    chain,
    transport: http(),
  });
}
```

### 3. Example implementation in a React component

Here's how you can use these hooks in a React component:

```tsx
'use client';

import { useState } from 'react';
import { isAddress } from 'viem';
import { zilliqa } from '@/config/chains';
import useZilliqaEnsName from '@/lib/hooks/useZilliqaEnsName';
import useZilEnsAvatar from '@/lib/hooks/useZilEnsAvatar';
import useZilliqaEnsAddress from '@/lib/hooks/useZilliqaEnsAddress';

export default function ZilNamesResolver() {
  // Name to Address state
  const [nameInput, setNameInput] = useState('');
  const [shouldResolveAddress, setShouldResolveAddress] = useState(false);
  
  // Address to Name state
  const [addressInput, setAddressInput] = useState('');
  const [shouldResolveName, setShouldResolveName] = useState(false);

  // ENS name to address resolution
  const { 
    address: resolvedAddress, 
    isLoading: isLoadingAddress, 
    error: addressError 
  } = useZilliqaEnsAddress({
    name: shouldResolveAddress ? nameInput.trim() : undefined,
    chainId: zilliqa.id,
  });

  // Address to ENS name resolution
  const { 
    name: resolvedName, 
    isLoading: isLoadingName 
  } = useZilliqaEnsName({
    address: shouldResolveName && isAddress(addressInput.trim()) 
      ? addressInput.trim() as `0x${string}` 
      : undefined,
    chainId: zilliqa.id,
  });

  // Get avatar for the resolved name
  const {
    avatar: nameAvatar,
    isLoading: isLoadingAvatar
  } = useZilEnsAvatar({
    address: resolvedAddress as `0x${string}`,
    chainId: zilliqa.id
  });

  // Handle name to address resolution
  const handleResolveAddress = () => {
    setShouldResolveAddress(true);
  };

  // Handle address to name resolution
  const handleResolveName = () => {
    setShouldResolveName(true);
  };

  return (
    <div className="container mx-auto p-4">
      <h1 className="text-2xl font-bold mb-6">ZilNames Resolver</h1>
      
      <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
        {/* Name to Address Section */}
        <div className="border p-4 rounded">
          <h2 className="text-xl mb-4">Resolve Name to Address</h2>
          <input
            type="text"
            value={nameInput}
            onChange={(e) => setNameInput(e.target.value)}
            placeholder="Enter a .zil name"
            className="border p-2 w-full mb-2"
          />
          <button 
            onClick={handleResolveAddress}
            className="bg-blue-500 text-white px-4 py-2 rounded"
            disabled={isLoadingAddress}
          >
            {isLoadingAddress ? 'Loading...' : 'Resolve'}
          </button>
          
          {resolvedAddress && (
            <div className="mt-4">
              <p><strong>Address:</strong> {resolvedAddress}</p>
              {nameAvatar && <img src={nameAvatar} alt="Avatar" className="w-12 h-12 rounded-full mt-2" />}
            </div>
          )}
          
          {addressError && <p className="text-red-500 mt-2">{addressError.message}</p>}
        </div>
        
        {/* Address to Name Section */}
        <div className="border p-4 rounded">
          <h2 className="text-xl mb-4">Resolve Address to Name</h2>
          <input
            type="text"
            value={addressInput}
            onChange={(e) => setAddressInput(e.target.value)}
            placeholder="Enter an EVM 0x address"
            className="border p-2 w-full mb-2"
          />
          <button 
            onClick={handleResolveName}
            className="bg-blue-500 text-white px-4 py-2 rounded"
            disabled={isLoadingName}
          >
            {isLoadingName ? 'Loading...' : 'Resolve'}
          </button>
          
          {resolvedName && (
            <div className="mt-4">
              <p><strong>Name:</strong> {resolvedName}</p>
            </div>
          )}
        </div>
      </div>
    </div>
  );
}
```

This example demonstrates a complete workflow for resolving ZilNames, including both forward and reverse resolution, and avatar fetching. For a working example, visit [sample-next-js-gray.vercel.app](https://sample-next-js-gray.vercel.app/) or fork the [sample-next-js](https://github.com/Plunderswap/sample-next-js) repository to see this in action.

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
