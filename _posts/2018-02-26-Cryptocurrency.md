---
layout: post
title: Bitcoin and Cryptocurrency Technologies (Princeton University)
---

I'm compiling my notes from the coursera course on Bitcoin and Cryptocurrency Technologies (Princeton University) below :

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
* Goofy coin : Goofy can create coins and transactions can occur in the system. No publishing of the blockchain. Problem is double spending.
* Scrooge coin : Same as Goofy coin except blockchain is published. Solves double spending since everyone can see the history but Scrooge signs each block to validate transactions so we have to trust Scrooge. Problem is that its centralized (Scrooge).
* Coins are immutable. So in a transaction, they are destroyed and recreated. You can subdivide or even combine coins.