---
layout: post
title: Bitcoin and Cryptocurrency Technologies (Princeton University)
---

I'm compiling my notes from the coursera course on Bitcoin and Cryptocurrency Technologies (Princeton University) below :

### Week 1 : Intro to Crypto.
**Crypto Hash Functions**
* Bitcoin hash is 256 bits.
* Cryptograpic hash functions are hash functions that have the following properties :
  1. Nobody can find a collision (even though they do exist).
    * Hash useful as a message digest. Compare 256 bit hashes to detect same object instead of actually comparing the objects.
  2. Hiding : Given H(x), it's impossible to find x.
    * Achieve this by concat x with r (chosen from a min-entropy distribution i.e. highly spread out probability distribution) and hashing that i.e., given H(concat(r,x)), its impossible to find x. 
    * Analogy : Seal value in envelope (commit to a value) and put it out there. Later we can open the envelope.
    * Commitment API :
      * commit(v) returns (H(concat(k,v)), k). Used to seal a value v and returns tuple. k is a random 256 bit value.
      * verify(c,k,v) returns (H(concat(k,v)) == c). Used to verify that a value v was the same as the value in the commitment.
      * API has the hiding property and also the collision free property.
  3. Puzzle friendly : Given y (high min entropy) and k, it is impossible to find x where H(concat(k,x))=y.
    * Application is search puzzle. Given puzzle id and target set Y, find solution x where H(concat(id,x)) in Y.
    * This property implies that there is no solving strategy better than trying random values of x to solve the puzzle.
    * Puzzle is used for Bitcoin mining.
* Bitcoin uses SHA-256 as crypto hash fn.
  * How it works : Takes msg and breaks into blocks that are 512 bits in size (last block has padding). Start by selecting 256 initial value from some table. Then you pass 256 bit and first 512 bit of the msg to a compression fn c that returns 256 bits. Keep repeating it till last block of msg is reached. Output is the hash.
  * If compression fn is collision free, the entire function is collision free.

**Hash Pointer Data Structure**
* Hash pointer data structure : Pointer to where data is stored and also has crypto hash of the data. The hash allows us the make sure that the data wasn't tampered.
* Linked list built with hash pointer is a blockchain. It's tamper free because if someone changes the value in one node, this changes the node data (data = value + hash). Thus, the prev node's hash value is not going to match the new data's hash value (because collision free hash fn). So now attacker has the modify the prev hash value. But then he has changed the prev data now. So this keeps repeating till head of linked list.
* Binary tree build from hash pointers is called Merkle tree. Only the leafs store the values. Advantage is that we can show if a block (value) exists in the tree in O(log n) by starting from the root hash (256 bits). If we maintain a sorted list of values on the leafs, we can even show non-membership in O(log n) time.
* Hash pointer's can be used in any data structure as long as there are no cycles (as the hashes won't match up).

**Digital Signatures**
* Only you can see and anyone can verify.
* Signature is tied to a specific doc. Can't be copied.
* API for digital signatures :
  * generateKeys(keysize) returns (sk, pubk). Randomized algo.
  * sign(sk, msg) returns sig. Randomized algo.
  * verify(pubk, msg, sig) returns isValid. Deterministic algo.
  * Valid signatures verify and you can't forge signatures.
* We use hash of msg as input to the digital signature.
* If you sign a hash pointer at the end of the blockchain, you're signing the entire contents of the blockchain.
* Bitcoin uses ECDSA for digital signing. Randomness is very impt to generating the keys and signing the keys.

![_config.yml]({{ site.baseurl }}/images/signing.png)

**Decentralized Identity Management**
* Public key is an identity of a person/actor i.e., the public key "says" a message. But to create the message, you need to use secret key which only you control.
* Decentralized identity management : You can create a new public key (public identity) and secret key (your private control of the identity) if you want. You're anonymous when publishing a message because nobody knows your secret key and they only see your public key that you can keep changing if you want by calling generateKeys again.
* Bitcoin address is a public identity.

**A Simple Cryptocurrency**
* Now lets use what we learn so far to show how a simple cryptocurrency could work.
* Goofy coin : 
  * Goofy can create coins and transactions can occur in the system.
  * Creates a coin by creating a message "Createcoin[coinId]".
  * Signs the message with private key.
  * Signature and message is a coin.
  * People can verify this by using signature, message and public key.
  * Goofy can send this coin to Alice by creating message "Pay this to Alice's public key" where "this" refers to to the coin's hash pointer.
  * Problem is double spending (spending the same coin twice) because blockchain is not published by Goofy.
* Scrooge coin : 
  * Same as Goofy coin except blockchain is published and append only. 
  * Solves double spending since everyone can see the history. 
  * Problem is that its centralized (Scrooge) i.e., Scrooge signs each block to validate transactions so we have to trust Scrooge. Scrooge could deny service to specific users by never validating their transactions.
* Coins are immutable. So in a transaction, they are destroyed and recreated as change. You can subdivide or even combine coins.

### Week 2 : How Bitcoin achieves Decentralization.
**Decentralization**
* Not all systems are all-or-nothing. Mix between centralization and decentralization.
* Aspects of decentralization in Bitcoin :
  * Peer-to-peer network.
  * Mining : But high centralization because of how much computation you need to mine.
  * Updates to software : Core devs trusted by community.

**Distributed Consensus**
* How to decentralize Scrooge coin.
* Why consensus? Reliability in distributed systems.
* Defining distributed consensus :
  * Protocol terminates and all correct nodes decide on the same value.
  * The value must have been proposed by some correct node.
  * e.g., A wants to pay B so she broadcasts the transaction to all Bitcoin nodes. Data in transaction : Signature of A, public key of B and hash (receipt).
* How consensus could work in Bitcoin : 
  * All nodes have a sequence of blocks of transactions they've reached consensus on.
  * Each node has a set of outstanding transactions it's heard about.
  * Disadv : Nodes may crash, nodes may be malicious, not all pairs of nodes connected, faults in network, latency, no notion of global time.
