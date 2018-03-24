---
layout: post
title: Bitcoin and Cryptocurrency Technologies (Princeton University)
---

I'm compiling my notes from the coursera course on Bitcoin and Cryptocurrency Technologies (Princeton University) below :

### Week 1
**Crypto hash fns**
* Bitcoin hash is 256 bits.
* Cryptograpic hash fn are hash fns that have the following properties:
  * Nobody can find a collision (even though they do exist).
    * Hash useful as a message digest. Compare 256 bit hashes to detect same object instead of actually comparing the objects.
  * Hiding : Given H(x), it's impossible to find x.
    * Achieve this by concat x with r (chosen from a min-entropy distribution i.e. highly spread out probability distribution) and hashing that i.e., given H(concat(r,x)), its impossible to find x. 
    * Analogy : Seal value in envelope (commit to a value) and put it out there. Later we can open the envelope.
    * Commitment API
      * commit(v) returns (H(concat(k,v)), k). Used to seal a value v and returns tuple. k is a random 256 bit value.
      * verify(c,k,v) returns (H(concat(k,v)) == c). Used to verify that a value v was the same as the value in the commitment.
      * API has the hiding property and also the collision free property.
  * Puzzle friendly: Given y (high min entropy) and k, it is impossible to find x where H(concat(k,x))=y.
    * Application is search puzzle. Given puzzle id and target set Y, find solution x where H(concat(id,x)) in Y.
    * This property implies that there is no solving strategy better than trying random values of x to solve the puzzle.
    * Puzzle is used for bitcoin mining.
* Bitcoin uses SHA-256 as crypto hash fn.
  * How it works : Takes msg and breaks into blocks that are 512 bits in size (last block has padding). Start by selecting 256 initial value from some table. Then you pass 256 bit and first 512 bit of the msg to a compression fn c that returns 256 bits. Keep repeating it till last block of msg is reached. Output is the hash.
  * If compression fn is collision free, the entire function is collision free.

**Hash pointer data structure**
* Hash pointer data structure : Pointer to where data is stored and also has crypto hash of the data. The hash allows us the make sure that the data wasn't tampered.
* Linked list built with hash pointer is a blockchain. It's tamper free because if someone changes the value in one node, this changes the node data (data = value + hash). Thus, the prev node's hash value is not going to match the new data's hash value (because collision free hash fn). So now attacker has the modify the prev hash value. But then he has changed the prev data now. So this keeps repeating till head of linked list.
* Binary tree build from hash pointers is called Merkle tree. Only the leafs store the values. Advantage is that we can show if a block (value) exists in the tree in O(log n) by starting from the root hash (256 bits). If we maintain a sorted list of values on the leafs, we can even show non-membership in O(log n) time.
* Hash pointer's can be used in any data structure as long as there are no cycles (as the hashes won't match up).

**Digital signatures**
* Only you can see and anyone can verify.
* Signature is tied to a specific doc. Can't be copied.
* API for digital signatures:
  * generateKeys(keysize) returns (sk, pk). Randomized algo.
  * sign(sk, msg) returns sig. Randomized algo.
  * verify(pk, msg, sig) returns isValid. Deterministic algo.
  * Valid signatures verify and you cant' forge signatures.
* We use hash of msg as input to the digital signature.
* If you sign a hash pointer at the end of the blockchain, you're siging the entire contents of the blockchain.
* Bitcoin uses ECDSA for digital signing. Randomness is very impt to generating the keys and signing the keys.

**Decentralized identity management**
* Public key is an identity of a person/actor i.e., the public key "says" a message. But to create the message, you need to use secret key which only you control.
* Decentralized identity management : You can create a new public key (public identity) and secret key (your private control of the identity) if you want. You're anonymous when publishing a message because nobody knows your secret key and they only see your public key that you can keep changing if you want.
* Bitcoin address is a public identity.

**A simple cryptocurrency**
* Double spending : Spending the same coin twice.
* Goofy coin : Goofy can create coins and transactions can occur in the system. Problem is double spending because blockchain is not published by Goofy.
* Scrooge coin : Same as Goofy coin except blockchain is published. Solves double spending since everyone can see the history but Scrooge signs each block to validate transactions so we have to trust Scrooge. Problem is that its centralized (Scrooge).
* Coins are immutable. So in a transaction, they are destroyed and recreated. You can subdivide or even combine coins.

### Week 2
**Decentralization**
* Not all systems are all-or-nothing. Mix between centralization and decentralization.
* Aspects of decentralization in bitcoin :
  * Peer-to-peer network.
  * Mining : But high centralization because of how much computation you need to mine.
  * Updates to software : Core devs trusted by community.

**Distributed consensus**
* How to decentralize Scrooge coin.
* Why consensus? Reliability in distributed systems.
* Defining distributed consensus :
  * Protocol terminates and all correct nodes decide on the same value.
  * The value must have been proposed by some correct node.
  * e.g., A wants to pay B so she broadcasts the transaction to all bitcoin nodes. Data in transaction : Signature of A, public key of B and hash (receipt).
* How consensus could work in bitcoin : 
  * All nodes have a sequence of blocks of transactions they've reached consensus on.
  * Each node has a set of outstanding transactions it's heard about.
  * Disadv : Nodes may crash, nodes may be malicious, not all pairs of nodes connected, faults in network, latency, no notion of global time.
* Impossibility results for consensus protocols :
  * Byzantine generals.
  * Fischer-Lynch-Paterson : consensus impossible with a single faulty node.
* Protocols : 
  * Paxos : Under certain conditions, the protocol can be stuck. But this model doesn't carry over to bitcoin as it was developed for distributed DBs.
* How bitcoin is different :
  * Introduces incentives.
  * Embraces randomness. Consensus only has to be reached over long time scales.

