# BigChainDB
## Overview
- BigchainDB is called a blockchain database because it has some blockchain properties and some database properties. 
- Below, we go through each design goal of BigchainDB 2.0 and outline how it was achieved.

<p align="center">
    <img src="https://i.imgur.com/BX9LRAvl.png"/>
</p>

## Full Decentralization and Byzantine Fault Tolerance
- In BigchainDB 2.0, each node has its own local MongoDB database [5], and all communication between nodes is done using Tendermint protocols.


>Tendermint: Tendermint is software for securely and consistently replicating an application on many machines. . By securely, we mean that Tendermint works even if up to 1/3 of machines fail in arbitrary ways. By consistently, we mean that every non-faulty machine sees the same transaction log and computes the same state.


Tendermint consists of two chief technical components: a blockchain consensus engine and a generic application interface

- Why do use Tendermint?
    - The system is solve the problem BFT.
    - When node is delete or worsted the data in that local database;, other nodes,the MongoDB databases in the other nodes won’t be affected.

- If every node in a BigchainDB 2.0 network is owned and operated by a
different person or entity, then it’s a decentralized network. Any node can fail and the rest of the network will continue to operate.

## Immutability
- The data in BigChain can not change or delete.
- The BigchainDB uses several strategies sush as:
    - Do not provide API change or delete.
    - Every node has a full copy of all the data in a standalone MongoDB database.
    - All transactions are cryptographically signed. After a transaction is stored, changing its contents will change the signature

## Owner-Controlled Assets
- Only the owner (or owners) of an asset can transfer that asset.
- BigchainDB allows external users to create as many assets.

>An asset in blockchain is a type of value that can be issued on a blockchain. All units of a given asset are fungible. Each asset has a globally unique asset ID that is derived from an issuance program. Each asset can optionally include an asset definition consisting of arbitrary key-value data. The asset definition is committed to the blockchain for all participants to see.

- User can’t create assets that appear to be
created by someone else. 

## Reference

[What is Tendermint?](https://tendermint.com/docs/introduction/introduction.html)

[Beginner’s Guide to Tendermint: Byzantine Fault Tolerant Blockchain Engine](https://blockonomi.com/tendermint-guide/)

[Blockchain Scaling Solutions: Cosmos and Plasma](https://medium.com/tendermint/blockchain-scaling-solutions-cosmos-and-plasma-b5ee09456f80)

[Bigchain](https://www.bigchaindb.com/whitepaper/bigchaindb-whitepaper.pdf)