* Impossibility results for consensus protocols :
  * Byzantine generals.
  * Fischer-Lynch-Paterson : consensus impossible with a single faulty node.
* Protocols : 
  * Paxos : Under certain conditions, the protocol can be stuck. But this model doesn't carry over to Bitcoin as it was developed for distributed DBs.
* How Bitcoin is different :
  * Introduces incentives.
  * Embraces randomness. Consensus only has to be reached over long time scales.

**Consensus without Identity**
* Traditionally need identites for security and some protocols also rely on node ids.
* Why no identities?
  * No central authority to give identities to node : Sybil attack (same attacker mimcks many people but really, its just one attacker).
  * Pseudonymity is a goal of Bitcoin.
* Weaker assumptions :
  * Can pick a random node in the system. Give token to nodes and later call on that node, like a raffle/lottery.
  * Sybil nodes together get 1 token.
* Implicit consensus :
  * Select random node.
  * Node proposes next block in the chain.
  * Other nodes implicity accept/reject the block.
* Consensus algo in Bitcoin :
  1. New transactions are broadcast to all nodes.
  2. Each node collects new transactions into a block.
  3. A random node gets to broadcast its block.
    * (Jumping ahead) Proof of work (i.e., finding the nonce that solves a hash puzzle of a block) determines the node that broadcasts its block. Solving this hash puzzle is completely random, but if you control more computing resources, your chances of solving the puzzle increases, which means your chances of proposing the new block increases (thereby obtaining the mining reward and the transaction fees).
  4. Other nodes accept the block only if all transactions in it are valid.
  5. Nodes express acceptance by including its hash in the next block they create.
* Finding holes in the consensus algo :
  * Can an attacker steal Bitcoins belonging to different user?
    * Nope, because attacker cannot forge a signature (due to cryptography).
  * Can A deny service to B by not including B's transaction in any node A proposes?
    * B just waits for another block to be proposed by an honest node.
  * Can a double spending attack be carried out?
    * Spender makes 2 transactions and publishes. Both are legitimate.
    * Due to network latency, some honest nodes will accept 1 transaction while others might accept another.
    * Honest nodes are following the policy of always extending the longest chain.
    * However, merchant can be sure of the correct transaction by tracking the number of confirmations of acceptance.
    * Double spend probability decreases exponentially with the number of confirmations.
  * To recap, protection against invalid trasactions is cyptographic but enforced by consensus. Protection against double spending is purely by consensus. You're never 100% sure a transaction is in consensus branch. Guarantee is probabilistic.

**Incentives and Proof of Work**
* Can we give incentives for nodes to behave honestly?
  * Incentive 1 : Block reward.
    * Creator of the block can add coin creation transaction to the block and choose a recipient (itself).
    * Currently fixed at 25 Bitcoins but halves every 4 years.
    * Block creator only gets those coins if that block is added to the longest chain.
    * Block reward is how Bitcoin is created. No other way.
    * Creation runs out in year 2140. So that means total supply of Bitcoin is limited to 21 million.
    * Does that mean that after year 2140, the nodes don't have any incentive to behave honestly?
  * Incentive 2 : Transaction fee.
    * The creator gets the fee. As block rewards get lower, these fees will increase.
* Remaining problems?
  * How to pick a random node?
  * How to avoid free-for-all due to rewards?
  * How to prevent Sybil attack?
* Solution to all 3 : Proof of work.
  * Select a random node :
    * In proportion to amount of computing power called proof of work (used in Bitcoin). 
    * In proportion to amount of Bitcoin owned called proof of stake (not used in Bitcoin).
  * Hash puzzle :
    * In order to create a block, find nonce st H(nonce \|\| prev_hash \|\| prev_transactions) is very small and falls within a target space (a really small space within the output space much smaller than 1%).
    * If hash function is secure, the only way to succeed solving the puzzle is to try enough nonces until you get lucky. Bernouili trial.
  * PoW properties :
    * Property 1 : Difficult to compute. 
      * Target output space is 1/10^20 of output space. 
      * Only miners will compete to create blocks.
      * Ensures no DDOS attack by attackers who want to create multiple fradulent blocks because its too expensive to create blocks. Prevents free-for-all.
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

**Brief Recap**
* Mining economics : If mining reward is greater than hardware and electricity cost, then miner can make a profit.
  * But reward is in Bitcoin and the costs are in fiat.
  * Reward depends on global hash rate.
* Consensus types in Bitcoin :
  * Exchange rate of Bitcoin to fiat.
  * Other nodes agree that I own a certain amount of Bitcoin.
  * Rules of the system.
* How to ensure security of blockchain? Pre-requisite is to have honest ecosystem. How to ensure honesty? Pre-requisite is if exchange rate of Bitcoin is high (because of mining reward). How to ensures high and stable value of Bitcoin? This can only happen if blockchain is secure. All three are linked.
* What happens if attacker has >50% of nodes (subvert consensus).
  * Can attacker steal coins?
    * Create a new longer branch with invalid transactions. But recipient will reject the longest branch because its not a valid branch since the signatures of the invalid transactions don't check out. So attacker can't really steal coins by subverting consensus.
  * Can attacker suppress some transactions?
    * Refuse to create new blocks that contain those transactions. So blockchain can be subverted. But those transactions will still reach the rest of the peer-to-peer network. So the attack can be detected.
  * Can attacker change the block reward?
    * These rules of the system cannot be changed because attacker cannot control the Bitcoin protocol that the rest of the nodes are running.
  * Can attacker destroy confidence of Bitcoin?
    * Blockchain is no longer decentralized so people won't trust Bitcoin anymore.
* As a block gets deeper in the chain, it gets more difficult to fork the chain from that earlier point. This is because to create a longer chain, you now need to add a lot more blocks and adding blocks requires PoW.
* What else can we do with consensus?

