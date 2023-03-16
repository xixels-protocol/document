# 2. Account

An account is a basic object used in Xixels to describe users and their related attributes. Each account has a unique identifier, which is a 16-byte fixed-length binary string. Currently the network node randomly generated an identifier when creating an account.

Xixels has two types of accounts:
- 0: Externally owned account (EOA)
- 1: Degas Account

## 2.1 Externally-Owned Account
An external-owned account is an account associated with an external user.Each external-owned account has a group of signers based on cryptographic signature technology. Xixels supports various signature formats and standards. The public keys or addresses of the signers are recorded in the consensus network nodes. The nodes authenticate messages sent by users by verifying the corresponding signatures.

The fields of an externally-owned account are as follows:

| Field | Type  | Note   |
|:------|:------|:-------|
| Account.Type | Enum | 0 (EOA) |
| Account.Identifier| 16-byte Binary | |
| Account.Signers | Array | Signer Information |

The struct of the signer information veries based on the type of the signer.

### 2.1.1 Ethereum Signer Information
The Ethereum signer is compatible with the [Ethereum external address](https://ethereum.org/en/developers/docs/accounts/) stander.

The Ethereum signer has the following fields:

| Field | Type  | Note   |
|:------|:------|:-------|
| Signer.Type | Enum | 0 (Ethereum signer) |
| Signer.Nonce | Int | A number used once when signing. It begins at zero and automatically increments following each signing event.| 
| Signer.Disabled | Boolean | Whether the signer is disabled. |
| EthereumSigner.Address| Bytes20| Ethereum EOA Address|

## 2.2 Degas Account

A Degas has its own account, which is called Degas Account. Degas Account enables developers to develop one Degas as a user of another Degas and implement certain automated functions. Degas account has no signers.

The fields of an degas account are as follows:

| Field | Type  | Note   |
|:------|:------|:-------|
| Account.Type | Enum | 1 (Degas Account) |
| Account.Identifier| Bytes16 | |