**Consensus without identity**
* Traditionally need identites for security and some protocols also rely on node ids.
* Why no identities?
  * No central authority to give identities to node- Sybil attack (same attacker mimcks many people but really, its just one attacker).
  * Pseudonymity is a goal of bitcoin.
* Weaker assumptions :
  * Can pick a random node in the system. Give token to nodes and later call on that node, like a raffle/lottery.
  * Sybil nodes together get 1 token.
* Implicit consensus :
  * Select random node.
  * Node proposes next block in the chain.
  * Other nodes implicity accept/reject the block.
* Consensus algo in bitcoin :
  * New transactions are broadcast to all nodes.
  * Each node collects new transactions into a block.
  * A random node gets to broadcast its block.
  * Other nodes accept the block only if all transactions in it are valid.
  * Nodes express acceptance by including its hash in the next block they create.
* How can attacker break this :
  * Steal bitcoins belonging to different user?
    * Nope, because attacker cannot forge a signature (due to cryptography).
  * A denys service to B by not including B's transaction in any node A proposes :
    * B just waits for another block to be proposed by a honest node.
  * Double spending attack : 
    * Spender makes 2 transactions and publishes. Both are legitimate.
    * Due to network latency, some honest nodes will accept 1 transaction while others might accept another.
    * Honest nodes are following the policy of always extending the longest chain.
    * However, merchant can be sure of the correct transaction by tracking the number of confirmations of acceptance.
    * Double spend probability decreases exponentially with the number of confirmations.
  * To recap, protection against invalid trasactions is cyptographic but enforced by consensus. Protection against double spending is purely by consensus. You're never 100% sure a transaction is in consensus branch. Guarantee is probabilistic.

**Incentives and Proof of work**
* Can we give incentives for nodes to behave honestly?
  * Incentive 1 : Block reward.
    * Creator of the block can add coin creation transaction to the block and choose a recipient (itself).
    * Currently fixed at 25 bitcoins but halves every 4 years.
    * Block creator only gets those coins if that block is added to the longest chain.
    * Block reward is how bitcoin is created. No other way.
    * Creation runs out in 2140. So that means total supply of bitcoin is limited to 21 million.
    * Does that mean that after 2140, the nodes don't have any incentive to behave honestly?
  * Incentive 2 : Transaction fee.
    * The creator gets the fee. As block rewards get lower, these fees will increase.
* Remaining problems?
  * How to pick a random node?
  * How to avoid free-for-all due to rewards?
  * How to prevent Sybil attack?
* Solution to all 3 : Proof of work.
  * Select a random node :
    * In proportion to amount of computing power called proof of work (used in bitcoin). 
    * In proportion to amount of bitcion owned called proof of stake (not used in bitcoin).
  * Hash puzzle.
    * In order to create a block, find nonce st H(nonce \|\| prev_hash \|\| prev_transactions) is very small and falls within a target space (a really small space within the output space much smaller than 1%).
    * If hash function is secure, the only way to succeed solving the puzzle is to try enough nonces until you get lucky. Bernouili trial.
  * PoW properties :
    * Property 1 : Difficult to compute. 
      * Target output space is 1/10^20 of output space. 
      * Only miners will compete to create blocks.
    * Property 2 : Size of target space is recalculated every 2 weeks. 
      * Avg time between 2 successive blocks produced globally is 10 mins.
      * This means if more hardware is added, the target space will be made smaller. 
      * Hence, P(A wins block creation) = fraction of global hash power she controls.
      * Why maintain this property? If blocks created close together, there would be a lot of inefficiency, and we lose the optimization benefits to put lot of transactions in a single block. So block latency will increase. Need to keep block latency fixed.
      * If majority of miners weighted by hash powers are honest, most of the attacks are not possible. Bec comptetion ensures that at least >50% of new nodes are proposed by an honest node.
      * Mean time to find block = 10 min / fraction of global computing power.
    * Property 3 : Trivial to verify that nonce was correct.
      * Nonce is published as part of the block.
      * Other nodes can verify that published nonce falls in the target output space.

**Brief recap**
* Mining economics : If mining reward is greater than hardware and electricity cost, then miner can make a profit.
  * But reward is in bitcoin and the costs are in fiat.
  * Reward depends on global hash rate.
* Consensus types in bitcoin :
  * Exchange rate of bitcoin to fiat.
  * Other nodes agree that I own a certain amount of bitcoin.
  * Rules of the system.
* How to ensure security of blockchain? Pre-requisite is to have honest ecosystem. How to ensure honesty? Pre-requisite is if exchange rate of bitcoin is high (because of mining reward). How to ensures high and stable value of bitcoin? This can only happen if blockchain is secure. All three are linked.
* What happens if attacker has >50% of nodes (subvert consensus).
  * Can attacker steal coins?
    * Create a new longer branch with invalid transactions. But recipient will reject the longest branch because its not a valid branch since the signatures of the invalid transactions don't check out. So attacker can't really steal coins by subverting consensus.
  * Can attacker suppress some transactions?
    * Refuse to create new blocks that contain those transactions. So blockchain can be subverted. But those transactions will still reach the rest of the peer-to-peer network. So the attack can be detected.
  * Can attacker change the block reward?
    * These rules of the system cannot be changed because attacker cannot control the bitcoin protocol that the rest of the nodes are running.
  * Can attacker destroy confidence of bitcoin?
    * Blockchain is no longer decentralized so people won't trust bitcoin anymore.
* As a block gets deeper in the chain, it gets more difficult to fork the chain from that earlier point. This is because to create a longer chain, you now need to add a lot more blocks and adding blocks requires PoW.
* What else can we do with consensus?