### Week 3 : Mechanics of Bitcoin.
**Bitcoin Transactions**
* Consider an account based ledger :
  * Have to keep track of balance in each participants account.
  * Hard to determine if transaction is valid because you have to look backwards till the beginning of time to check all transactions involving the particular participant who is sending the coins.
  * This is why Bitcoin isn't based on an account based ledger. Instead, based on transaction ledger.
* Transaction based ledger :
  * Given an input, the output value must total the input value. e.g., (Assume 1 transaction in 1 block) In transaction number x1, A has 25 coins. In transaction x1+1, A wants to send B 10 coins. So the input is x1[A], which is 25 coins, and output is that A sends 10 coins to B and resends herself 15 coins (change address), which totals to 25 coins. In transaction x1+2, B wants to send 4 coins to C. So the input is x1+1[B], which is 10 coins, and output is that B sends 4 coins to C and resends 6 coins to himself (change address). which totals 10 coins.
  * Validity is easy now because we only do a finite backward scan.
  * Can do joint payments (with multiple inputs, where each input has 1 signature i.e., owned by 1 person).

**Bitcoin Scripts**
* Transaction output addresses are really scripts. So are the input addresses.
* Bitcoin scripting language :
  * Simple.
  * Support for cryptography.
  * Stack based.
  * Limits on time/memory.
  * No loops.
  * Not turing complete. By design because these scripts are to be run by miners.
  * Output is either error or no error. If error, transaction not accepted into the block.
* 99% of Bitcoin scripts are exactly the same : simple signature check (also called pay to public key script).
* Nodes have a whitelist of scripts that can be run.
* Proof-of-burn script :
  * Write abitrary data into the blockchain by destroying a bit of your currency.
  * Way to destroy Bitcoin and gain an altcoin. 
* Pay-to-script hash :
  * Retailer specifies a script to send Bitcoins to instead of an address.

**Applications of Bitcoin Scripts**
* Escrow transactions : A wants to buy from B but B wants the money first while A wants the goods first.
  * Solution is to use MULTISIG script so that money is released if 2 of 3 people sign the transaction. Involve another person J (judge). In normal exchange of money for goods, A and B can sign the transaction and everything is great. In dispute, J will have to decide whom to support. How do we trust J though?
* Green addresses : A wants to pay B but B is offline and cannot see the blockchain at all.
  * Real world solution is that A pays a bank and bank stores the money in green addresses. Later, the bank sends the money to B.
  * Mt Gox did this so people don't trust this so much anymore in the Bitcoin world.
* Efficient micro payments : A wants to make a lot of small payments to B for using B's service but  does not want to pay a transaction fee for each payment. Can we combine all micro payments to make 1 large payment?
  * Solution is to use MULTISIG. A sends B 1 coin at time t0. But B never signs it, so its not yet published as a transaction. By time t1>t0, A would have sent B 10 coins and decides to stop using B's service. B now signs the last transaction and it is published for acceptance into a block.
  * What happens if B never signs the last transaction? Then the coin is just sitting in escrow forever. Solution is to use lock time. Before the micropayment protocol, both A and B have to sign a transaction that refunds all of A's money back if the micropayments have not been signed by B by the lock time.
* Multiplayer lotteries, hash pre-image challenges, coin swapping protocols are other applications.
* Bitcoin scripts can be used to define smart contracts, but due to the nature of the scripting language, only few types of smart contracts can be defined.

**Bitcoin Blocks**
* Why bundle transactions into blocks?
  * Makes blockchain shorter so easier to verify the chain.
  * Single unit of work for miners.
* Bitcoin block : 
  * Hash chain of blocks and Merkle tree of transactions in each block.
  * The Merkle tree also contains a special transaction called the coinbase transaction. This contains the mining coin reward plus the transaction fees.

**The Bitcoin Network**
* Bitcoin P2P network : 
  * Runs on TCP port 8333.
  * All nodes are equal.
  * New nodes can be added at any time.
  * Forget non-responding nodes after 3 hrs.
* How do new nodes join?
  * New nodes find a seed node (1 node in the network) from a list of seed nodes.
  * Ask the seed node to return all its peers. Can keep repeating this.
  * Choose which ones from all the nodes you know to peer with.
* Whats the P2P network good for?
  * Network maintains the blockchain.
  * Entire network needs to know about new transactions : flooding algo/gossip algo.
    * It's like BFS.
    * Nodes will only relay the transaction if :
      * Transaction/script is valid.
      * Script matches a whitelist. This avoids unusual scripts.
      * Haven't seen the script before. This avoids infinte loop.
      * Doesn't coflict with others seen before (even though the others might also be valid). This avoids double spending.
  * Nodes will have different transaction pools because they disagree on which transactions should be added to the published block. It's like a race condition. Its only when one of the nodes publishes a block that this race condition is resolved. Every miner implements their own logic on which transaction to choose if there is a conflict.
  * New blocks are announced using the same algo as new transactions. New blocks should be added to their copy of the longest chain but there is no restriction preventing them from creating a fork. But that's ok, the protocol can withstand that.
  * Block propagation takes over 30 sec on average. The protocol is not very efficient in terms of propagation time.
* How big is the network? Always changing and hard to estimate. But only 5-10k are permanently connected and fully validating nodes.
* Fully validating node :
  * Permanently connected.
  * Store the entire blockchain (about 20 Gb).
  * Active network connection.
  * Track all the UTXOs (unspent transaction outputs) in RAM (about 1 Gb).
* Simple payment verification node : 
  * Only care about certain transactions e.g., Wallets.
  * Store block headers only, so they can't verify that other transactions in the block are valid.
  * They trust the fully validating nodes.
  * Need only 20 Mb of storage.

**Limitations to the Bitcoin Protocol**
* Hard coded limits :
  * 10 min avg time creation per block.
  * 1 Mb per block.
  * 250 bytes per transaction.
  * 7 transaction per second (2-10k transactions per second for Visa).
  * 1 signature function only ESDSA.
  * Hard coded hash functions.
