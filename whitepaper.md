
**Xixels: A Decentralized Game Infrastructure**

*Bernard*

**Motivation**

Over several decades, the gaming industry has achieved remarkable success, profoundly transforming human life. Games have brought people of different ages and backgrounds together, transcending language and cultural barriers. Furthermore, technologies that were originally developed for the gaming industry have found extensive application in fields such as education, healthcare, and beyond. Overall, the gaming industry has revolutionized the way people interact with technology, creating a whole new world of entertainment and innovation.

However, the creativity of the game industry is greatly constrained. First, with the development of the industry, the production cost of games has skyrocketed. The investment for a typical AAA game can easily reach hundreds of millions of dollars. Many excellent creative ideas cannot be implemented due to a lack of funding. Besides, large platforms monopolize the distribution resources for games. These platforms often use a narrow principle to allocate distribution resources: the platform only considers short-term financial returns as the main basis for allocation. Many excellent game products cannot obtain distribution resources fairly, thus unable to reach players. Monopolistic platforms gain the vast majority of economic benefits, leaving creators with only meager profits, which further worsens the survival of creators.

On the other hand, players invest a significant amount of time and money into games, yet the game companies hold complete control over the game's sovereignty, with no say from the players. Game companies can modify game parameters and settings at will to maximize their own profits, causing player assets to rapidly depreciate and the game experience to deteriorate. Furthermore, game companies can stop game operations at any time, causing player assets to vanish in an instant.

In the past decade, decentralized technologies represented by blockchain, such as Bitcoin and Ethereum, have made significant progress and applications. These decentralized technologies have achieved remarkable achievements in the financial field and provided ideas for the development of other industries. Some practitioners in the gaming industry are also actively exploring the combination of gaming and blockchain technology, such as issuing in-game assets on the blockchain and have made some initial achievements in exploring decentralized gaming.

The decentralized approach is the key tool to solve the problems mentioned above. As a fully open source, decentralized programmable gaming infrastructure, Xixels is on a mission to address these issues.

1. The infrastructure is built on decentralized technology. Anyone can run the platform program to provide services to the public and access the platform's state data. This ensures that no organization or individual can maliciously modify the platform's state or even terminate the platform's development and operation. Because once such an organization appears, anyone can fork the platform to continue running it. This ensures that the sovereignty of the platform belongs to the entire community, including contributors and users.
1. Xixels' applications, referred to as Decentralized Game Applications (Degas for short), are fully open and offer interoperability and composability. This empowers developers to leverage the existing programs and resources on the platform, rather than starting from scratch. Meanwhile, the platform's high level of openness means that exceptional applications can receive substantial support from the community in terms of research and development, marketing, and more, significantly reducing development costs for developers.
1. The governance of Xixels and its decentralized applications, similar to many successful blockchain projects, follows the principles of democratic community governance as an open and decentralized ecosystem. We believe that under this governance spirit, with the introduction of appropriate economic models, we can maximize the balance of interests between contributors (including Xixels developers, game developers, computing resource providers, etc.) and users, thus avoiding the monopoly of resources and governance and providing reasonable incentives for all participants.

While blockchain technology offers a path to high standards of decentralization, its strict requirements significantly limit its functionality and performance, and come with high usage costs. Xixels, on the other hand, takes a decentralized and distributed architecture approach, delivering an easy-to-use and high-performance infrastructure that meets its promise of decentralized consensus. Moreover, Xixels remains compatible with and leverages existing blockchain technology for protecting digital assets and establishing economic models.

Developers can easily create and release decentralized games on Xixels for free by leveraging Xixiels infrastructure. With the freedom to compose and interact with other games on the platform, developers can create innovative and immersive gaming experiences without the complexity and costs of building their games from scratch.

Although Xixels was originally designed to address the pain points and requirements of the gaming industry, we firmly believe that the technology we have been developing has far-reaching applications beyond just gaming. In addition to gaming, our infrastructure can facilitate various applications such as interactive movies and virtual spaces, delivering an exceptional experience to users.

**Architecture**

**…**
![](Xixels.A.Decentralized.Game.Infrastructure.001.png)

**Consensus Network**

