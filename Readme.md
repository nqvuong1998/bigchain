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

- `BigchainDB Consortium`: The people and organizations that run the nodes in a BigchainDB network belong to a BigchainDB consortium (i.e. another organization). A consortium must have some sort of governance structure to make decisions. If a BigchainDB network is run by a single company, then the “consortium” is just that company.

>What’s the Difference Between a BigchainDB Network and a Consortium?

A BigchaindB network is just a bunch of connected nodes. A consortium is an organization which has a BigchainDB network, and where each node in that network has a different operator

### Full Decentralization and Byzantine Fault Tolerance
- In BigchainDB 2.0, each node has its own local MongoDB database [5], and all communication between nodes is done using Tendermint protocols.


>Tendermint: Tendermint is software for securely and consistently replicating an application on many machines. . By securely, we mean that Tendermint works even if up to 1/3 of machines fail in arbitrary ways. By consistently, we mean that every non-faulty machine sees the same transaction log and computes the same state.
Tendermint consists of two chief technical components: a blockchain consensus engine and a generic application interface

- Why do use Tendermint?
    - The system is solve the problem BFT.
    - When node is delete or worsted the data in that local database;, other nodes,the MongoDB databases in the other nodes won’t be affected.

- If every node in a BigchainDB 2.0 network is owned and operated by a
different person or entity, then it’s a decentralized network. Any node can fail and the rest of the network will continue to operate.

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
-  


## Reference

[What is Tendermint?](https://tendermint.com/docs/introduction/introduction.html)

[Beginner’s Guide to Tendermint: Byzantine Fault Tolerant Blockchain Engine](https://blockonomi.com/tendermint-guide/)

[Blockchain Scaling Solutions: Cosmos and Plasma](https://medium.com/tendermint/blockchain-scaling-solutions-cosmos-and-plasma-b5ee09456f80)

[Bigchain](https://www.bigchaindb.com/whitepaper/bigchaindb-whitepaper.pdf)

[Assets in blockchain](https://chain.com/docs/1.1/core/build-applications/assets)

[2018. The Year for Data Driven Blockchains](https://blog.bigchaindb.com/2018-the-year-for-data-driven-blockchains-1b8ce0b5f9)

[BigchainDB Documentation](https://bigchaindb.readthedocs.io/en/latest/)
