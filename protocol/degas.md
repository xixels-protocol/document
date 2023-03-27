# 3. Degas

Degas consists of metadata, server-side programs, and client-side programs. The state data of Degas is stored on Xixels' consensus network nodes.

Although it is not mandatory, Xixels encourages developers to publish their code under an open-source license. Third-party platforms can verify whether the developer's source code matches the executable file.


# 3.1 Metadata

The metadata of Degas is maintained by the Leader Node of the consensus network and is accessible to all network users. 

In this section, we provide the definitions for all metadata. In the following sections, we will delve into the meaning of each metadata.

| Field | Type | Note |
|:------|:-----|:-----|
| account | 16-byte Binary | The unique identitier. Refer to [Account](account.md). |
| owner | 16-byte Binary | The owner of the Degas. It **MUST** be an EOA. |
| serverSideImages | Array of `serverImage` | A list of server-side images |
| serverVersion | Int | The version number of server-side program, which equals the length of `serverSideImages` minus 1 |
| clientSidePrograms | Array of `clientProgram` | A list of client-side programs|
| clientVersion | Int | The version number of client-side program, which equals the length of `clientSidePrograms` minus 1 |
| serverUpgradable | Boolean | When it is false, the server-side image cannot be upgraded anymore. |
| serverCores | Int | The number of CPU cores required to run the server-side image. |
| serverMemory | Int | The amount of memory, in bytes, required to run the server-side image. |
| serverStatus | Enum | Status code of the server-side porgram. |

`serverStatus` has the following potential values
| Value | Note |
|:------|:-----|
| 0     | The server is running. |
| 1     | The server is restarting. |
| 2     | The server is stopped. |
| 3     | The server is terminated by Leader Node. |
| 4     | The server is banned by Xixels DAO. |


`serverImage` has the following fields：
| Field | Type | Note |
|:------|:-----|:-----|
| url | String | The URL of the image. |
| hash | 32-byte Binary | Sha256 of this image. |
| entryPoint | String | The initial script in the image. |
| rpcPort | Int | The port of the RPC provided by this image. Negative value means there is no RPC service. |
| messagePort | Int | The port of the message service provided by this image. Negative value means there is no message service. |
| addTime | Unix Timestamp | The time of adding this image to the Degas. Updated by Leader Node.  |

`ClientProgram` has the following fields：
| Field | Type | Note |
|:------|:-----|:-----|
| files | Array of the ClientFile | A list of the client-side file  |
| entryPoint | String | File name of the intial script |

`ClientFile` has the following fields
| Field | Type | Note |
|:------|:-----|:-----|
| url | String | The URL of the file  |
| hash | 32-byte Binary | The Sha256 of the file |
| unzip | Boolean | When it is true, the client should unzip the file before run the program |


**Trusted External Storage**

Xixels utilizes third-party storage platforms, referred to as Trusted External Storage, to store Degas program files. The specific list of Trusted External Storage providers is determined through community governance. Any URLs referenced in the metadata must point to files stored on the Trusted External Storage.
 
# 2.2 Server-side program

Xixels offers developers the simplest and most user-friendly server-side development model, allowing developers to use any programming language for server-side program development. Server-side programs are packaged as Kubernetes images and can be run on Xixels' Leader Node.

Before deploying Degas on Xixels, developers must publish the Image on Trusted External Storage and submit the relevant URL and the hash value to the Leader Node. After verification, the Leader Node allocates a separate Kubernetes Pod for the image and runs it.

The size limit for each image is 1GB. This restriction MAY be changed by community governance.

Developers have the option to specify the required number of CPU cores and memory to run the server-side program. If the computing resources on the Leader Node do not meet these requirements, the container will not run. In cases where developers do not specify the requirements, the default values will be 1 core and 4GB of memory.

The network communication of server-side programs is strictly limited, and only the following network behaviors are allowed by the Network Policy of Leader Node:
1. Only the Gateway can access the Pod's RPC and message Service. This means that clients can only access the server-side program's RPC and message service via the Gateway.
2. The Pod can only access the Gateway's RPC and Message Service. This means that server-side programs can only access other Degas' RPC and message service as a client through the Gateway. Server-side programs cannot actively access any other network resources.
3. The Pod can access to the database service provided by Xixels.

The server-side program MAY run a JSON-RPC service that is bound to the TCP port specified by `RPCPort`. The Gateway redirects RPC requests to this port. For more information about the RPC service, refer to [here](rpc.md) .

The server-side program MAY run a message service that is bound to the TCP port specified by `MessagePort`. The Gateway redirects messages to this port. For more information about the message service, refer to [here](message.md) 

Developers have the ability to upgrade their server-side images by submitting new ServerImage data to the Xixels Leader Node. Once the Leader Node receives the new version of the image, it stops the container's operation and switches to the new version. After the upgrade, `ServerVersion` is incremented by 1 automatically. The upgrade feature is irreversible, and all historical versions of the image information are stored in `ServerSideImages`. The last element of `ServerSideImages` is the active image. Additionally, developers have the option to disable the upgrade feature by setting `ServerUpgradable` to False.

Xixels provides a database service for the server-side programs. All Degas' data that requires persistence should be stored in this database exclusively.

# 2.3 Client-side program

The client-side program is a crucial part of Degas that runs within the client's runtime. The program consists of multiple files that are stored on the Trusted External Storage. The metadata, which contains the URLs and hash values of these files, is saved on the Leader Node. Clients read the metadata to load and run the files.

Developers can upgrade the client-side program by providing new metadata. However, upgrades are irreversible, and all historical versions of the client-side program are stored in `ClientSidePrograms`. The latest record in `ClientSidePrograms` indicates the current version.

The client-side program is not packaged as one file but is recorded as independent files in the metadata. This allows different versions of the client-side program to reference the same files, minimizing the number of files that need to be downloaded during upgrades.

The client-side program can store some cached data locally, but it is not accessible to other client entities. Additionally, the Client can clear Degas' local cache at any time.

For more information on the client runtime, please refer [here]().