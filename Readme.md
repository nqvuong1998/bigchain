# BigChainDB - Scalable blockchain database
## Overview
- BigchainDB is called a blockchain database because it has some blockchain properties and some database properties. 
- It was first released — open source — in February 2016
- BigchainDB version 2.0 makes significant improvements over previous versions.

<p align="center">
    <img src="https://i.imgur.com/BX9LRAvl.png"/>
</p>

## Bigchain Design 
### Terminology
- `BigchainDB Node`: A BigchainDB node is a machine (or logical machine) running BigchainDB Server and related software. Each node is controlled by one person or organization.

- `BigchainDB Network`: A set of BigchainDB nodes can connect to each other to form a BigchainDB network. Each node in the network runs the same software. A BigchainDB network may have additional machines to do things such as monitoring.

- `BigchainDB Consortium`: The people and organizations that run the nodes in a BigchainDB network belong to a BigchainDB consortium (i.e. another organization). A consortium must have some sort of governance structure to make decisions. If a BigchainDB network is run by a single company, then the `consortium` is just that company.

>What’s the Difference Between a BigchainDB Network and a Consortium?

- A BigchaindB network is just a bunch of connected nodes. A consortium is an organization which has a BigchainDB network, and where each node in that network has a different operator.

### Full Decentralization and Byzantine Fault Tolerance
- In BigchainDB 2.0, each node has its own local MongoDB database [5], and all communication between nodes is done using Tendermint protocols.


>Tendermint: Tendermint is software for securely and consistently replicating an application on many machines. . By securely, we mean that Tendermint works even if up to 1/3 of machines fail in arbitrary ways. By consistently, we mean that every non-faulty machine sees the same transaction log and computes the same state.
Tendermint consists of two chief technical components: a blockchain consensus engine and a generic application interface

- Why do use Tendermint?
    - The system is solve the problem BFT.
    - When node is delete or worsted the data in that local database, the MongoDB databases in the other nodes won’t be affected.

- If every node in a BigchainDB 2.0 network is owned and operated by a different person or entity, then it’s a decentralized network. Any node can fail and the rest of the network will continue to operate.

### Immutability
- The data in BigChain can not change or delete.
- The BigchainDB uses several strategies sush as:
    - `No APIs for changing or deleting data.`Do not provide API change or delete.
    - `Replication.`Every node has a full copy of all the data in a standalone MongoDB database.
    - All transactions are cryptographically signed. After a transaction is stored, changing its contents will change the signature.
    - `Internal watchdogs`. All nodes monitor all changes and if some unallowed change happens, then appropriate action can be taken.
    -` External watchdogs.` A consortium may opt to have trusted third-parties to monitor and audit their data, looking for irregularities. For a consortium with publicly-readable data, the public can act as an auditor.
    - `Economic incentives.` Some blockchain systems make it very expensive to change old stored data. Examples include proof-of-work and proof-of-stake systems. BigchainDB doesn’t use explicit incentives like those.
    -`Full or partial backups` may be recorded from time to time, possibly on magnetic tape storage, other blockchains, printouts, etc.
    - `Strong security.` Node owners can adopt and enforce strong security policies.
    - `Node diversity.` Diversity makes it so that no one thing (e.g. natural disaster or operating system bug) can compromise enough of the nodes. See the section on the kinds of node diversity.

### Owner-Controlled Assets
- Only the owner (or owners) of an asset can transfer that asset.
- BigchainDB allows external users to create as many assets.

>An asset in blockchain is a type of value that can be issued on a blockchain. All units of a given asset are fungible. Each asset has a globally unique asset ID that is derived from an issuance program. Each asset can optionally include an asset definition consisting of arbitrary key-value data. The asset definition is committed to the blockchain for all participants to see.

- User can’t create assets that appear to be created by someone else. 
- BigchainDB checks every transaction to make sure it’s not trying to transfer an output that was already transferred (spent) by another transaction.

### High Transaction Rate
-  One design goal of BigchainDB has always been the ability to process a large
number of transactions each second.
- Performance tests were being written and started, but no concrete results were available yet.

### Low Latency and Fast Finality
- Tendermint-based networks (such as BigchainDB networks) take only a few seconds (or less) for a transaction to be included in a new committed block. And transaction can not revert or change, so it will be fast finally state.

### Indexing & Querying Structured Data
- Each node in Bigchain DB contains MongoDB in local. So each node operator has access to the full power of MongoDB for indexing and querying the stored data.
- Node in Bigchain can provide different operator for external user such as REST API, GraphQL API.
- Each node operator can add additional indexes and query APIs.

### Sybil Tolerance
- In this netword allow to add node to the netword. This can lead to Sybil attack.
>A Sybil attack is an attempt to control a peer network by creating multiple fake identities. To outside observers, these fake identities appear to be unique users. However, behind the scenes, a single entity controls many identities at once. As a result, that entity can influence the network through additional voting power in a democratic network, or echo chamber messaging in a social network.