The Xixels consensus network is a high-performance decentralized distributed network. We can imagine the services provided by the consensus network as a virtual server. Users access this massive virtual server through a client.

The consensus network provides the following main features:

- Storage for Degas code and resource files submitted by developers.
- Persistence of Degas state data.
- Encapsulation of asynchronous message communication with clients.
- API for reading external blockchain data.

Contemporary games have extremely high-performance requirements for server-side operations, and to support game operations, the Xixels consensus network prioritizes optimizing performance. On the other hand, game developers have their own familiar programming environments and development patterns. For example, game servers typically use an asynchronous programming model. In order to maximize convenience for developers to migrate to Xixels, the Xixels consensus network also provides the programming model that developers are most familiar with.

As an open and decentralized network, in addition to providing the above functions and performance, Xixels consensus network must achieve the following decentralization goals:

- Anyone can run a consensus network node and provide functional services without permission
- Anyone can access and verify the Degas code, resource files, and state data on the network

Compared to existing blockchains, the above goals do not require anyone to generate blocks on the main chain. However, these decentralization goals are sufficient to ensure that the sovereignty of Xixels belongs to all community members and users. Firstly, similar to all open-source projects, the protocol and code of Xixels are completely open source, and anyone can freely use, modify, and redistribute these codes. This also ensures that anyone can provide services for Xixels, improve and develop Xixels, and that no centralized organization or individual can truly and completely stop the Xixels service. Furthermore, the Degas code, resource files, and state data on Xixels are completely public within the consensus network. This ensures that anyone can continue to run the Xixels world from any time slice, resume or fork it. Even if some nodes in the network fail, people can always restart new nodes to provide services and continue to develop the Xixels world. This fundamentally guarantees that the Xixels world will not be controlled by any centralized organization or individual.

Blockchain networks usually have strong verifiability. Nodes in the blockchain network can automatically verify the state of the network at any time. For example, any Ethereum node can verify the result of a smart contract execution. However, the strong verifiability of blockchain often relies on a verifiable computing environment (such as EVM). The performance of such verifiable computing environments is often very low, usually unable to exceed 100 TPS on a single-core CPU. Yet games sometimes require server performance as high as 10,000 TPS on a single core. This means that a verifiable computing environment cannot meet the performance requirements of games. Additionally, such verifiable computing environments often provide a limited programming model, making it extremely difficult for developers to develop and port on it.

Unlike blockchain, Xixels consensus network does not automatically verify Degas's running results. Xixels leaves the specific verification work of Degas to developers and users. Firstly, Xixels publicly releases all code and data, which is a necessary prerequisite and guarantee for verification work. Secondly, different games usually have different requirements for verifiability. Game developers can provide specific verification programs based on the game's business logic. The verification program provided by the developer can verify all game logic or only verify key processes. For example, a MOBA game can provide a game verification program that allows users to verify the result of a specific game by replaying the game operation record. Additionally, a fully verifiable Degas can introduce zero-knowledge proof technology to strictly prove the game logic on the server-side. Thirdly, different community users can perform detailed verification on any specific core logic they are interested in. For example, a card game player can calculate the ratio of a certain scarce card to all cards and verify that the probability of card issuance is consistent with the publicly released code.

While there are no existing blockchain networks that directly meet the performance and functional requirements of Xixels for consensus networks, the development of blockchain technology has provided a wealth of infrastructure that can be used. Combining and utilizing this infrastructure can greatly simplify the development of our consensus network. For example, the code and resource files for Degass can be stored on a decentralized file system, and the nodes of the Xixels consensus network only need to store links to these files.

The top priority of building the consensus network is to meet the operational requirements of real-world applications, including modern games. Games and other applications place extremely high demands on the network’s performance, functionality, and ease of use. Balancing network performance and decentralization capability is a difficult task. Therefore, in the early stages of Xixels, only the minimum decentralization features mentioned above will be provided, and the consensus network will be continuously upgraded and decentralized in the later stages of the project. The development plan and evolution roadmap for the consensus network can be found in the "Roadmap". To compensate for the impact of weak verifiability on the security of the system at this stage, the consensus network allows assets and logic that require extremely high security to be stored on external blockchains that users approve. To this end, Xixels consensus network provides Blockchain Reader, which enables developers and users to leverage the advantages of existing blockchains.

