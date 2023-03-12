# 5. Message Service

Message Service is a high-performance asynchronous communication service designed specifically for gaming and other scenarios. The server-side and the client-side programs of Degas can communicate with each other through the Message Service. 


## 5.1 WebSocket

The Message Service uses WebSocket as the underlying communication protocol, which is a mature communication protocol supported by a large number of network infrastructures and browsers, with strong adaptability. WebSocket is defined in the [RFC 6455 standard](https://www.rfc-editor.org/rfc/rfc6455).

The server and the client of the Message Service **MUST** handle the WebSocket Control Frames (Ping, Pong, and Close) correctly according to the protocol specification.

The server and the client of the Message Service **MUST** only send Binary type WebSocket Data Frames and **MUST NOT** send Text type Data Frames.

## 5.2 Message Gateway
The Leader Node provides a WebSocket service as the Gateway for the Message Service.

the Gateway authenticates the Client and binds the connection to a specific Account.

A Degas Container can run a WebSocket service and communicate with the Clients. However, messages between the Degas server-side and the Client-side need to be forwarded through the Gateway.

## 5.3 Frame

This section provides definitions and field meanings for all network data frames used in the Message Service.

There are four main messages based on the sender and receiver of the data:

- From the Client to the Gateway: [ClientMessage](#532-clientmessage)
- From the Gateway to the Client: [GatewayMessage](#533-gatewaymessage)
- From the Gateway to the Degas server-side program: [GatewayInternalMessage](#534-gatewayinternalmessage)
- From the Degas server-side program to the Gateway: [DegasMessage](#535-degasmessage)

Each of the the main message is assocaited some payload messages. For example, `ClientMessage` has 3 payload messages: `ClientHelloMessage`, `ClientEthAuthMessage` and `ClientDegasMessage`. In the following text, when it refers to sending some payload messages, it means encapsulating the payload message in the corresponding main message and then sending the main message.

A Degas server-side program MAY function as a client and access the message services provided by the other Degas programs.

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
        NetworkAddressSignerMessage network_address_signer = 3;
        EthereumSingerMessage ethereum_signer = 2;
    }
}
```
When the Client connects to the Gateway's WebSocket, it must immediately send a `ClientHelloMessage` to the Gateway. In this message, the Client needs to provide its account identifier and the signer information to be used for subsequent authentication.

### 5.3.2.1.2 NetworkAddressSignerMessage
```protobuf
message NetworkAddressSignerMessage {
}
```
This message is sent when a Degas container connect to the Gateway. The Client MUST NEVER send this message.


### 5.3.2.1.2 EthereumSingerMessage
```protobuf
message EthereumSingerMessage {
    string address = 1;
}
```
This message is sent when the Client chooses to use an Ethereum type signer for authentication. The message contains the Ethereum address of the signer.

### 5.3.2.3 ClientEthAuthMessage
```protobuf
message ClientEthAuthMessage {
    bytes signature = 1;
}
```
### 5.3.2.4 ClientDegasMessage
```protobuf
message ClientDegasMessage {
  bytes degas_id = 1;
  bytes payload = 2;
}
```
This message encapsulates the data sent by the Client to Degas. The `degas_id` is the 16-byte identifier of the Degas instance that will receive the message. The `payload` field contains the specific data, with a **MAXIMUM** length of 65536 bytes.

## 5.3.3 GatewayMessage

`GatewayMessage` is the message sent from the Gateway to the Client.

```protobuf
message GatewayMessage {
  oneof payload {
    GatewayErrorMessage error = 1;
    GatewayAuthRequestMessage auth_request = 2;
    GatewayDegasMessage degas = 3;
  }
}
```

### 5.3.3.1 GatewayErrorMessage
```protobuf
message GatewayErrorMessage {
  enum ERROR_CODE {
    AUTH_FAIL = 0;
    DUP_SESSION = 1;
    DEGAS_ERROR = 2;
    CLIENT_ERROR = 3;
  }
  ERROR_CODE code = 1;
}
```
This message is sent by the Gateway to the Client when an error occurs.

### 5.3.3.2 GatewayAuthRequestMessage
```protobuf
message GatewayAuthRequestMessage {
    oneof request {
        EthereumAuthRequestMessage ethereum_auth_request;
    }
}
```
When entering the authentication phase, the Gateway sends this message to the Client, requesting the Client to perform account authentication according to the specified authentication method.

### 5.3.3.3 EthereumAuthRequestMessage
```protobuf
message EthereumAuthRequestMessage {
    string challenge = 1;
}
```
When performing Ethereum authentication, the Gateway generates a random Challenge string and sends it to the Client.

### 5.3.3.4 GatewayDegasMessage
```protobuf
message GatewayDegasMessage {
  bytes degas_id = 1;
  bytes payload = 2;
}
```

## 5.3.4 GatewayInternalMessage
`GatewayInternalMessage` is the message sent from the Gateway to the Degas.

```protobuf
message GatewayInternalMessage {
  oneof payload {
    GatewayForwardedMessage forwarded_message = 1;
  }
}
```

### 5.3.4.1 GatewayForwardedMessage
```protobuf
message GatewayForwardedMessage {
    bytes account_id = 1;
    bytes payload = 2;
}
```
The `payload` field of the [ClientDegasMessage](#5323-clientdegasmessage) is packaged into this message and sent to Degas.

## 5.3.5 DegasMessage

```protobuf
message DegasMessage {
  oneof payload {
    DegasToClientMessage degas_to_client = 1;
  }
}
```

## 5.3.5.1 DegasToClientMessage
```protobuf
message DegasMessage {
    bytes account_id = 1;
    bytes payload = 2;
}
```

## 5.4 Behavior

### 5.4.3 Internal Connection

When creating a Degas container, if it provides a message service, it is mandatory to register the service information with the Leader Node. The Gateway of the node always maintains an innternal WebSocket connection to the Degas container.

### 5.4.2 State

There are two states of a WebSocket connection between the Gateway and the Client:

1. AuthState
Immediately after the WebSocket connection is established, it enters the `AuthState`. In this state, the Gateway attempts to authenticate the Client using the signature of the corresponding account. Once authentication is successful, the connection enters the ForwardState.

2. ForwardState
In this state, the Gateway forwards messages between the Degas server-side programs and the Client. Message sent to and received from various Degas programs are forwareded through the same WebSocket connection, which is known as multiplexing.

3. AuthErrorState
If there is any error during the `AuthState`, the Gateway sends a [GatewayErrorMessage](#5331-gatewayerrormessage) with code `AUTH_FAIL` to the Client and close the WebSocket connection.

### 5.4.3 Authentication

Xixels supports multiply cryptographic signatures, and the Gateway uses signature-based authentication.

In the `AuthState`, the authentication process is as follows:

1. The Client send [ClientHelloMessage](#5321-clienthellomessage) to the Gateway. In this message, the Client chooses a signer bound to the account and use this signer to deal with the authenicatoin.

2. The Gateway checks if the signer is a valid signer bound to the account. If the check fails, the Gateway enter the `AuthErrorState`. Otherwise, the Gateway performs the specefic authentication according to the signer's cryptographic method.

3. The Gateway checks if there is any other WebSocket connection in the `ForwardState` associated with the same account. If such a connection exists, the Gateway sends a [GatewayErrorMessage](#5331-gatewayerrormessage) with code `DUP_SESSION` to the old connection and close it.

4. After a successful authentication, the Gateway enters the `ForwardState`. Otherwise, the Gateway enters the `AuthErrorState`.

#### 5.4.3.1 Internal Network Address Authentication

This authentication method is exclusively intended for authenticating Degas server-side programs and MUST NOT be used by the Client.

When a Degas container connects to the Gateway, the Gateway authenticates the container by its IP address. It compares the container's IP address, obtained from Kubernetes, with the IP address of the WebSocket connection's peer. If they match, the authentication is successful. Otherwise, the authentication fails.

#### 5.4.3.2 Ethereum Signature Authentication

When the Client uses an Ethereum signer, the Gateway employs a "Challenge-Response" authentication process that consists of the following steps:

1. The Gateway generates an object with two fields:
- Message: A human-readable prompt string. For example: "Welcome to Xixels!\nSign this message to sign in. This request will not trigger a blockchain transaction or cost any gas fees."
- Challenge: A random 16-byte hex string.

2. The Gateway encodes the object into a JSON string, encapsulates the string in a [GatewayAuthRequestMessage](#5332-the Gatewayauthrequestmessage), and sends it to the Client.

3. The Client signs the received JSON object using [EIP712](https://eips.ethereum.org/EIPS/eip-712) and follows a predefined parameter template.

```Javascript
// Parameter Template for signing the JSON object
{
    // The fixed domain information
    domain: {
      chainId: 1,
      name: 'Xixels Authentication',
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
        { name: 'Message', type: 'string' },
        { name: 'Challenge', type: 'string' },
      ],
    },

    message: ${RECEIVED_JSON_OBJECT}
}

```

4. The Client encapsulates signature in a [ClientEthAuthMessage](#5333-ethereumauthrequestmessage), and sends the message back to the Gateway.

5. The Gateway validates the signature. If the signature is valid, the authenication succeeds. Otherwise, the authenticatoin fails.

### 5.4.4 Client Message Forward

During the `ForwardState`, the Client is permitted to send any arbitrary payload data to the a specified server-side Degas program. The forward process is as follows:

1. The Client encapsulates the payload data in a [ClientDegasMessage](#5324-clientdegasmessage) and sent to the Gateway.

2. The Gateway retrieves the Degas ID and the payload data from the received [ClientDegasMessage](#5324-clientdegasmessage). 

3. If there is no internal WebSocket connection between the Gateway and the corresponding Degas container, the Gateway attempts to establish a connection. If the connection fails due to any reason, the Gateway drops the message and send a [GatewayErrorMessage](#5331-gatewayerrormessage) with code `DEGAS_ERROR` to the Client.

4. The Gateway encapsulates the payload data the Client's account ID in a [GatewayForwardedMessage](#5341-gatewayforwardedmessage) and sends it to the Degas container via the corresponding internal WebSocket connection.

### 5.4.5 Degas Message Forward

The Degas server-side program is permitted to send any arbitrary payload data to a specified Client. The forward process is as follows:

1. The Degas program encapsulates the payload data in a [DegasToClientMessage](#5351-degastoclientmessage) and sent to the Gateway via the internal WebSocket connection.

2. The Gateway retrieves the Account ID and the payload data from the received [DegasToClientMessage](#5351-degastoclientmessage). 

3. The Gateway searchs for an WebSocket connection in the `ForwardState` to the corresponding Client. If there is no such connection, the Gateway drops the message and send a [GatewayErrorMessage](#5331-gatewayerrormessage) with code `CLIENT_ERROR` to the Degas program.

4. The Gateway encapsulates the payload data the Degas ID in a [GatewayDegasMessage](#5334-gatewaydegasmessage) and sends it to the Client via the connection found in the previous step.





