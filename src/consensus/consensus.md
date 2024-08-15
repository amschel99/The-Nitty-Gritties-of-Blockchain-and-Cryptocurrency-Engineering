# Distributed consensus

We should begin with the Byzantine Generals Problem. It’s a game theory problem that describes the difficulty decentralized parties encounter in arriving at a consensus without relying on a central authority. A blockchain network should be permissionless, meaning anyone can join without needing to provide their identity. If we’re building a cryptocurrency, we need a globally ordered log, or you can think of it as a distributed database. You can compare this to a traditional bank database that records transaction information. However, this database should be replicated across all nodes in the network. This process is known as state machine replication. In such a network, multiple participants are involved, and they all need to agree on the system’s state. This issue is commonly referred to as distributed consensus.

In such a system, one critical thing needs to be addressed. What if some participants in the network decide to subvert the protocol? Since the system is permissionless, an attacker can decides to create multiple nodes, essentially for free and take over the network? This is what’s known as The Sybil Attack.

Proof of work is one of the most well-known solutions to the Sybil attack. In a nutshell, to create a new block on the blockchain requires a significant amount of computing resources such as electricity.

The difficulty of PoW ensures that adding new blocks to the blockchain requires substantial computational effort. This helps protect the network against Sybil attacks by making it difficult for attackers to overwhelm the network with a flood of fake transactions or blocks.

So how does this work technically?

Each block in the blockchain contains several pieces of information:

The hash of the previous block.
A set of transactions that represent changes to the Bitcoin ledger.
A nonce, which is a random number used to create a unique hash for the block.
The target, which is a specific value that the hash of the block must be below to be considered valid.
Miners compete to find a nonce that, when combined with the other block data, produces a hash that is below the target value. This process involves repeatedly hashing the block data with different nonce values until a suitable hash is found. Since the hash function used in Bitcoin (SHA-256) is deterministic and unpredictable, finding the correct nonce requires a significant amount of computational work.

The difficulty of finding the nonce is adjusted regularly to ensure that new blocks are added to the blockchain at a relatively constant rate, approximately every 10 minutes. The difficulty is determined by the target value, which is inversely proportional to the difficulty level. A lower target value means a higher difficulty, requiring more computational work to find a valid nonce.

Miners who successfully find a valid nonce and create a new block are rewarded with a predetermined number of bitcoins, along with any transaction fees included in the block. This incentive encourages miners to participate in the network and secure the blockchain.

Basically, miners use a ton of computer power to solve puzzles and create new blocks in the Bitcoin network. It’s like they’re competing to solve a really hard math problem, and whoever solves it first gets to add a block to the blockchain and earn some bitcoins.

The problem is, this process uses up a crazy amount of electricity and resources.

Ofcourse there are other consensus mechanisms such as Proof of stake used by etheureum but we will dive into thos later on.
