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

Some RPCs MAY require the caller to sign the request.

The procedure for signing is as follows:

1. If the signature result is one of the paramters in the RPC, the parameter value should be set to null.

2. Serialize the RPC request JSON object into a string format, following [JSON Canonicalization Scheme(JSC)](https://www.rfc-editor.org/rfc/rfc8785).

3. Hash the serialized JSON using the SHA-3 algorithm.

4. Sign the hash using the signer's private key.


The procedure of verification the signature.

1. Extract the signature from the parameters and set the parameter value to null.

2. Serialize the RPC request JSON object into a string format, follwing JSC.

3. Hash the serialized JSON using the SHA-3 algorithm.

4. Verify the signature of the hash using the signer's public key.

## 4.4 Account Maintenance

There are a set of RPCs for account maintenance. 

### 4.4.1 GetAccount

`GetAccount` returns the metadata of the speficied account. This account MUST be an externally-owned account.

| Parameter |  Type | Optional | Note |
|:----------|:------|:-------:|:-----|
| account | 16-byte binary | N | The account ID of the Degas to be called. |

**Returns**

This RPC returns the account information defined in [2.1 Externally-Owned Account](/protocol/account.md#21-externally-owned-account)

### 4.4.2 CreateAccount

`CreateAccount` create a new external-owned account. 

| Parameter |  Type | Optional | Note |
|:----------|:------|:-------:|:-----|
| ethereumSignerAddress | 20-byte binary | N | When using Ethereum Signer, set the signer 's address |

| Returns |  Type |  Note |
|:----------|:------|:-----|
| account | 16-byte binary | The ID of the newly created account. |


### 4.4.3 AddSigner
### 4.4.4 RemoveSigner

## 4.5 Degas Maintenance

### 4.5.1 GetDegas
### 4.5.2 CreateDegas
### 4.5.3 SetDegasOwner
### 4.5.4 UpgradeDegas
### 4.5.5 UpdateDegasResource


## 4.5 Degas RPC

### CallDegas

A Degas server-side program can offer an RPC service, and clients can access this service by calling the Degas' RPC through the Leader Node's Gateway.

To make a Degas RPC call, the client uses the `CallDegas` PRC method of the Leader Node with the following parameters.

| Parameter |  Type | Optional | Note |
|:----------|:------|:-------:|:-----|
| degas | 16-byte binary | N | The account ID of the Degas to be called. |
| method | string | N | The method of the Degas' JSON-RPC to be called. |
| paramters | Object or Array| N | The paramters passed to the Deags' JSON-RPC. |
| account | 16-byte binary| Y | The account ID of the user. |
| signerIndex | Int | Y | The index of signer that signs the request. |
| signature | Binary | Y | The signature of the request signed by the account. |

The Degas server-side progrma MAY requrie the user to sign the requrest when calling the RPC. It is particularly used when the RPC updates the state of the server-side program. By verifing the signature, the RPC can authenticate the user's account. Besides, only requests with signatures are stored and synchonized among the Xixels network. If the Degas server-side program wants futher proof of the correctness of the RPC call, it SHOULD require the signature.

However, a read-only Degas RPC, which dose not update the state of the server-side program, SHOULD NOT requrie the signature.

When the Gateway receives a request, it verifies the signature. If the verification is successful, the Gateway copies the `Account` field into the `Parameters` and calls the corresponding Degas' RPC method using the newly generated paramters.

The response JSON object returned from the Degas RPC is packed into the `data` object with a filed name `degasRepsonse` in the Gateway's RPC response.