Blockchain Reader is a set of APIs provided by Xixels to Degas for reading the state of external blockchain. This set of APIs can only read the state of the blockchain and does not have any function to update the blockchain. Blockchain-Reader solves two problems: 1. Through this set of APIs, we can introduce existing assets in the blockchain world into Xixels. For example, the Degas program can easily read a user's NFT assets and display them in its own game. 2. Due to the compromise made by Xixels consensus network on decentralization in favor of performance, critical high-security logic is not suitable for implementation in Xixels consensus network. In this case, developers can implement these logics in an external blockchain network. For example, game developers can write a very simple Ethereum smart contract to accept ETH as a deposit in the game. Xixels' Degas only needs to verify the corresponding Ethereum transaction record through Blockchain Reader APIs to proceed the deposit operation in the game.

**Client**

The client is the gateway for users to access the Xixels gaming world. Users explore the Xixels world and use various features and services provided by Degas developers through the client. If we were to make a comparison, the Xixels client is similar to a wallet in existing blockchain networks. However, unlike blockchain networks, Xixels clients are typically fat clients. The Xixels client runs a large amount of Degas code which provides rich local logic and visual experience.

A typical client would provide the following basic functionalities:

- Client APIs that can be used by Degas developers.
- User interface for user interactions.
- Visualization and rendering engine.
- Management of user's private key.
- Encapsulation of asynchronous message communication with the consensus network.

Xixels does not limit the implementation of the client or specific computing platforms. This means that the Xixels client can be implemented on various smart devices such as PCs, mobile phones, and game consoles. The client can optimize the user's interaction experience according to its physical platform.

Visualization is an important function of a gaming platform, and all visualization rendering work is completed within the client. As an open-source protocol, Xixels does not specify the specific implementation scheme for visualization. The protocol only specifies the model and format of the data that needs to be displayed. Different Xixels clients can implement different rendering methods. For example, a client that supports 3D rendering can display models on a flat display screen; a lightweight client can display models in the form of ASCII characters on a black and white character terminal; and a VR client can display models in a VR headset. Since 3D vision is currently the most mainstream user experience, Xixels will prioritize the development of clients that support 3D rendering.

In addition to vision and sounds, Xixels will also continuously expand the user's sensory model definition based on the characteristics of its physical platform. A simple example is that Xixels will define external vibration models. A client on a game console can implement this vibration as a force feedback vibration on a controller, while a client on a smartphone can call the vibration motor on the phone.

In a distributed network, using encryption technology to manage user assets is a mature solution. Similar to a blockchain wallet, the client needs to handle the user's private key carefully to balance security and usability. Xixels uses an account abstraction system and is compatible with various public-private key systems, including Ethereum, as signature providers. This maximizes the use of encrypted wallet signatures to save assets.

**Degas**

Xixels is a completely open and permissionless creation platform for developers to build the Xixels Ecosystem by writing code. The application program running on Xixels is called a Degas, which typically consists of a set of code and a set of resource files. The code is the executable program logic, while the resource files are data files, including models for visual rendering.

A Degas is usually an independent application. But Degass is not isolated from each other. On the contrary, Degass on Xixels have the ability to call each other, making them highly composable. This composable ability greatly reduces the threshold for developers and makes Xixels a truly open and completely free creation platform.

Firstly, this composability enables Degas developers to combine functionalities by calling other Degas programs, rather than starting from scratch. For example, if there is already a cool avatar Degas on Xixels, users can create their own avatars through this Degas. Then, an RPG game can invoke this avatar Degas and display the user-created avatar in the amusement park application. Users can also seamlessly call this avatar Degas without jumping out of the current game to switch to another independent app. This gives the RPG game the ability to provide users with customized avatars.

Secondly, developers do not need to develop large and comprehensive content, but only need to focus on their own creativity and develop their own product to the extreme. Other applications can reference the developer's creativity for mutual benefit. Some game enthusiasts can create new gameplay and game content based on existing Degass, such as implementing PUBG-style gameplay in an existing TPS game, creating MOBA-style battle maps in an RTS game, or displaying a different ending for an RPG game.