* Constants can't just be changed. Lot of implications.
* Hard-forking :
  * Involves upgrading the software in every node.
  * But the un-upgraded nodes will not accept blocks sent by the upgraded nodes because those transaction/script operations don't even exist in the old software.
  * Until old nodes upgrade their software, they will keep rejecting new nodes.
  * So the blockchain will split (hard fork) and 2 separate blockchains will form.
  * New hard-fork possibilities : New instructions, changes to size limits, change to mining rate, small bugfixes, etc. But unlikely to happen, use altcoins to solve instead.
* Soft-forking :
  * Idea is that the upgraded software will only make validation tighter.
  * Rules are stricter i.e., they only limit the set of valid scripts.
  * The bad part is that old nodes might mine invalid blocks that will be rejected by the new nodes with the upgraded software.
  * e.g., Pay to script hash was implemented via a soft fork.
  * New soft-fork possibilities : new signature schemes, extra per-block metadata (e.g., put in "coinbase" parameter in a transaction).

### Week 4 : How to Store and Use Bitcoins.
**Simple Local Storage**
* To spend Bitcoin :
  * Info from blockchain.
  * Owner's secret.
  * How to store and manage secret keys?
* Goals :
  * Availability : Can spend anytime.
  * Security.
  * Convenience.
* Simple approach : store secret key in a file.
  * Availability : As available as your device.
  * Security : As secure as your device.
  * Very convenient.
* Wallet software.
  * Keeps track of coins. 
  * Provides nice UI. 
  * Have separate key for each coin, thereby maximizing privacy. Wallet can do this automatically.
* How to convey address to receive Bitcoin?
  * Base58 notation : Encode address in Base58.
  * Use QR code.

**Hot and Cold Storage**
* Hot storage : Online. Its convenient buy risky.
* Cold storage : Offline. Archival but safer.
* Hot storage knows addresses of cold storage. Just sent it to that address. Cold storage does not need to be connected to the network at all!
* Problem : Want to use new address for each coin sent to cold for privacy. But cold wallet is offline.
  * Solution 1 : Cold generates a batch of addresses and sends it to hot side before going offline.
    * Not so good because have to do this periodically.
  * Solution 2 : Hierarchical wallets.
    * Call generateKeyHier function. This creates generator for address. This is send to hot wallet. It also creates generator for keys. This is sent to hot wallet. Now cold wallet can go offline and hot wallet can generate new addresses for each coin by calling the address generator with a new integer input.
* How to store info in cold wallet?
  * Solution 1 : Info in device, device locked in a safe.
  * Solution 2 : Brain wallet i.e., user remembers passphrase.
  * Solution 3 : Paper wallet i.e., print info on paper.
  * Solution 4 : Tamperproof device i.e., device that signs, but won't divulge keys.

**How to Split and Share Keys**
* If we store key in 1 location, there is a single point of failure.
* Secret sharing : Take secret key and divide into N pieces (shares) st if we're given any K pieces, we can reconstruct secret key. If lesser than K, then we cannot learn the secret key.
  * e.g., N=2, K=2 : Generate P large prime. We have to generate secret S=[0, P) and R=[0, P). Split : X1=(S+R) mod P. X2=(S+2R) mod P. Reconstruct : (2X1-X2) mod P=S.
  * Proof presented on why secret sharing works.
  * Good : Adversary needs to discover many pieces.
  * Bad : The shares still need to be combined in order to reconstruct the secret. This is still a single point of failure.
    * Solution is multisig. Use the shares separately to sign without needing to reassemble the secret key.

**Online Wallet and Exchanges**
* Online wallet : Website stores the keys.
  * Convenient.
  * Security worries (Bitgrail, Mt Gox).
* Bank-like services : You deposit coins and can withdraw but Bank can invest their reserves as they see fit.
* On an exchange : No transaction actually gets added to the Blockchain. They only promise that they will give you the coins you own.
  * Convenient.
  * Risky because the exchange only promises to give you the coins you own.
  * Ponzi schemes possible (Bitconnect).
  * Most exchanges fail. The ones that succeed are the ones that handle the most traffic (and hence the most attacks).
* Tranditional banks are govt regulated so depositors have some level of protection.
* Bitcoin exchange can prove that it has fractional reserve (called proof of reserve).
  * Reserve : The exchange creates a transaction to itself. Also, the exchange signs a challenge string with the same private key used to validate the transaction. This proves that bank (or someone who the bank knows) has that amount of reserve.
  * Deposits : Exchange also has to prove how many demand deposits you hold. Exchange uses Merkle tree (whose leaves contains user accounts). Each tree node has total of coins in the subtree. An account owner can use Merkle tree to prove that his or her account was included in the Merkle tree.
  * Fractional reserve = Reserve / Deposits

**Payment Services**
* Customers want to pay with Bitcoins. Merchant wants to receive dollars and don't want to deal with exchange rate risk or tech risk or security risk (of storing Bitcoins).
* A payment service can charge a fee and handle above scenario. It absorbs all of the risks involved in the process.

**Transaction Fees**
* No transaction fee (consensus fee) if :
  * tx is less than 1000 bytes.
  * All outputs are 0.001 or larger.
  * Priority (age of coins + value of coins over tx size) is large enough.
* If you don't pay the transaction fee, your transaction will take longer.
* Some miners may not abide by these rules but most follow them so your transaction will eventually make it into the blockchain.

**Currency Exchange Markets**
* Supply of Bitcoins = Supply of Bitcoins in circulation + Demand deposit in exchange (i.e., exchange has more deposits than they have Bitcoins).
* Demand for Bitcoins = BTC to mediate transactions (transaction mediation demand) + BTC as investment (if market thinks that demand will go up in the future).
* Model for transaction demand :
  * T = Transaction value mediated by BTC ($ per sec).
  * D = Duration that BTC is needed by a transaction (sec). This is because a transaction removes the BTC from the supply temporarily.
  * S = Supply of BTC.
  * P = Price of BTC.
  * S/D = number of coins will become available per sec (after the transaction is completed). This models the supply for BTC.
  * T/P = number of coins needed per sec to serve all the transactions. This models the demand for BTC.
  * When S/D=T/P, then price of BTC is in equilibrium i.e., P = TD/S. So if people hold BTC as investment, S goes down so P goes up. If people want to use BTC for transaction mediation, TD goes up and P goes up.

