# Whitepaper

## What is blockchain
## What is Crypstarter
Cryptstarter is a decentralized blockchain built to help startups access the new technology revolution and raise funds from our community.

Based on Proof-of-Stake technology, Crypstarter uses cutting-edge technology to provide unparalleled security and durability to decentralized applications, systems, and communities.

According to Cryptstarter, interoperable blockchain technology makes the global economy stronger through decentralization, accountability through transparency, and efficiency through programmable value. We also provide a user-friendly environment that allows businesses to access blockchain technology as well as grow and raise funds from our community.

Not only stopping at building an ecosystem with blockchain applications into individual business models of projects, but Crypstarter is also developed with the goal of creating a highly interconnected open economy in its Ecosystem, where all the participants can get profit through their contribution.

From the above needs, Crypstarter officially launched the concept of Proof of Investment (POI) for the first time. This is one of the brand new core values ​​of Crypstarter Blockchain in particular and blockchain technology in general.

Proof Of Investment will be a concept throughout the operating structure of Crypstarter Tokenomics and will soon become a pioneering new technology in blockchain technology. It will be a breakthrough solution for financial related applications in the future.

## Technology behind CST blockchain
### Overall Architecture
Our blockchain, built with the Cosmos SDK on top of the consensus engine Tendermint, is an application-specific blockchain, as opposed to another popular type of virtual-machine blockchain with Ethereum as one the most prominent examples. 
In a virtual-machine blockchain, the development revolves around building a decentralized application on top of an existing blockchain as a set of smart contracts. While smart contracts can be a good fit for use cases such as single-use applications (e.g., ICOs,) they often fall short for building complex decentralized platforms due to its limitations in terms of flexibility, sovereignty and performance.
On the other hand, application-specific blockchains offer a radically different development paradigm where a blockchain could be customized to operate a single application. Developers have all the freedom to make design decisions required for the application to run optimally. In addition, they can easily extend the application’s complexity with modules while not sacrificing sovereignty, security or performance.
![Blockchain architecture (courtersy Tendermint Inc.)](./static/architecture.png)
At its core, a blockchain is a replicated deterministic state machine. A state machine is a computer science concept whereby a machine can have multiple states, but only one at any given time. In a blockchain context, the state machine is deterministic, i.e., if a node is started at a given state and replays the same sequence of transactions, it will always end up with the same final state. The Cosmos SDK gives developers maximum flexibility to define the state of their application, transaction types and state transition functions. To replicate the state machine implemented with Cosmos SDK over the network, we use Tendermint. Tendermint is an application-agnostic engine that is responsible for handling the networking and consensus layers of a blockchain. In practice, this means that Tendermint is responsible for propagating and ordering transaction bytes. The interface for message exchange between Tendermint and the application is called the Application Blockchain Interface (ABCI.) It consists of a set of methods, each with a corresponding Request and Response message type.

### Cosmos SDK
#### What is Cosmos SDK?
The Cosmos SDK is the most advanced framework for building custom application-specific blockchains today. It is open source and designed to build blockchains out of composable modules with ease. At its core, the Cosmos SDK is a boilerplate implementation of the ABCI in Golang. It comes with a multistore to persist data and a router to handle transactions. An application built on top of the Cosmos SDK handles transactions from Tendermint via the DeliverTx method of the ABCI in four steps:
1. Decode transactions received from the Tendermint consensus engine
2. Extract messages from transactions and do basic sanity checks
3. Route each message to the appropriate module so that it can be processed
4. Commit state changes
#### Main components of the Cosmos SDK
##### `baseapp`
Like other Comos SDK applications, our blockchain extends from the baseapp, which is the boilerplate implementation of a Cosmos SDK application. It comes with an implementation of the ABCI to handle the connection with the underlying consensus engine. The goal of baseapp is to provide a secure interface between the store and textensible state machine while defining as little about the state machine as possible.
##### Multistore
The Cosmos SDK provides a multistore for persisting state. The multistore allows developers to declare any number of KVStores. These KVStores only accept the []byte type as value and therefore any custom structure needs to be marshalled using a codec before being stored.
The Multistore abstraction is used to divide the state in distinct compartments, each managed by its own module.
##### Modules
Cosmos SDK applications are built by aggregating a collection of interoperable modules. Each module defines a subset of the state and contains its own message/transaction processor, while the Cosmos SDK is responsible for routing each message to its respective module. 
Each module can be seen as a little state-machine. Developers need to define the subset of the state handled by the module, as well as custom message types that modify the state. In general, each module declares its own KVStore in the multistore to persist the subset of the state it defines and developers can leverage on 3rd party modules when building their own modules. 