Finally, a developer with a certain idea may not need to fully implement it themselves. They can implement a part of the idea or framework, and leave the remaining part to be completed by other developers in the community. This open and collaborative development model maximizes the overall resources of the community, reduces development costs, and unleashes the creativity of developers.

The relationship between Degas and Xixels can be compared to that between smart contracts and Ethereum, but there are also significant differences. Smart contracts are fully deployed and run on the blockchain, and users interact with them by initiating synchronous function calls. However, Degas programs are published on Xixels, with some of the code running on the consensus network and some running on the client, with the two parts of the code communicating through asynchronous messaging. To maximize the reduction of the learning curve for developers, the programming model for Degas is also closer to traditional game development and more friendly to game developers.

The Degas code deployed on the consensus network is similar to the server-side code in the traditional server-client model, while the Degas code deployed on the client is similar to traditional client code. Xixels encapsulates all network and platform details as APIs. Xixels provides rich and powerful APIs. Developers can access various functions by calling these API interfaces.

By providing powerful and easy-to-use APIs, Xixels provides developers with a simple development environment. Developing on Xixels is as simple as developing in a local environment. Developers do not need to understand complex network mechanisms, but only need to focus on their creativity itself.

**Applications**

**Games**

The original intention of Xixels is to provide a friendly decentralized environment for game developers to develop and run their games, and to create a game platform where players have real ownership of their digital assets. Xixels provides open API interfaces for developers to develop and deploy single-player, multiplayer, and MMO games in a decentralized manner. Supported platforms include iOS/Android/Browser/PC. Users can easily access their games and in-game assets across different platforms.

As anyone can resume, fork, or run Xixels from any point in time, this limits the power of developers to arbitrarily modify game content and in-game asset properties. In addition, the traditional game industry practice of voluntarily shutting down a game will no longer exist. As long as there is consensus among the player community, the community can take over the operation of a game and even continue to develop new content for the game.

**Digital Asset Creation Tool**

Developers can use open APIs to develop and deploy their own digital asset creation tools. Creators can also create and publish digital assets based on these tools, and the published digital assets can be permanently stored in Xixels or in decentralized storage systems supported by Xixels, such as IPFS/ARweave. Digital creation tools can be flexibly integrated into other Degass through their composability, which means that different Degass can seamlessly access these digital creation tools within the program. This enables excellent digital creation tools to be widely applied.

**Interactive Movies**

Creators can quickly create interactive movies with complex branches and logic using open APIs. Due to the composability of the system, creators can use the results or even intermediate processes of existing works as inputs for new works, thereby achieving cross-work plot linkage. Moreover, due to the openness of the system, the community can create derivative works based on existing works to form its own unique plot and ending, thus expanding the content and vitality of interactive movies.

**Real-time Rendering Animation/Interactive Hot Update Animation**

Unlike traditional animation playback, Xixels allows real-time rendering of animation on the client-side. This enables animations to transition from broadcast mode to interactive mode and allows for flexible modification and updating of content after the animation is released. This also allows derivative creators to use existing materials to depict their own envisioned animation scenes.

**Virtual Space**

Developers can create virtual spaces for their own content, companies/entities can customize their virtual office areas, and community users can customize their own exclusive spaces. The composability and openness of Xixels allows for rich possibilities in space display.

**Roadmap**

Xixels is a gaming infrastructure. The difficulty in building such infrastructure lies in the following contradiction: when the infrastructure is not mature enough, there are often no ecological applications, which means there are no end-users; and without ecological applications and users, it is impossible to promote the iterative development of the infrastructure. To solve this contradiction, the construction of Xixels will adopt a synchronous development approach of infrastructure and ecological applications. We do not pursue building a perfect infrastructure and then introducing developers to build applications. Instead, we will actively introduce developers at each iterative stage and build the Xixels ecosystem simultaneously. Under this approach, we will simplify the design of the consensus network in the early stage of Xixels, only introducing the most basic open-source and decentralized ideas but providing usable client functions. This enables the Xixels project to deliver an excellent user experience to users early on, which helps to form a virtuous Product-Market-Fit cycle.

