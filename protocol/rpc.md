# 4. RPC

The Leader Node of Xixels provides RPC services. This set of RPC services mainly includes the following functions:
- Querying status of the node and Degas
- Deploying and maintaining Degas
- Maintaining account information
- Gateway for Degas RPC

## 4.1 JSON-RPC

The RPC services comply with the [JSON-RPC 2.0 Specification](https://www.jsonrpc.org/specification).


## 4.2 Hex Value Encoding

In Node RPC, binary data is encoded in Hex with the string "0x" as a prefix. The valid characters for Hex encoding, besides the "0x" prefix, are [0-9,a-f,A-F], which are case-insensitive. The encoded result is used as a string in the RPC request or response. The Hex data type referred to in the following text is encoded in this way.

## 4.3 XIXELS_CALL

