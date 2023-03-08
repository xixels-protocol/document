# 5. Message Service

Message Service is a high-performance asynchronous communication service designed specifically for gaming and other scenarios. The server-side and client-side programs of Degas can communicate with each other through the Message Service. 


## 5.1 WebSocket

The Message Service uses WebSocket as the underlying communication protocol, which is a mature communication protocol supported by a large number of network infrastructures and browsers, with strong adaptability. WebSocket is defined in the [RFC 6455 standard](https://www.rfc-editor.org/rfc/rfc6455).

The server and client of the Message Service **MUST** handle the WebSocket Control Frames (Ping, Pong, and Close) correctly according to the protocol specification.

The server and client of the Message Service **MUST** only send Binary type WebSocket Data Frames and **MUST NOT** send Text type Data Frames.

## 5.2 Message Gateway
The Leader Node provides a WebSocket service as the Gateway for the Message Service.

The Gateway authenticates the client and binds the connection to a specific Account.

A Degas Container can run a WebSocket service and communicate with clients. However, messages between the Degas server-side and client-side need to be forwarded through the Gateway.

## 5.3 Frame

This section provides definitions and field meanings for all network data frames used in the Message Service.

The frames are classified into four types based on the sender and receiver of the data:

- From Client to Gateway: [ClientMessage](#532-clientmessage)
- From Gateway to Client: [GatewayMessage](#533-gatewaymessage)
- From Gateway to Degas: [GatewayInternalMessage](#534-gatewayinternalmessage)
- From Degas to Gateway: [DegasMessage](#535-degasmessage)


## 5.3.1 Protobuf

The frames transmitted in the Message Service **MUST** be encoded using Protobuf. All the Protobuf message definations **MUST** be written in the "proto3" syntax. 

## 5.3.2 ClientMessage

`ClientMessage` is the message sent from the Client to the Gateway.

```protobuf
message ClientMessage {
  oneof payload {
    ClientHelloMessage hello = 1;
    ClientEthAuthMessage eth_auth = 2;
    ClientDegasMessage degas = 3;
  }
}
```


### 5.3.2.1 ClientHelloMessage

```protobuf
message ClientHelloMessage {
    bytes account_id = 1;
    oneof signer {
        EthereumSingerMessage ethereum_signer = 2;
    }
}
```
When the client connects to the Gateway's WebSocket, it must immediately send a `ClientHelloMessage` to the Gateway. In this message, the client needs to provide its account identifier and the signer information to be used for subsequent authentication.

### 5.3.2.2 EthereumSingerMessage
```protobuf
message EthereumSingerMessage {
    string address = 1;
}
```
This message is sent when the client chooses to use an Ethereum type signer for authentication. The message contains the Ethereum address of the signer.

### 5.3.2.3 ClientDegasMessage
```protobuf
message ClientDegasMessage {
  bytes degas_id = 1;
  bytes payload = 2;
}
```
This message encapsulates the data sent by Client to Degas. The `degas_id` is the 16-byte identifier of the Degas instance that will receive the message. The `payload` field contains the specific data, with a **MAXIMUM** length of 65536 bytes.

## 5.3.3 GatewayMessage

`GatewayMessage` is the message sent from the Gateway to the Client.

```protobuf
message GatewayMessage {
  oneof payload {
    GatewayErrorMessage error = 0;
    GatewayAuthRequestMessage auth_request = 1;
    GatewayDegasMessage degas = 2;
  }
}
```

### 5.3.3.1 GatewayErrorMessage
```protobuf
message GatewayErrorMessage {
  enum ERROR_CODE {
    AUTH_FAIL = 0;
  }
  ERROR_CODE code = 0;
}
```
This message is sent by the Gateway to the Client when an error occurs.

### 5.3.3.2 GatewayAuthRequestMessage
```protobuf
message GatewayAuthRequestMessage {
    oneof request {
        EthereumAuthMessage ethereum_auth;
    }
}
```
When entering the authentication phase, the Gateway sends this message to the client, requesting the client to perform account authentication according to the specified authentication method.

### 5.3.3.3 EthereumAuthMessage
```protobuf
message EthereumAuthMessage {
    string challenge = 0;
}
```
When performing Ethereum authentication, the Gateway generates a random Challenge string and sends it to the client.

## 5.3.4 GatewayInternalMessage
`GatewayInternalMessage` is the message sent from the Gateway to the Degas.

```protobuf
message GatewayInternalMessage {
  oneof payload {
    GatewayForwardMessage client_message = 0;
  }
}
```

### 5.3.4.1 GatewayForwardMessage
```protobuf
message GatewayForwardMessage {
    bytes account_id = 0;
    bytes payload = 1;
}
```
The `payload` field of the [ClientDegasMessage](#5323-clientdegasmessage) is packaged into this message and sent to Degas.

## 5.3.5 DegasMessage

```protobuf
message DegasMessage {
  oneof payload {
    DegasToClientMessage degas_to_client = 0;
  }
}
```

## 5.3.5.1 DegasToClientMessage
```protobuf
message DegasMessage {
    bytes account_id = 0;
    bytes payload = 1;
}
```

## 5.4 Behavior

### 5.4.1 Authentication

### 5.4.2 Client Message Forward

### 5.4.2 Degas Message Forward