### Week 5 : Bitcoin Mining.
**Recap**
* Miners :
  * Store and boradcast the blockchain.
  * Validate new transactions.
  * Vote by hash power on consensus.

**Task of Bitcoin Miners**
* How to become a miner :
  * Join the network, listen for new transactions and validate them.
  * Listen for new block and maintain the blockchain. Validate new blocks and blockchain.
  * Assemble a new block.
  * Find the nonce to make your block valid (the really hard part).
  * Hope other nodes accept your blocks.
  * Profit.
* Finding a valid block :
  * Assemble all your transactions from your pending transaction pool.
  * Assemble header that points to previous block.
  * Search for nonce s.t. hash of block header have the required number of zeroes. This is an iterative process.
  * If you go through all nonces and can't find the right hash, then you have to increment an extra nonce value in the coinbase transaction (recall that coinbase is a create coin transaction that pays to yourself) and retry searching for the header nonce all over.

![_config.yml]({{ site.baseurl }}/images/fvb.png)

* 2 ^ 66 is number of tries needed to find the right nonce.
* Mining difficulty set every 2 weeks based on how efficient the miners were in the past 2 weeks. Over time, the mining difficult keeps increasing.
* Average time to find a block is about 10 mins.

**Mining Hardware**
* SHA-256 : 
  * Designed by NSA and isn't broken cryptographically.
  * 265 bits split to 8, 32 bits words.
  * In each round, some of the words undergo bit mainpulation. 
  * Some words are added together mod 32. The new word becomes the first word and the rest of the words shift over to the right.
  * The previous steps are done 80 times.
  * Nonce is run through SHA-256 to get the hash and check if hash is suitable.
* GPU :
  * Nowdays you need GPUs for mining because CPUs are way too slow.
  * Parallel ALUs.
  * Overclocking introduces errors into the SHA-256 process.
    * But it might be ok to live with these errors.
    * "Goodput" : Throughput of SHA-256 * Error Rate.
    * Overclock by 50% with 30% hashing error rate is still ok.
  * Need good motherboard to run multiple GPUs.
  * GPUs draw a lot of power.
  * GPU throughput of computing hashes is 10 times better than CPUs. But still, its really long.
* FPGAs :
  * Higher performance than GPUs.
  * Excellent for bit manipulation.
  * Better cooling.
  * More expensive than GPUs.
  * Needs more expertise.
  * Still takes 25 years with 100 boards to find a block.
* Bitcoin ASICs :
  * Application specific integrated circuit.
  * Need to pre-order and delivery takes time.
  * Require significant expertise.
  * Example is Terraminer IV : 2 THz of hashes, $6k cost, still takes 14 mths to mine a block.
  * Most ASICs get obsolete within 6 mths.
  * Half of the profits made within the first 6 mths.
  * Hence, shipping delays are really costly.
  * Probably cheaper to buy Bitcoin instead of mining.
* Professional mining centers :
  * Need cheap power, good network and cool climate.

**Energy consumption and Ecology**
* Energy aspects of mining :
  * Hardware manufacturing of ASICs.
  * Electricity.
  * Cooling.
* Estimating energy usage (2014) :
  * Top down approach : 25 BTC reward per block mined = $15k. This is $25/s being minted. How much electricity can $25/s buy? Assuming 10c per kWH, this is 900 MW.
  * Bottom up approach :  Network hash rate : 150 million Hz. Estimate is 150 MW.
  * Typical hydro dam : 1000 MW.
  * Coal fired plant : 2000 MW.
  * So we need 1 large power plant to power the Bitcoin network.
* But even in traditional currency, a lot of energy is used.
* Data furnaces : Use Bitcoin mines as a home heater. But gas heaters are 10x more efficient. And who owns the blocks that are produced? What happens when consumers turn off heaters?
* Could we make a currency that doesn't require proof of work?

**Mining Pools**
* Economics of being a smaller miner :
  * ASIC is $6k, and block is mined in about 14 mths. About $1k per mth revenue.
  * A lot of risk because mining is a probabilistic process.
* Mining pool :
  * All participants pool their ASICs.
  * Pool manager will give same block header to all participants.
  * When block is found by a miner, that miner will send all BTC to pool manager.
  * Pool manager distributes revenues based on how much work each miner performed. 
  * Amount of work is determined by "mining shares". These are blocks that are "near valid" (those that start with a lot of zeroes but not 66 needed to make it a valid block). These shares are sent to the pool manager by each miner.
* Mining pool variation :
  * Pay per share : 
    * The pool doesn't need to find a valid block. 
    * If a share is found by a miner, that can be sent to the pool manager and miner can get paid a flat rate per share.
    * This means that the pool manager bears a lot of risk because the block hasn't necessarily been created yet.
    * So pool manager can charge a higher fee.
    * What if miner doesn't send a valid block?
  * Proportional model :
    * Find the valid block first, and only then will the pool manager distribute the reward per share.
    * This is lower risk for the pool manager.
    * Miner has some motivation to publish the valid block.
  * "Luke-jr" approach :
    * Pool owner collects no fee.
    * Miners don't get revenue until they have 1 BTC.
    * Pool owner keeps the spread.
* There are a lot of mining protocols out there.
* 90% of miners mine in pools.
* Pros/cons :
  * Smaller miners can participate and mining is more predictable.
  * But this leads to centralization. Also, number of fully validating nodes will go down.

**Mining Incentives and Strategies**
* Default mining strategies :
  * Which transactions to include? Anything above min transaction fee.
  * Which block to mine on top of? Longest chain.
  * How to choose between colliding blocks? First come first serve.
  * When to announce new blocks? Immediately once found.
