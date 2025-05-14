# ZilNames: Zilliqa Name Service Documentation

![](<../../.gitbook/assets/Zilliqa_name_service_banner.png>)

## Introduction to ZilNames

ZilNames is Zilliqa's domain name service, offering human-readable names with the `.zil` extension. Similar to Ethereum's ENS, ZilNames allows users to replace complex wallet addresses with memorable names, making transactions and identification within the Zilliqa ecosystem simpler and less error-prone.

Features include:

- Human-readable names (e.g., `myname.zil`)
- Reverse resolution (wallet address to name)
- Profile avatars
- Cross-application compatibility in the Zilliqa ecosystem

## Contract Details

The main contract used for Zilnames resolution is the UniversalResolver.  This is the most simplistic and also works out of the box with Viem.

The other advanced option is using the L2Resolver contract but this is more advanced and manual.

Contracts for the full set (only need to use these if you want to setup new addresses)  Alternatively you can direct users to https://zilnames.com/

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
| UniversalResolver   | [0x2b6a7953c510392aE88c7C302a984460Daa8AF24](https://otterscan.testnet.zilliqa.com/address/0x2b6a7953c510392aE88c7C302a984460Daa8AF24) |

## ABIs

Here is links to the ABIs used for Zilnames

<a href="https://static.plunderswap.com/zilnames-abis/BaseRegistrar.abi" download>BaseRegistrar.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/L2Resolver.abi" download>L2Resolver.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/RegistrarController.abi" download>RegistrarController.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/Registry.abi" download>Registry.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/ReverseRegistrar.abi" download>ReverseRegistrar.abi</a> \
<a href="https://static.plunderswap.com/zilnames-abis/UniversalResolver.abi" download>UniversalResolver.abi</a> 

## OPTION 1 - ZilNames Integration using Viem

Viem.sh now supports ZilNames out-of-the-box thanks to the UniversalResolver contract. This is the simplest way to integrate ZilNames into your dApp.

First, define the Zilliqa chain configuration in your Viem setup:

```typescript
import { defineChain } from 'viem'

export const zilliqa = defineChain({
  id: 32769,
  name: "Zilliqa",
  iconUrl: "https://plunderswap.com/images/chains/33101.png",
  nativeCurrency: { name: "Zilliqa", symbol: "ZIL", decimals: 18 },
  rpcUrls: {
    default: { http: ["https://api.zilliqa.com/"] },
  },
  blockExplorers: {
    default: { name: "Otterscan", url: "https://otterscan.zilliqa.com" },
  },
  contracts: {
    ensUniversalResolver: {
      address: "0x2b6a7953c510392aE88c7C302a984460Daa8AF24",
    },
    ensRegistry: {
      address: "0x2196b67Ca97bBcA07C01c7Bdf4f35209CC615389",
    },
    multicall3: {
      address: "0x38899efb93d5106d3adb86662c557f237f6ecf57",
      blockCreated: 5313022,
    },
  },
});
```

Once your client is configured with the Zilliqa chain, you can use Viem's ENS actions. Remember to normalize ENS names using `normalize` from `viem/ens` before passing them to these functions.

### 1. Forward Resolution (Name → Address)

Use `getEnsAddress` to resolve a `.zil` name to an address.

```typescript
import { normalize } from 'viem/ens'
import { createPublicClient, http } from 'viem'
import { zilliqa } from './chains' // Assuming your chain definition is in chains.ts

const publicClient = createPublicClient({
  chain: zilliqa,
  transport: http(),
})

async function resolveZilNameToAddress(name: string) {
  const ensAddress = await publicClient.getEnsAddress({
    name: normalize(name), // e.g., normalize('example.zil')
  });
  return ensAddress;
}
```

For more details, see the [Viem getEnsAddress documentation](https://viem.sh/docs/ens/actions/getEnsAddress).

### 2. Reverse Resolution (Address → Name)

Use `getEnsName` to resolve an address to its primary `.zil` name.

```typescript
import { createPublicClient, http } from 'viem'
import { zilliqa } from './chains'

const publicClient = createPublicClient({
  chain: zilliqa,
  transport: http(),
})

async function resolveAddressToZilName(address: `0x${string}`) {
  const ensName = await publicClient.getEnsName({
    address: address,
  });
  return ensName;
}
```

For more details, see the [Viem getEnsName documentation](https://viem.sh/docs/ens/actions/getEnsName).

### 3. Avatar Resolution

Use `getEnsAvatar` to get the avatar URL for a `.zil` name.

```typescript
import { normalize } from 'viem/ens'
import { createPublicClient, http } from 'viem'
import { zilliqa } from './chains'

const publicClient = createPublicClient({
  chain: zilliqa,
  transport: http(),
})

async function getZilNameAvatar(name: string) {
  const avatarUrl = await publicClient.getEnsAvatar({
    name: normalize(name),
  });
  return avatarUrl;
}
```

For more details, see the [Viem getEnsAvatar documentation](https://viem.sh/docs/ens/actions/getEnsAvatar).

### 4. Resolver Address

Use `getEnsResolver` to get the resolver address for a `.zil` name.

```typescript
import { normalize } from 'viem/ens'
import { createPublicClient, http } from 'viem'
import { zilliqa } from './chains'

const publicClient = createPublicClient({
  chain: zilliqa,
  transport: http(),
})

async function getZilNameResolver(name: string) {
  const resolverAddress = await publicClient.getEnsResolver({
    name: normalize(name),
  });
  return resolverAddress;
}
```

For more details, see the [Viem getEnsResolver documentation](https://viem.sh/docs/ens/actions/getEnsResolver).

### 5. Text Records

Use `getEnsText` to retrieve arbitrary text records associated with a `.zil` name.

```typescript
import { normalize } from 'viem/ens'
import { createPublicClient, http } from 'viem'
import { zilliqa } from './chains'

const publicClient = createPublicClient({
  chain: zilliqa,
  transport: http(),
})

async function getZilNameTextRecord(name: string, key: string) {
  const textRecord = await publicClient.getEnsText({
    name: normalize(name),
    key: key,
  });
  return textRecord;
}
```

For more details, see the [Viem getEnsText documentation](https://viem.sh/docs/ens/actions/getEnsText).

## OPTION 2 - ZilNames Integration using UniversalResolver Contract

This option demonstrates how to directly interact with the `UniversalResolver` contract for more fine-grained control or when not using Viem's built-in ENS functions. You will need the ABI for the `UniversalResolver` contract. (Linked above)

Refer to the [ENS Universal Resolver Documentation](https://docs.ens.domains/resolvers/universal) for detailed information on the contract's functions and how to encode data.

The `UniversalResolver` contract address on Zilliqa Mainnet is `0x2b6a7953c510392aE88c7C302a984460Daa8AF24`. You can find the ABI linked in the [ABIs section](#abis).

Here's a conceptual Next.js example using `wagmi` and `viem` for contract interaction:

### 1. Setup

Ensure you have `wagmi` and `viem` installed. You'll also need the `UniversalResolver.abi`.

```typescript
// lib/universalResolver.ts
import { Address, encodeFunctionData, parseAbi, toHex } from 'viem';
import { namehash, normalize, packetToBytes } from 'viem/ens'; // For name normalization, hashing, and DNS encoding
// Assuming UniversalResolver.abi is available in your project
// You'll need to load the ABI content, e.g., by importing it as a JSON module or reading the file content.
// const UniversalResolverABI = await import('../abis/UniversalResolver.abi.json'); // Example if using JSON module
// For this example, we'll assume the ABI string is loaded into a variable.
// The actual ABI content should be sourced from the 'UniversalResolver.abi' file linked in the 'ABIs' section of this document.
const UNIVERSAL_RESOLVER_ABI_JSON_STRING = '[...]'; // Placeholder: Replace with actual JSON string content of UniversalResolver.abi

const universalResolverAddress = "0x2b6a7953c510392aE88c7C302a984460Daa8AF24" as Address;
const universalResolverAbi = parseAbi(UNIVERSAL_RESOLVER_ABI_JSON_STRING);

// Viem client (example setup)
// import { createPublicClient, http } from 'viem';
// import { zilliqa } from './chains'; // Your chain definition
// const client = createPublicClient({ chain: zilliqa, transport: http() });

// Helper function to interact with the UniversalResolver
async function callUniversalResolver(client: any, functionName: string, args: any[]) {
  try {
    const data = await client.readContract({
      address: universalResolverAddress,
      abi: universalResolverAbi,
      functionName,
      args,
    });
    return data;
  } catch (error) {
    console.error(`Error calling ${functionName}:`, error);
    throw error; // Re-throw to allow caller to handle
  }
}

export async function resolveNameWithUniversalResolver(client: any, name: string): Promise<[`0x${string}`, Address] | null> {
  const normalizedName = normalize(name); // UTS-46 normalization
  const dnsEncodedName = toHex(packetToBytes(normalizedName)); // DNS encode the name

  // Construct the call data for the target resolver's `addr(bytes32)` function.
  // This is what we want the UniversalResolver to call on the actual resolver for the name.
  const node = namehash(normalizedName);
  const addrFunctionSignature = 'addr(bytes32)'; // Standard ENS resolver function
  // For simplicity, assuming a basic ABI for the target resolver's addr function.
  // In a real scenario, you might need a more complete resolver ABI if dealing with various resolver types.
  const targetResolverAddrAbi = parseAbi([`function ${addrFunctionSignature} view returns (address)`]);
  
  const callDataToForward = encodeFunctionData({
    abi: targetResolverAddrAbi,
    functionName: 'addr',
    args: [node]
  });

  try {
    // Call the `resolve` function on the UniversalResolver
    const result = await callUniversalResolver(client, 'resolve', [dnsEncodedName, callDataToForward]) as [`0x${string}`, Address];
    // result[0] is the ABI-encoded address from the target resolver
    // result[1] is the address of the target resolver
    return result;
  } catch (e) {
    console.error(`Error in resolveNameWithUniversalResolver for ${name}:`, e);
    return null;
  }
}


export async function reverseResolveAddressWithUniversalResolver(client: any, address: Address): Promise<[`0x${string}`, Address] | null> {
  // Construct the reverse node string, e.g., "d2135cfb216b74109775236e36d4b433f1df507b.addr.reverse"
  const reverseNodeString = `${address.slice(2).toLowerCase()}.addr.reverse`;
  const dnsEncodedReverseNode = toHex(packetToBytes(reverseNodeString)); // DNS encode the reverse node string

  // Construct the call data for the target resolver's `name(bytes32)` function.
  const node = namehash(reverseNodeString);
  const nameFunctionSignature = 'name(bytes32)'; // Standard ENS resolver function
  const targetResolverNameAbi = parseAbi([`function ${nameFunctionSignature} view returns (string)`]);

  const callDataToForward = encodeFunctionData({
    abi: targetResolverNameAbi,
    functionName: 'name',
    args: [node]
  });
  
  try {
    // Call the `resolve` function on the UniversalResolver
    const result = await callUniversalResolver(client, 'resolve', [dnsEncodedReverseNode, callDataToForward]) as [`0x${string}`, Address];
    // result[0] is the ABI-encoded name string from the target resolver
    // result[1] is the address of the target resolver
    return result;
  } catch (e) {
    console.error(`Error in reverseResolveAddressWithUniversalResolver for ${address}:`, e);
    return null;
  }
}

// Note on UNIVERSAL_RESOLVER_ABI_JSON_STRING:
// This placeholder should be replaced with the actual JSON string content of the UniversalResolver.abi file.
// You can typically achieve this by importing the .abi file as a JSON module if your bundler supports it,
// or by reading the file content and parsing it.
// Example (conceptual, depends on your environment):
// import abiFileContent from '../abis/UniversalResolver.abi.json';
// const UNIVERSAL_RESOLVER_ABI_JSON_STRING = JSON.stringify(abiFileContent);
```

*Important Note on `callDataToForward`*: The `data` parameter (which we've named `callDataToForward` for clarity) for the Universal Resolver's `resolve` function is crucial. It is the ABI-encoded function call that you intend to execute on the *actual name's resolver contract*, not on the Universal Resolver itself. The Universal Resolver acts as a gateway, forwarding this call to the appropriate resolver for the given name.

*   For forward resolution (name to address), this is typically an encoded call to `addr(bytes32 node)` or `addr(bytes32 node, uint256 coinType)`.
*   For reverse resolution (address to name), this is typically an encoded call to `name(bytes32 node)`.
*   The Universal Resolver's `resolve` function returns `(bytes result, address resolverAddress)`. You will then need to decode `result` using the ABI of the function you originally encoded into `callDataToForward` (e.g., decode `result` as an `address` if you called `addr`, or as a `string` if you called `name`).

### 2. React Component Example

```tsx
// components/ZilNamesUniversalResolver.tsx
import { useState } from 'react';
import { usePublicClient } from 'wagmi';
import { zilliqa } from '@/lib/chains'; // Your chain definition
import { resolveNameWithUniversalResolver, reverseResolveAddressWithUniversalResolver } from '@/lib/universalResolver'; // Adjust path
import { Address, isAddress, decodeAbiParameters, parseAbiParameters } from 'viem';

export default function ZilNamesUniversalResolver() {
  const publicClient = usePublicClient({ chainId: zilliqa.id }); // Ensure this client is configured for Zilliqa
  const [nameInput, setNameInput] = useState('');
  const [addressInput, setAddressInput] = useState('');
  
  const [resolvedForwardAddress, setResolvedForwardAddress] = useState<Address | null>(null);
  const [forwardResolverAddress, setForwardResolverAddress] = useState<Address | null>(null);
  const [forwardError, setForwardError] = useState<string | null>(null);

  const [resolvedReverseName, setResolvedReverseName] = useState<string | null>(null);
  const [reverseResolverAddress, setReverseResolverAddress] = useState<Address | null>(null);
  const [reverseError, setReverseError] = useState<string | null>(null);
  
  const [isLoading, setIsLoading] = useState(false);

  const handleResolveName = async () => {
    if (!publicClient || !nameInput) {
      setForwardError("Please connect wallet and enter a name.");
      return;
    }
    setIsLoading(true);
    setResolvedForwardAddress(null);
    setForwardResolverAddress(null);
    setForwardError(null);
    try {
      const result = await resolveNameWithUniversalResolver(publicClient, nameInput);
      if (result) {
        const [encodedAddressBytes, resolverAddr] = result;
        // Decode the address from the bytes returned by the target resolver
        const decodedAddress = decodeAbiParameters(
          parseAbiParameters('address'),
          encodedAddressBytes
        )[0];
        setResolvedForwardAddress(decodedAddress);
        setForwardResolverAddress(resolverAddr);
      } else {
        setForwardError("Could not resolve name or an error occurred.");
      }
    } catch (e: any) {
      console.error("handleResolveName error:", e);
      setForwardError(e.message || "Failed to resolve name.");
    } finally {
      setIsLoading(false);
    }
  };
  
  const handleReverseResolveAddress = async () => {
    if (!publicClient || !isAddress(addressInput)) {
      setReverseError("Please connect wallet and enter a valid address.");
      return;
    }
    setIsLoading(true);
    setResolvedReverseName(null);
    setReverseResolverAddress(null);
    setReverseError(null);
    try {
      const result = await reverseResolveAddressWithUniversalResolver(publicClient, addressInput as Address);
      if (result) {
        const [encodedNameBytes, resolverAddr] = result;
        // Decode the name string from the bytes returned by the target resolver
        const decodedName = decodeAbiParameters(
          parseAbiParameters('string'),
          encodedNameBytes
        )[0];
        setResolvedReverseName(decodedName);
        setReverseResolverAddress(resolverAddr);
      } else {
        setReverseError("Could not resolve address or an error occurred.");
      }
    } catch (e: any) {
      console.error("handleReverseResolveAddress error:", e);
      setReverseError(e.message || "Failed to resolve address to name.");
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div>
      <h2>Resolve via UniversalResolver (Direct Call - Advanced)</h2>
      <p className="text-sm text-gray-600 mb-2">
        This section demonstrates direct calls to the UniversalResolver. 
        Ensure your client is connected to Zilliqa network.
      </p>
      
      {/* Name to Address */}
      <div className="my-4 p-4 border rounded">
        <h3 className="font-semibold">Name to Address (Forward Resolution)</h3>
        <input
          type="text"
          value={nameInput}
          onChange={(e) => setNameInput(e.target.value)}
          placeholder="Enter .zil name (e.g., example.zil)"
          className="border p-1 mr-2 my-1"
        />
        <button onClick={handleResolveName} disabled={isLoading} className="bg-blue-500 text-white p-1 rounded disabled:opacity-50">
          {isLoading ? 'Resolving...' : 'Resolve Name'}
        </button>
        {resolvedForwardAddress && (
          <div className="mt-2 text-green-700">
            <p>Resolved Address: {resolvedForwardAddress}</p>
            <p className="text-xs">Resolver Used: {forwardResolverAddress}</p>
          </div>
        )}
        {forwardError && <p className="mt-2 text-red-500">{forwardError}</p>}
      </div>
      
      {/* Address to Name */}
      <div className="my-4 p-4 border rounded">
        <h3 className="font-semibold">Address to Name (Reverse Resolution)</h3>
        <input
          type="text"
          value={addressInput}
          onChange={(e) => setAddressInput(e.target.value)}
          placeholder="Enter 0x address"
          className="border p-1 mr-2 my-1"
        />
        <button onClick={handleReverseResolveAddress} disabled={isLoading} className="bg-blue-500 text-white p-1 rounded disabled:opacity-50">
          {isLoading ? 'Resolving...' : 'Reverse Resolve Address'}
        </button>
        {resolvedReverseName && (
          <div className="mt-2 text-green-700">
            <p>Resolved Name: {resolvedReverseName}</p>
            <p className="text-xs">Resolver Used: {reverseResolverAddress}</p>
          </div>
        )}
        {reverseError && <p className="mt-2 text-red-500">{reverseError}</p>}
      </div>
    </div>
  );
}
```

**Key considerations for direct UniversalResolver interaction:**

*   **DNS Encoding:** Names need to be DNS-encoded into bytes. This is a non-trivial step. Viem handles this internally for its ENS functions, but for direct calls, you'll need to implement or find a library for this.
*   **Call Data Construction:** The `data` argument for the `resolve` function must be the ABI-encoded call to the specific function on the underlying resolver (e.g., `addr(bytes32)` for resolving an address, or `text(bytes32, string)` for a text record).
*   **ABI:** You need the ABI for `UniversalResolver.sol`. The `resolve` function returns `(bytes, address)` where the first component is the result of the call to the underlying resolver, and the second is the address of that resolver. You'll need to decode the `bytes` result using the ABI of the function you called on the underlying resolver.

For most use cases, **OPTION 1 (using Viem's built-in ENS functions)** is highly recommended due to its simplicity and abstraction over these complexities. Option 2 provides an avenue for advanced scenarios or environments where Viem's abstractions are not directly used.

## OPTION 3 - ZilNames Manual Integration using L2Resolver

You can also use the L2Resolver using the following functions/examples.

### 1. Forward Resolution (Name → Address) using L2Resolver

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

For React applications, here's how you might implement hooks to use ZilNames manually:

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
        const contractAddress = chainId === zilliqa.id 
          ? "0x5c0c7BFd25efCAE366fE62219fD5558305Ffc46F" 
          : "0x579C72c5377a5a4A8Ce6d43A1701F389c8FDFC8e";
        
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

This example demonstrates a complete workflow for resolving ZilNames, including both forward and reverse resolution, and avatar fetching.

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
