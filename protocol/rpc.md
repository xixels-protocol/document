# 4. RPC

The Leader Node of Xixels provides RPC services. This set of RPC services mainly includes the following functions:
- Account maintenance
- Degas maintenance
- Gateway for Degas RPC

## 4.1 JSON-RPC

The RPC services comply with the [JSON-RPC 2.0 Specification](https://www.jsonrpc.org/specification).


## 4.2 Hex Value Encoding

In Node RPC, binary data is encoded in Hex with the string "0x" as a prefix. The valid characters for Hex encoding, besides the "0x" prefix, are [0-9,a-f,A-F], which are case-insensitive. The encoded result is used as a string in the RPC request or response. The Hex data type referred to in the following text is encoded in this way.

## 4.3 RPC Request Signature

Some RPCs MAY require the caller to sign the request. In this scenario, the RPC MUST contains three parameters:
- `account`  16-byte binary. The account ID of the user that signs the request.
- `signer` JSON object. The signer who signs the request. The structure of this object varies based on the specified signer type.
  - `ethereumSignerAddress` When using Ethereum signer, it's the signer's address.
- `signature` binary. The signature of the request signed by the account.

When the RPCs detailed in subsequent sections require signing the request, it implies that the caller must provide the `account`, `signer`, and `signature` parameters.

### 4.3.1 Signing the Request
The procedure for signing is as follows:

1. Set the value of the `signature` field of the request JSON object to null.

2. Serialize the JSON object into a string format, following [JSON Canonicalization Scheme(JSC)](https://www.rfc-editor.org/rfc/rfc8785).

3. Hash the serialized JSON using the SHA-3 algorithm.

4. Sign the hash using the signer's private key. The signing algorithm deponds on the type of the signer.

**Ethereum Signer**
If the signer is a Ethereum Signer, the caller first gernerate a JSON object with the following two fields:
- Nonce: The nonce of the signer in the metadata.
- Hash: The hash which is got in step 3.

Then, the caller signs the JSON object using [EIP712](https://eips.ethereum.org/EIPS/eip-712) and follows a predefined parameter template.

```Javascript
// Parameter Template for signing the JSON object
{
    // The fixed domain information
    domain: {
      chainId: 1,
      name: 'Xixels RPC Signature',
      version: '1',
    },

    primaryType: 'Login',
    types: {
      EIP712Domain: [
        { name: 'chainId', type: 'uint256' },
        { name: 'name', type: 'string' },
        { name: 'version', type: 'string' },
      ],
      // Refer to PrimaryType
      Login: [
        { name: 'Nonce', type: 'uint256' }        
        { name: 'Hash', type: 'bytes' },
      ],
    },

    message: ${JSON_OBJECT_TO_SIGN}
}

```

### 4.3.2 Verifing the Signature

The procedure of verification the signature.

1. Extract the signature from the parameters and set the parameter value to null.

2. Serialize the RPC request JSON object into a string format, follwing JSC.

3. Hash the serialized JSON using the SHA-3 algorithm.

4. Verify the signature of the hash using the signer's public key. The verifing algorithm deponds on the type of the signer.

5. If the verificaiton is successful, the nonce of the signer is increased by 1.

## 4.4 Account Maintenance

There are a set of RPCs for account maintenance. 

### 4.4.1 GetAccount

**Paramters**

- `account` 16-byte binary. It is the account ID of the Degas to be called.

**Returns**

This RPC returns the metadata of the account defined in [2.1 Externally-Owned Account](./account.md#21-externally-owned-account).

**Note**

It is a read-only RPC. 


### 4.4.2 CreateAccount

**Parameters**

- `signer` JSON object. The signer who signs the request. The struct of this JSON object depends on the type of the signer.
  - `ethereumSignerAddress` 24-byte binary. If using Ethereum signer, pass the signer's address.
- `signers` Array of JSON object. The initial signers of this account. 

**Returns**

- `account` 16-byte binary. The ID of the newly created account.

**Note**

This RPC create a new external-owned account. The caller must provide at least a signer to create this account.

### 4.4.3 AddSigner

**Parameters**

- `account` 16-byte binary. The ID of the account to which the signer is added.
- `newSigner` JSON object. The signer to add. The struct of this JSON object depends on the type of the signer. Check [here](#43-rpc-request-signature) for the details of the struct.
- `signer` JSON object. The signer who signs the request. 
- `signature` binary. The signature of the request signed by the account.

**Returns**

None

**Note**

This RPC appends a new signer to the specified account. The request must be signed by one of the existing signers. 

If the signer already exists but is disabled, executing this RPC will enable the signer again.

### 4.4.4 DisableSigner

**Parameters**

- `account` 16-byte binary. The ID of the account to which the signer is added.
- `signerToDisable` JSON object. The signer to add. The struct of this JSON object depends on the type of the signer. Check [here](#43-rpc-request-signature) for the details of the struct.
- `signer` JSON object. The signer who signs the request. 
- `signature` binary. The signature of the request signed by the account.

**Returns**

None

**Note**

This RPC disables a specific signer associated with the given account. However, at least one signer must remain enabled for the account. If the target signer to disable is the enabled signer, this RPC call will fail.

## 4.5 Degas Maintenance

### 4.5.1 GetDegas

**Parameters**

- `degas` 16-byte binary string. The account ID of the Degas to be called.

**Returns**

This RPC returns all the meta-data defined in [3.1 Metadata](./degas.md#31-metadata)

**Note**

This is a read-only RPC.

### 4.5.2 CreateDegas

**Paramters**
- `serverImage` JSON-object, which is defined in [3.1 Metadata](./degas.md#31-metadata). This is a optional parameter. If the Degas has no server-side program, set paramter to null.
- `clientProgram` JSON-object, which is defined in [3.1 Metadata](./degas.md#31-metadata). This is a optional parameter. If the Degas has no client-side program, set paramter to null.
- `serverUpgradable` Boolean. True when the server-side program can be upgraded.
- `serverCores` Int. The number of CPU cores to run the server-side program required by the owner.
- `serverMemory` Int. The memory in bytes to run the server-side program required by the owner.
- `account`  16-byte binary. The account ID of the user that signs the request. This account is the default owner of the Degas to create.
- `signer` JSON object. The signer who signs the request. The structure of this object varies based on the specified signer type.
- `signature` binary. The signature of the request signed by the account.

**Ruturns**

When success, the RPC returns the account ID of the newly created Degas.

**Note**

A Degas MUST has at least a server-side program or a client-side program.
The server-side program MAY be un-upgradable. In this case, the owner cannot upgrade the server-side program anymore. But,the owner can alway upgrade the client-side program.

### 4.5.3 SetDegasOwner

**Paramters**

- `degas` The account ID of the degas.
- `newOwner` The account ID of the new owner. When set null, it means there will be no owner of this Degas.
- `account` 16-byte binary. The account ID of the user that signs the request. This must be the old owner.
- `signer` JSON object. The signer who signs the request. The structure of this object varies based on the specified signer type.
- `signature` binary. The signature of the request signed by the account.

**Returns**

None

**Note**

This RPC set a new owner of an existed Degas.

### 4.5.4 UpgradeDegas

**Paramters**
- `degas` The account ID of the degas to upgrade.
- `serverImage` JSON-object, which is defined in [3.1 Metadata](./degas.md#31-metadata). This is a optional parameter. When upgrading the server-side program, set this parameter.
- `clientProgram` JSON-object, which is defined in [3.1 Metadata](./degas.md#31-metadata). This is a optional parameter.  When upgrading the client-side program, set this parameter.
- `account`  16-byte binary. The account ID of the user that signs the request. This account must be the owner of the Degas to create.
- `signer` JSON object. The signer who signs the request. The structure of this object varies based on the specified signer type.
- `signature` binary. The signature of the request signed by the account.

**Ruturns**

None

**Note**

This RPC add a new `serverImage` or `clientProgram` to the meta-data of the Degas. If the old server-side program is running, this RPC will terminate the old image and run the latest one.

### 4.5.5 DisableServerUpgrade
**Paramters**
- `degas` The account ID of the degas.
- `account`  16-byte binary. The account ID of the user that signs the request. This account is the default owner of the Degas to create.
- `signer` JSON object. The signer who signs the request. The structure of this object varies based on the specified signer type.
- `signature` binary. The signature of the request signed by the account.

**Ruturns**

None

**None**

This RPC disable a Degas to upgrade server-side program anymore. 


### 4.5.6 SetDegasContainerResource

**Paramters**

- `degas` The account ID of the degas.
- `serverCores` Int. The number of CPU cores to run the server-side program required by the owner.
- `serverMemory` Int. The memory in bytes to run the server-side program required by the owner.
- `account`  16-byte binary. The account ID of the user that signs the request. This account is the default owner of the Degas to create.
- `signer` JSON object. The signer who signs the request. The structure of this object varies based on the specified signer type.
- `signature` binary. The signature of the request signed by the account.

**Ruturns**

None



## 4.5 Degas RPC

A Degas server-side program can offer an RPC service, and clients can access this service by calling the Degas' RPC through the Leader Node's Gateway.

To make a Degas RPC call, the client uses the `CallDegas` PRC method of the Leader Node with the following parameters.

### 4.5.1 CallDegas

**Parameter**
- `degas` 16-byte binary string. The account ID of the Degas to be called.
- `method` text string. The method of the Degas' JSON-RPC to be called.
- `paramters` JSON Object or Array. The paramters which are passed to the Deags' JSON-RPC.
- `account`  Optional. 16-byte binary. The account ID of the user that signs the request.
- `signer` Optional. JSON object. The signer who signs the request. 
- `signature` Optional. The signature of the request signed by the account.

**Retuns**

When success, the RPC returns the data from the Degas RPC.

**Note**

The Degas server-side progrma MAY requrie the user to sign the requrest when calling the RPC. It is particularly used when the RPC updates the state of the server-side program. By verifing the signature, the RPC can authenticate the user's account. Besides, only requests with signatures are stored and synchonized among the Xixels network. If the Degas server-side program wants futher proof of the correctness of the RPC call, it SHOULD require the signature.

However, a read-only Degas RPC, which dose not update the state of the server-side program, SHOULD NOT requrie the signature.

When the Gateway receives a request, it verifies the signature. If the verification is successful, the Gateway copies the `Account` field into the `Parameters` and calls the corresponding Degas' RPC method using the newly generated paramters.

The response JSON object returned from the Degas RPC is packed into the `data` object with a filed name `degasRepsonse` in the Gateway's RPC response.