* Can we get more revenue if we don't use default strategies.
* Forking attack :
  * Let 0 < alpha < 1 be the amount of mining power you own.
  * If you have a lot of mining power (alpha > 0.5), you can rewrite history. This means you can double spend. Or you can obtain some product from your victim and then not pay them.
  * This attack can be detected.
  * An attacker could even bribe miners so that he gets alpha > 0.5. This would be tragedy of commons where individual miners care about short term profit more than the stability of the currency.
  * A solution to forking attacks are checkpointing.
* Block witholding attack :
  * Don't announce blocks immediately. Accumulate more blocks first.
  * Why do this? As soon as another miner finds a valid block, you publish 2 blocks and every miner will accept your 2 blocks instead. So all other miners just wasted their time.
  * What if you have only 1 block and someone else finds a block? Then you have to immediately publish and hope you win the race to be accepted into the blockchain.
  * If you peer with every node, you will always win the race because you can announce your new block to everyone hopefully before the other miner does.
  * This has not been observed in practice yet.
* Punitive forking :
  * Suppose you want to block all transactions from address X.
  * But if alpha < 0.5, as soon as a chain exists that has a transaction involving address X, and if you refuse to accept that chain, then you're basically mining on an orphan fork and this is a waste of your time.
  * Feather forking strategy : 
    * You will act like before except that you will give up blacklisting X if you get enough confirmations. 
    * For you to remove the last block with X's transaction, you will have to find 2 blocks.
    * If you have alpha = 0.2, then you can publicly announce your blacklist and smaller miners might follow you (as they want to avoid finding blocks that might be overwritten by you).
* As of 2014, transaction fees don't matter much as miners get 99% of revenue from mining rewards. But later on, miners will be making their revenue from the transaction fees. But how will they enforce higher transaction fees? Mining pools?

### Week 6 : Bitcoin and Anonymity.

**Anonymity Basics**
* Bitcoin addresses are public key hashes and not your real identity. This is called pseudonymity and Bitcoin is only pseudonymous.
* Anonymity = Pseudonymity + Unlinkability (as user interacts with system repeatedly, these interactions cannot be linked to each other).
* Reddit has pseudonymity via usernames while 4chan has anonymity.
* Why unlinkability needed?
  * Many Bitcoin services need your real identity.
  * Linked profiles can be deanonymized due to side channels e.g., your transaction times are correlated with when you tweet online.
* Defining unlinkability in Bitcoin :
  * Hard to link different addresses of same user.
  * Hard to link different transactions of same user.
  * Hard to link sender of payment to its recipient e.g., the payment reaches the recipient by some circuituous and indirect means.
* Quantifying anonymity :
  * Complete unlinkability is hard (for addresses and transactions).
  * Anonymity set : The crowd that one can blend into e.g., 1000s of transactions that look just like mine.
     * To calculate set, we need to define adversary model and reason carefully about what adversary knows, and doesn't know.
* Without privacy in Bitcoin, your transaction history is permanently exposed.
* But what about money laundering? This is a legitimate worry.
* Similar dilemma : Tor network, which is an anonymous communication network.
* People have been trying to make anonymous currencies from a long time e.g., Anonymous e-cash.
  * In anonymous e-cash, we have a bank that maintains user accounts and the amount of money they own. Users can transfer money to each other and the bank will not be able to link them at all! This is done by a neat cryptographic trick called blind signatures where the signer creates a signature that he/she does not know.
  * In this system, user A can withdraw money and bank signs and returns a signature to user A that bank doesn't know. User A can send the signature (token) to user B and user B can deposit with bank. Bank will then check a double spend table and if token is not present, then the token can be deposited to B's account. However, bank doesn't know who sent B the token.
  * So you can trust the bank with keeping your money but not with anonymity.
  * Developed by David Chaum.
* Anonymity and decentralization are in conflict with each other.
  * Interative protocols for anonymity are hard to decentralize e.g., blind signatures require the bank as the third part.
  * Decentralization usually requires public traceability (like blockchain) in order to prevent things like double spend but this runs contrary to anonymity.

**How to de-anonymize Bitcoin**
* Application layer de-anonymization :
  * Shared spending is evidence of joint control. e.g., Alice uses her wallet software to combine 5 BTC and 3 BTC in 1 transaction and then buy a teapot that is worth 8 BTC. This tells attacker that both input addresses are in control of some user.
  * Addresses can also be linked transitively. e.g., Everytime Alice has a cluster of addresses (shared inputs) that have been linked, and she creates a new transaction that combines one of those addresses with a new addresses, this new address can be added to the cluster.
  * Change addresses can also be detected with heuristics (change addresses are addresses where the change is sent ater you buy a product that is lesser than the input).
  * Miekeljohn et Al. published a paper where they labelled clusters for major service providers (like Mt Gox and Satoshi dice) using a Bitcoin transaction graph.
  * If Miekeljohn et Al. notice that your cluster is within a labelled cluster (in the transaction graph), they can subpoena the service provider and get your real identity.
* Network layer de-anonymization : 
  * The first node to inform you of a transaction is probably the source of it.
  * There is a good system, called Tor, to provide network layer anonymization. But Tor is optimized for low latency (web browsing) so it compromises anonymity sometimes. 
  * Since Bitcoin has high latency, perhaps we can develop a more fine-tuned anonymity network that doesn't have Tor's shortcomings. 
  * We can build a mixnet solution to provide anonymity in the Bitcoin network. But Tor is pretty widely used and analysed so its best to just use Tor for network anonymity.

**Mixing**
* This section discusses solutions to inhibit transaction graph analysis.
* To protect anonymity, use an intermediary. 
  * Online wallets.
    * Reputable but regulated.
    * Require identity so you have no anonymity wrt wallet service.
  * Dedicated mixing services.
    * Promise to delete all records and don't require your identity.
    * How does it work? Mixer asks you for a recipient address and then sends you an address to send the coins to so that the coins eventually get routed to the recipient address.