### Tendermint
#### What is Tendermint
Tendermint is software for securely and consistently replicating an application on many machines. By securely, it means Tendermint works even if up to ⅓ of machines fail in arbitrary ways. By consistently, it means that every non-faulty machine sees the same transaction log and computes the same state. Secure and consistent replication is a fundamental problem in distributed systems; it plays a critical role in the fault tolerance of a broad range of applications, from currencies, to elections, to infrastructure or orchestration, and beyond.
Tendermint consists of two chief technical components: a blockchain consensus engine and a generic application interface. The consensus engine, called Tendermint Core, ensures that the same transactions are recorded on every machine in the same order. The application interface, ABCI, enables the transactions to be processed in any programming language. Unlike other blockchain and consensus solutions, which come pre-packaged with built in state machines, developers can use Tendermint for BFT state machine replication of applications written in whatever programming language and development environment is right for them.
#### How it works
##### Consensus
Tendermint implements a BFT consensus protocol as illustrated in the figure below.
![Tendermint's BFT consensus protocol (courtersy Tendermint Inc.)](./static/../media/tendermint_consensus.png)
Participants in the protocol are called validators; they take turns proposing blocks of transactions and voting on them. Blocks are committed in a chain, with one block at each height. A block may fail to be committed, in which case the protocol moves to the next round, and a new validator gets to propose a block for that height. Two stages of voting, namely pre-vote and pre-commit, are required to successfully commit a block. A block is committed when more than ⅔ of validators pre-commit for the same block in the same round.

Validators may fail to commit a block for a number of reasons; the current proposer may be offline, or the network may be slow. Tendermint allows them to establish that a validator should be skipped. Validators wait a small amount of time to receive a complete proposal block from the proposer before voting to move to the next round. This reliance on a timeout is what makes Tendermint a weakly synchronous protocol, rather than an asynchronous one. However, the rest of the protocol is asynchronous, and validators only make progress after hearing from more than two-thrids of the validator set. A simplifying element of Tendermint is that it uses the same mechanism to commit a block as it does to skip to the next round.

Assuming less than one-third of the validators are Byzantine, Tendermint guarantees that safety will never be violated - that is, validators will never commit conflicting blocks at the same height. To do this it introduces a few locking rules which modulate which paths can be followed in the flow diagram. Once a validator pre-commits a block, it is locked on that block. It must pre-vote for the block and can only unlock and pre-commit for a new block if there is a two-third consensus for that block in a later round.
##### Stake
In many systems, not all validators will have the same “weight” in the consensus protocol. Thus, we are not so much interested in one-third or two-thirds of the validators, but in those proportions of the total voting power, which may not be uniformly distributed across individual validators. 

Since Tendermint can replicate arbitrary applications, it is possible to define a currency, and denominate the voting power in that currency. When voting power is denominated in a native currency, the system is often referred to as Proof-of-Stake. Validators can be forced, by logic in the application, to “bond” their currency holdings in a security deposit that can be destroyed if they’re found to misbehave in the consensus protocol. This adds an economic element to the security of the protocol, allowing one to quantify the cost of violating the assumption that less than one-third of voting power is Byzantine.


## Proof Of Investment
Blockchain technology is developing very strongly in recent years. Derived from Bitcoin's Proof of Work technology, through many upgrades to meet the expanding needs of both system and applicability, new technologies, in turn, bring practical applications Much more practical, optimal, and efficient like Proof of Stake, Proof of Authority... This helps blockchain technology gradually become necessary and bring more practical applications.

Cryptstarter Blockchain is a platform that allows creating an open economy on its own structure. Unlike other projects that apply blockchain technology to a business model or solve a separate problem. Cryptstarter pursues the vision of creating an ecosystem with multiple economic models that work in sync. To achieve this, we are developing a completely new concept, **Proof of Investment (POI)** technology.

### What is POI?

### What is Crypstarter tokenomics?

### What is CST 2nd market?

## Crypstarter participants

### Investors

#### Lead Investor
Lead investors are experts in the field of investment or venture capital funds. Lead investors will play a leading role in the evaluation and analysis of projects submitted to the Crypstarter Crowdfunding platform.

#### Retail Investor
Retail investors are participants who buy and hold CST token and use it to invest in projects listed on the CST crowdfunding platform.

#### Startups/business owners
The creators of featured projects on our platform. They are startups or business owners and have a common need to apply blockchain technology to their business model and seek capital from the community to develop their products.

#### Service providers
They are experts in many fields such as lawyers, business consultants, advisors, auditors, etc., have activities on the system and contribute to projects when necessary and will be rewarded with CSTA from Crypstarter platform.

#### Crypstarter community
They are the community of CST token holders. They can participate in activities and projects on the Crypstarter Network platform through voting through the DAO mechanism. They can receive additional rewards as CSTA through staking CST Token or become a Service provider and receive rewards from the system.

## Crypstarter coin
To take full advantage of blockchain technology application and meet the structure of Crypstarter Tokenomics, Crypstarter develops dual coin mechanism including CST (BEP20 Token) and CSTA (Native Token). This mechanism helps the system operate with the highest logic while bringing maximum benefits to all parties on the Crypstarter Network.
While CST is considered the "money" of Crypstarter and the most liquid for its holders, CSTA is seen as the "energy" of the Crypstarter Ecosystem.

### Crypstarter Native Token - CSTA
CSTA is a Native token generated from the Crypstarter Blockchain itself and is considered the "energy" of the Crypstarter Ecosystem. CSTA is a coin used to pay transaction fees, rewards for staking CST Tokens, or contributions by participants in the Crypstarter Ecosystem. CSTA does not limit the maximum power supply and will always be burned to ensure transparent features and a solid system.

### Crypstarter BEP20 Token - CST
In order to globalize our project and provide the highest liquidity for Crypstarter coin holders, we create CST on Binance Smart Chain. CST is considered as the "Money" of the Crypstarter ecosystem


## Crypstarter Ecosystem

## Crypstarter Tokenomics

### Fundraising Platform

#### Crowfunding Platform for Pre-seed round

#### Crypstarter IDO Launchpad

### Startup Fundraising Process
#### Our Vision
We believe that by allowing everyone, regardless of background, to enter the realm of high technology, we are giving individuals all around the world a fresh chance. Experts at Crypstarter aim high on a wide range of topics, respecting democratization and grassroots effort.
#### Project Approval
Crypstarter has two mechanisms for project approval
1. The project is appraised and evaluated by the Lead Investors (who are the leading investors in the industry with expertise in appraisal), they will be the direct investors in the project with a large proportion. Once approved, the project will be proposed on the Cryptstarter Platform.   
2. The project is voted by Cryptstarter community by using their Governence tokens. If 51% positive votes will be announced on the Cryptstarter Platform.
#### Fundraising Project Requirements
Each fundraising project should include these categories
1. Pitch deck (pdf for Investors),
2. Business model,
3. The project’s main goals and pain points to be solved,
4. Team profile, roadmap, partners,
5. Necessary marketing documentations,
6. Videos, necessary links,
7. Investment return conditions,
8. The name of the project and token name,
9. The start and end date of the Cryptstarter funding,
10. The Amount of money need to be raise, also in tokens with details of Soft Cap and Hard Cap target
11. Information on the number of emitted tokens and the percentage of them being up for sale
#### Project Token 
Startups are required to initialize their Token in order to participate in Cryptstarter's fundraising platform. Their Project Token will be used to exchange with CST provided by Investors. Tokens can be easily generated on the Cryptstarter Platform with no blockchain knowledge required.

### What are the project ideas we want?

#### Blockchain Technologies
Blockchain is the future of the world's technology, we always welcome more startups joining this industry
#### NFT - Metaverse Project
Metaverse and NFT are becoming the leading trends in the technology field at present and in the future. Startups participating in development in this area will always be welcome
#### Impac Investing
We want to create a platform to help investors support startups in areas that have a positive impact on quality of life, environment, and society.
#### Green tech - Sustainability
Cryptstarter aims to protect the environment and conserve natural resources. Green-tech & sustainability projects are always our top priority

### Investment Process

#### Investment Process
1. Registration on crypstarter.io
2. Purchase CST Token and send those to a digital Crypstarter wallet to stake for a necessary period
3. Choose the project listed on the Crypstarter Platform
4. Exchange CST Token for the project's token of your choosing.
5. Accept the agreement with the Startups when exchanging Crypstarter Coin on
6. Investor will receive three benefits from the Startup
   a. The company share
   b. The project tokens
   c. The proof of investment
#### Co-investment method
##### The idea
Cryptstarter is the first platform that creates a co-investment system that includes an investment relationship between three parties, Startups - Lead Investors - Retail investors. When the project is appraised and invested by the Main Investor from the beginning with a minimum rate of 20%, the Retail Investor can copy the strategy for a fee to mainstream investors. Thereby, startups can access community capital more easily and transparently. 
The difference of Cryptstarter is that it allows Startups to issue Cryptocurrencies for their own project based on its Blockchain and managed by a fully-featured Smart contract like the Ethereum network. This allows investors to receive more direct economic benefits when the project's Token increases in price and also gives Startups more flexibility in raising capital through the sale of their Project Token.
##### How it works?
1. **Project appraiser**: Lead investors are our project appraisers of Crypstarter. Not only giving professional opinions, but they are also the ones who make investment decisions with a minimum rate of 20%. That's why any project they are invested in is more reliable than others.
2. **Project listing**: Project appraised by Lead Investors will be featured on their portfolio page on our platform as well as especially recommended on the Crypstarter Platform. Retail investors can choose the project they want and join our special co-investment program.
3. **Co-investment Method**: Leading investors share their investment strategies and reports on the projects they have chosen. Not everyone knows how to do Due Diligence on a project in a professional and precise way. This solution helps Retail Investors use their investment in a safer way.
4. **Fee Agreement**: Projects that were shared and invested by Lead Investors will allow Retail Investors to participate in the co-investment model easily. In return, both can agree on a suitable fee. We provides a simple and transparent way through our blockchain technology.

#### Community Activity Rewards
1. With the vision of creating a platform where all participants can get rewards, Cryptstarter offers Community Activity Rewards program to all CST Holders for contributing and operating on the platform. The system includes:
	a. Voting to approve the project
	b. Proposed system changes
2. Activities on the platform will receive a reward of xxx CSTA and be transferred directly to the CST Wallet registered on the system.

#### Service Provider Rewards
##### The concept
Cryptstarter introduces the concept of creating an open economy on its ecosystem, this is an opportunity for many parties to receive economic benefits when participating in supporting projects and receiving Project Token rewards, as well as part The reward is CST Token from Cryptstarter's system.
##### Service providers participating in the system include
a. Lawyer
b. Digital marketing
c. Graphics & design
d. Video & Animation
e. Writing & Translation
f. Business Operation
	i. Virtual Assitant
	ii. Accountant
	iii. Business consulting
g. Programming & Tech
	i. Web programming
	ii. Mobile application
	iii. Desktop application
	iv. Game development
	v. Testing
	vi. QA & Review
##### How it works?
1. **CST Holders**: Before officially registering in the system, the service provider must buy and stake a specified  percentage of CST Token according to the regulations of the system. The system will be divided into Tiers in order, the higher the value of the services, the higher the CST Token ratio required for staking. This is to ensure the quality and seriousness of service providers when participating in projects. Service Provider can be registered as an individual or an Organization/Enterprise
2. **Service provider ranks**:
   a. _Tier 1_: Hold or stake from 200,000 - 500,000 CST
   b. _Tier 2_: Hold or stake from 510,000 - 1,000,000 CST
   c. _Tier 3_: Hold or stake from 1,010,000 - 5,000,000 CST
   d. _Tier 4_: Hold or stake from 5,010,000 - 10,000,000 CST
   e. _Tier 5_: Hold or stake from 10,000,000+ CST
3. Service registration: 
   Service Provider use their CST Staking as a collateral to received the Cryptstarter Service Provider Certification. After that, they can begin support any projects on the Cryptstarter Ecosystem by dealing with the business owner thought Cryptstarter Platform.
4. Community voting: 
   Service providers that receive good reviews and feedback from the community will receive a reward of CST for their contribution. The bonus will be detailed according to each level. (for example, receiving 100 4 star reviews will receive a reward of 100 CST). The community will also receive rewards as active contributions to the system.
5. Perform service: 
   After satisfying all the required conditions, the Service provider can choose the appropriate projects to register to provide services at the request of the business owner. Agreements on milestones, fees, payments will be handled through a smart contract. When the service is completed, the Service Provider will receive a reward of CSTA from Cryptstarter and a Project token from the Business owner.

#### Virtual Loan
##### The concept
Cryptstarter introduces the concept of Virtual Loans for the first time and is seen as an extended benefit exclusively for Investors in the Cryptstarter Fundraising program. This is a special mechanism only available on the Cryptstarter ecosystem, making investment participation for startups extremely safe and attractive for investors. 
Unlike conventional investment models or other crowdfunding platforms, Investors participating in the Cryptostarter ecosystem receive various economic benefits that are not available on other platforms.
##### How it works?
1. After finished their investment, the investor will received the Cryptstarter Investor’s benefit:
	- Company's share
	- Project tokens
	- Proof of Investment
2. The special benefit for becoming an Cryptstarter Investors is you can get a Virtual Loan based on your Project tokens and the Proof of Investment. Basically the investor will receive a CST token virtual loan with a value corresponding to the amount their invested in the startup on the Cryptstarter crowdfunding platform and use it for staking to get more CSTA.
3. Investors will not have the right to withdraw, buy, sell or convert virtual loans, it will only be used for staking to receive rewards as CSTA through our staking program.
4. The process:
	i. Investor use their Proof of Investment and Project tokens to lock on our Platform as a collateral.
	ii. Investor register to our Virtual loan program
	iii. Investor receive an amount of CST Token with a value corresponding to the amount they invested
	iv. The CSTA Reward will be sent to the investor’s wallet in a period as requested
	v. Investors can sell part or all of their project tokens, then their virtual loan will decrease accordingly. All project token sale information will be automatically adjusted in their proof of investment via smart contract.
	vi. Investor will pay the virtual loan’s interest via CST/CSTA. The Virtual loan Interest will be xxxx CSTA

#### Secondary Market