We currently divide the project's development into the following three stages:

**Galileo**

In this stage, our goal is to release a fully functional version of Xixels. Developers can publish Degass on Xixels, and users can access Xixels through the client. All data on Xixels is decentralized, which means the survival and development of Xixels do not depend on any centralized organization. On the other hand, to launch Xixels to developers and users as soon as possible, the project will simplify and compromise in the following areas in this stage:

1. There is only one Leader node in the consensus network, and all clients interact with the Leader node. Anyone can run a Follower node, and the Follower stage subscribes to data from the Leader node, saving and verifying the entire consensus state. Like other blockchain networks, we will have a so-called "mainnet," and the Leader node of the mainnet is run by the community. Of course, anyone can also fork the consensus network and run their own independent Leader node.
1. In this stage, the consensus network does not introduce an economic model. The difficulty of running developer programs on an open platform like a blockchain is how to allocate and use computing resources. Without restrictions, malicious or poor-quality Degas programs can easily consume computing resources. A straightforward solution is to introduce an economic model and price computing resources. Users need to pay the necessary costs to use computing resources, thereby avoiding unreasonable use of resources and solving the resource allocation problem through marketization. In this stage, we simplify the consensus network design and do not introduce an economic model, which also means that developers cannot deploy Degass on the mainnet without permission.

For the second restriction, only Degass approved by community governance can be deployed on the Xixels mainnet in this stage. Although there are such restrictions, anyone can still run their independent Xixels instance through forking, thereby obtaining the right to develop and use Xixels without permission.

In this stage, the Xixels community will release at least one client with a complete 3D visual engine. This client can run on smartphones, PCs, and game consoles. After completing this stage, we can provide users with a very excellent user experience, introduce developers to develop truly user-friendly Degas products, and bring real users to Xixels.

**Newton**

In this stage, our main goal is to make it possible to deploy Degass on the mainnet without permission. This will make Xixels a completely creative platform. It is worth noting that although deploying Degass on Xixels will not require permission after this stage is achieved, different client implementations may still selectively run Degas programs based on specific laws or community rules to meet compliance requirements.

The focus of this stage is to introduce a tokenomics model. The model needs to address three issues:

- Incentivizing nodes to provide computing resources for running Xixels.
- Incentivizing excellent Degas developers to create great game content.
- Charging developers or users to pay for the use of Xixels’ resources.

**Einstein**

In this stage, our goal is to further enhance the decentralization of the consensus network:

1. To eliminate the single point of failure problem of Leader nodes in the network as much as possible, making Xixels more robust.
1. Enhancing the verifiability of the consensus network, making it relatively easy for the community to verify the network status.

The technical challenges in this stage are great. It should be pointed out that the degree of decentralization of the consensus network and the performance of the network are a pair of contradictions that cannot be completely resolved. However, meeting the performance requirements of the Degass is a higher priority. Therefore, in this stage, we can only strive to enhance the decentralization of the network as much as possible, rather than attempting to create a utopian network that is both highly performant and decentralized.

As we are still in the early stage of the project and the entire blockchain technology is still rapidly evolving, we will not make specific technical plans for this stage for now. Instead, we will do detailed planning and design after the start of this stage.

**Community**

Xixels is an open and free infrastructure that is entirely driven, developed, and shared by the community. The project's development is collectively completed by developers in the community. In theory, the Xixels community defines the Xixels protocol, including API interface specifications, data transfer specifications, resource data format specifications, and the implementation of this protocol is also completed by the community. We encourage different developers to develop different nodes and clients while adhering to protocol specifications to maximize the richness of the ecosystem and usage scenarios.

As an open content creation platform, Xixels' success is heavily reliant on Degas developers' creativity. The community will cultivate, assist, and motivate outstanding Degas developers to create a gaming platform with excellent user experience.

Xixels' governance follows the principles of community governance. In the early stages of the project, the community is led by core contributors. Core contributors should actively listen to community opinions, develop community governance rules, and hold regular meetings to develop project plans and promote project development. In the process of project development, especially after the introduction of the economic model, the project's governance process should be continuously optimized. Project governance should move towards a more decentralized and democratic direction.