* Principles of mixes :
  * Use a series of mixes.
  * All mix transactions must have the same value (chunk value).
  * Client side must be automated.
  * Mixing fee : Mixes should randomly swallow an entire chunk but most of the time return the chunk.
* Current mixes don't follow all the above principles.
* How to trust mixes?
  * Stay in business and build up reputation.
  * Users can test for themselves. If chunk size is very small then users can test the mix themselves.
  * Cryptographic "warranties". Mix provides some warranty to guarantee safe movement of funds otherwise users can publicize this warranty and others won't use the mix.

**Decentralized Mixing**
* Why decentralized mixing?
  * No bootstrapping problem. Find community of peers who all want to do mixing.
  * Theft is impossible. Enforced by technical meeans.
  * Possibly better anonymity.
  * More philosophically aligned with Bitcoin.
* Decentralized mixing : Coinjoin.
  1. Find peers who want to mix.
  2. Exchange input and output addresses.
  3. One of the peers constructs the transaction.
  4. Send it around and collect signature from each peer. Each peer checks if its output is present.
  5. Broadcast the transaction.
* Problems with Coinjoin :
  * How to find peers?
    * Solution : Use an untrusted server to find peers.
  * Peers know your input-output mapping. 
    * We have no idea who the peers are. Could be a single attacker with Sybil nodes.
    * Strawman solution : Peers exchange input addresses, then disconnect and reconnect over Tor and finally exchange output addresses.
    * Better solution : Decryption mixnets.
  * Denial of service. 
    * One of the nodes never signs the transaction. 
    * One of the nodes spends its transaction outside of the coinjoin so that the joined transaction will look like a double spend and be rejected.
    * Solutions : 
      * Proof of work.
      * Proof of burn aka Fidelity bonds in Bitcoin. You burn some cash in order to participate.
      * Server kicks out malicious participants.
      * Cryptographic "blame" protocol.
* High level flows could be identifying. e.g., A receives 43.12325 BTC per week as income and regularly saves 5% of that to retirement account. These are uniquely specific values and there is also a timing pattern.
  * Solution : Merge avoidance. Instead of merging inputs for payments, why not have a protocol so that the receiver can provide multiple output addresses? This way, sender can make many transactions to send from different input addresses to different output addresses.

**Zerocoin and Zerocash**
* Built into the Bitcoin protocol to solve anonymity problems but they're not backward compatible.
* Zerocoin developed at Johns Hopkins. Zerocash is an improved version of Zerocoin.
* Zerocoin : Protocol level mixing. Cryptographic guarantee of mixing. You don't need to rely on anybody.
* Basecoin : Bitcoin-like altcoin. They can be converted into Zerocoin and back. This breaks link between original Basecoin and the new Basecoin.
* Zerocoins :
  * Zerocoin is a cryptographic proof that you owned a Basecoin and made it unspendeable.
  * Miners can verify proofs.
  * Gives you right to redeem new Basecoin.
* Challenges :
  * How to construct proofs? Zero knowledge proofs (a way to prove statements without revealing any other info). 
  * How to ensure that each proof can only be spent once?
* Minting a Zerocoin :
  * Anyone can create Zerocoin using "commitment".
  * "Commitment" : Create a serial number S and a random private number r. Then compute H(S,r), which is the "commitment".
  * Next, you put the "commitment" into the blockchain, and you have gained 1 Zerocoin!
  * Putting the "commitment" into the blockchain is done via a mint transaction. To create a mint transaction, you have to put 1 Basecoin as input, sending it to the "commitment" (not to a recipient public address).
* Spending a Zerocoin : 
  * You have to reveal the serial number S. Miners will check if S has been seen before.
  * Next, you will create a zero knowledge proof. "I know a number r st H(S,r) is a Zerocoin in the blockchain." This doesn't reveal the random number r. So miners now know that you own a Zerocoin.
  * Now you have the right to redeem a Basecoin that you had spent earlier to create the Zerocoin.
  * Pick any arbitrary Zerocoin in the blockchain and use it as input to a new transaction, out of which comes a new Basecoin.
  * You have anonymity because you didn't reveal random number r, even if you revealed S. Beacuse nobody knows which "commitment" i.e., Zerocoin, i.e., H(S,r) is actually yours.
* Zero knowledge proofs are quite efficient but less efficient than Bitcoin transactions.
* Zerocash : 
  * All transactions are Zerocoin. No need Basecoins. 
  * Since all transactions use Zerocoins i.e, H(S,r), transaction amounts are untraceable. 
  * Ledger only records the existance of these transactions, not the amounts.
  * Side channel attacks that can be carried out on Zercoin are no longer true for Zerocash.
  * The catch :
    * Setup of the system is different.
    * Need S and r to generate the Zercoins and these are very large in size.
    * These secret inputs must be securely destroyed.

![_config.yml]({{ site.baseurl }}/images/btc_anon.png)

**Tor and the Silk Road**
* Anonymity in communication network refers to unlinkability between the sender and receiver.
* Tor :
  * Route messages between sender and receiver. If at least 1 routing node is honest, then the communication is safe-ish (Safe-ish because it might still be possible to determine that A and B are communicating if the attacker controls incoming and outgoing requests into and out of the network by using timestamps of the network activity).
  * How to hide routing information to nodes? Layered encryption, resembling an onion.
    * Each router has a public key.
    * If A wants to send message, she has to pick a path of routers.
    * Then A can execute a protocol to each router's public key and obtain symmetric key for each router.
    * Then if A wants to send message to B, she encrypts the message using each symmetric key from each router (according to the path she picked).
    * Then A sends the encrypted message to the first router in the path.
    * The router then "peels" the first layer of encryption from the message and gets the IP of the next router and an encryption message.
    * This keeps happening until the last router is reached, where B's IP is revealed with the actual message. The problem here is that B will eventually receive the unencrypted message from the last router node (can be solved with HTTPS).
    * So anonymity is achieved because each of the router nodes' only know the previous nodes' IP and the next nodes' IP, breaking the link between A and B.