- In a BigchainDB network, the governing organization behind the network controls the member
list, so Sybil attacks are not an issue.

## Use case
### Supply Chain
- In a typical supply chain , there are several parties join with each other. This major problem is management and security of infomation being shared.

### Intellectual Property Rights Management 

## BigchainDB in the Decentralization Ecosystem
- As a blockchain database, BigchainDB is complementary to other decentralized systems, such as decentralized file storage, decentralized data exchange protocols, smart-contract blockchains, and decentralized processing

## How it to do?
### BigchainDB Transactions
- A BigchainDB transaction is a JSON string that conforms to a BigchainDB Transactions Specification (Spec).
- Each transactions spec explains the expected keys and values (including what they mean), instructions for how to construct a transaction, a list of checks that must be done to check if a transaction is valid, and details of the cryptographic primitives used. Listing 1 shows an example BigchainDB transaction.

### Sending a Transaction to a BigchainDB Network
- We can send transaction to a BigchianDB network using the BigchainDB HTTP API. Example:

POST /api/v1/transactions

POST /api/v1/transactions?mode=async

POST /api/v1/transactions?mode=sync

POST /api/v1/transactions?mode=commit

<p align="center">
    <img src="https://i.imgur.com/V0AH4RWm.png">
</p>

### Arrival of a Transaction at a Node
- BigchainDB uses the Flask web application development framework.
- Flask is used to route the request to a Python method for handling that endpoint.
- Then method checks the validity of the transaction. If it is invalid, the transaction is finished, and the HTTP response statuscode is 400 (Error) and information about what was invalid. If the transaction is valid, then it's converted to Base64 and put into new JSON string with some other infomation. Then Bigchain send it to local Tendermint in body of HTTP POST request. That request uses the Tendermint Broadcast API.

### Arrival of a Transaction at a Tendermint Instance
- When a transaction is sent to a Tendermint node, it will run via `CheckTx` against the application. If it `passes` CheckTx, it will be included in the mempool, `broadcast to other peers`, and eventually included in a block. 

- There are multiple phases to processing a transaction, we offer multiple endpoints to broadcast a transaction: 
    - /broadcast_tx_async: It will return right away without waiting to hear if the transaction is even valid.
    - /broadcast_tx_sync:  It will return with the result of running the transaction through CheckTx.
    - /broadcast_tx_commit will wait until the transaction is committed in a block or until some timeout is reached, but will return right away if the transaction does not pass CheckTx. . .




## The Bitcoin Scalability Issues
### Throughput
- The Bitcoin network processes just 1 transaction per second (tps) on average, with a theoretical maximum of 7 tps.
- This throughput is unacceptably low when compared to the number of transactions processed by Visa (2, 000 tps typical, 10, 000 tps peak), Twitter (5, 000 tps typical, 15, 000 tps peak), advertising networks (500, 000 tps typical), trading networks, or email networks (global email volume is 183 billion emails/day or 2, 100, 000 tps).

### Latency
Each block on the Bitcoin blockchain takes 10 minutes to process. It is better to wait for about an hour, giving more nodes time to confirm the transaction. By comparison, a transaction on the Visa network is approved in seconds at most. Many financial applications need latency of 30 to 100 ms.

### Capacity and network bandwidth
- The Bitcoin blockchain is about 70 GB (at the time of writing); it grew by 24 GB in 2015 [21]. It already takes nearly a day
to download the entire blockchain. If throughput increased by a factor of 2, 000, to Visa levels, the additional transactions would result in database growth of 3.9 GB/day or 1.42 PB/year. At 150, 000 tps, the blockchain would grow by 214 PB/year (yes, petabytes). If throughput were 1M tps, it would completely overwhelm the bandwidth of any node’s connection.


## Reference

[What is Tendermint?](https://tendermint.com/docs/introduction/introduction.html)

[Beginner’s Guide to Tendermint: Byzantine Fault Tolerant Blockchain Engine](https://blockonomi.com/tendermint-guide/)

[Blockchain Scaling Solutions: Cosmos and Plasma](https://medium.com/tendermint/blockchain-scaling-solutions-cosmos-and-plasma-b5ee09456f80)

[Bigchain](https://www.bigchaindb.com/whitepaper/bigchaindb-whitepaper.pdf)

[Assets in blockchain](https://chain.com/docs/1.1/core/build-applications/assets)

[2018. The Year for Data Driven Blockchains](https://blog.bigchaindb.com/2018-the-year-for-data-driven-blockchains-1b8ce0b5f9)

[BigchainDB Documentation](https://bigchaindb.readthedocs.io/en/latest/)