* Silk road : What if the server wants to hide its address?
  1. Server connects to a rendezvous point (a Tor node) through Tor.
  2. It publishes the mapping between its IP and the rendezvous point via Tor's directory service (known as onion addresses, not DNS).
  3. Client connects to rendezvous point using the Tor browser by inputting the onion addresses.
* Anonymity is morally ambiguous but also important to protect.

### Week 7 : Community, Politics, and Regulation.
**Consensus in Bitcoin**
* Consensus about rules :
  * What makes a transaction valid.
  * What makes a block valid.
  * How P2P nodes should behave.
  * Protocols and formats.
* Consensus about history : Agree on contents of the blockchain.
* Consensus on value : Agree that coins are valuable.

**Bitcoin Core Software**
* Most widely used software.
* Defines the de-facto rules in Bitcoin.
* Bitcoin Improvement Proposal : proposal for improvements to Bitcoin includes tech specs and rationale.
* If users don't like a new rule, they can fork the rules. There will be a fork in the blockchain if there is a (hard) fork in the rules.
* If fork was meant as altcoin, then both branches coexist peacefully.
* If fork was a disagreement over the rules, then the branches will fight for market share (i.e., want more merchants to accept it and people to buy it).

**Stakeholers: Who's in Charge?**
* In a negotiation about rule setting, who controls the outcome?
* Do Bitcoin core devs have the power? They control the rules.
* Do miners have the power? They write to the blockchain.
* Do investors have the power? In case of hard fork, they control which fork to extend.
* Do merchants and customers have the power? They generate the primary demand for Bitcoin. Investors are just guessing where the merchants and customers will go in the future.
* Do payment services have the power? They handle all the transactions for the merchants and customers.
* Bitcoin foundation : Pays the devs and talks to govt as the voice of Bitcoin.
  * Some people don't really like that people in suits are representing Bitcoin to the govt.
  * Who put these people in charge?
* All stakeholders are in charge.

**Roots of Bitcoin**
* Libertarianism : Govt should not interfere.
* Cypherpunk movement : Use crypto to protect individual interests without govt interference.
* Satoshi Nakamoto : Nobody knows who this person(s) is.
* I noticed that they didn't mention the 2008 financial crisis.

**Governments Notice Bitcoin**
* Untraceable digital cash (if it exists) defeats capital controls. Govt can't control the flow of the currency inside and outside the country.
* Untraceable digital cash also enables crimes like money laundering, kidnapping, extortion and sale of illegal items.
* Govts try to disconnect BTC from their local fiat currency.
* Lessons from the fall of Silk Road :
  * Hard to keep real and virtual world apart.
  * Hard to stay anonymous for a long time.
  * Feds can "follow the money" i.e., Feds were watching specific addresses that were held by Ross Ulbricht and any attempt to convert BTC to fiat would've been a traceable event.

**Anti Money-Laundering**
* Goal of AML : Stop large amounts of money from (1) crossing borders or (2) moving between underground to legitimate economy without detection.
* Know your customer (KYC) rules : 
  * Require businesses to identify and authenticate who their clients are and tie their activities to people in the real world.
  * Evaluate risk of client.
  * Watch for anomalous behaviour.
* Mandatory reporting in the U.S. :
  * Any transaction over $10k has to be reported.
  * Structuring transactions (i.e., avoiding $10k transactions by breaking it into multiple transactions) must also be reported.
* Govts take AML rules very seriously. Bitcoin businesses have been shut down due to this. Business people have been arrested.

**Regulation**
* Argument for regulation : Markets don't give you the result that you like and regulations can help here.
* Two types of market failures are discussed (1) Lemons market and (2) Price fixing.
* Lemons market :
  * For example, if you have a market with 2 types of used cars, 1 is low quality (LQ) and 1 is high quality (HQ) but costs a bit more. Assume the cars look similar. 
  * Consumers can't tell the difference between these two types of used cars. So consumer's won't pay extra for the HQ used car while sellers won't produce HQ cars. 
  * Now market is stuck in equilibrium where LQ cars are produced and consumers are not very happy. This is worse for buyers. Also worse for producers because they make less money selling LQ cars. 
  * This is market failure called asymmetric information failure, called the lemons market (LQ cars being the lemons).
* Fixing lemons market :
  * Market based approach :
    * Sellers reputation : 
      * Consumers believe a seller who tells the truth about HQ and LQ products. 
      * But it takes a while for producers to build the reputation. 
      * Also, the seller may decide to become dishonest later.
      * Doesn't work in businesses where there are one-off transactions.
    * Warranties :
      * Seller provides warranties for buyer if they buy a product.
      * But the warranty is another type of product. e.g., Seller may not honor the warranty.
  * Regulations :
    * Require products to be labelled and penalize producers for lying.
    * Minimum quality standards with enforcement.
    * Required warranties with enforcement.
* Price fixing :
  * Sellers raise prices and agree not to compete with each other e.g., bread price fixing by Loblaws.
  * These are illegals in most jurisdictions, part of "antitrust" or "competition" law.

**New York's BitLicense Proposal**
* As of July 2014, this was a proposal but as of August 2015, its law. So you need a BitLicense to operate in NY.
* If you're a NY business, you need a BitLicense if you're :
  * Receiving virtual currency.
  * Storing virtual currency (wallet services, exchanges).
  * Trading virtual currency as a business.
  * Providing retail conversion services (exchanging fiat to virtual currency or between virtual currency).
  * Issuing or controlling a virtual currency.
* You can apply for the license by participating in KYC and paying an application fee.
* Licensees must :
  * File reports.
  * Maintain financial reserve.
  * Follow rules on : Custody of consumer assets, AML, recordkeeping, cybersecurity and disaster recovery.
  * Designate a compliance officer and have written policies.
  * Disclose risks to consumers.
* As of May 2018, only 5 BitLicenses have been awarded.