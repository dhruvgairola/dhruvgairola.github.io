---
layout: post
title: Cryptocurrency Speculation
type: blog
---

My earliest recollection in the cryptocurrency space was a discussion I had with a colleague in 2012, who had described Bitcoin as an anonymous means of buying shady goods and services online (Bitcoin is not truly anonymous). Almost 6 years later, as the public interest in cryptocurrency burgeoned, I wondered what had changed. Popular media had always been critical, associating cryptocurrency with anarchy (Silk Road) and fraud (Mt Gox). All of a sudden, the average joe wanted to "invest" in Bitcoin. My aged uncle cornered me at a family party, wanting to chat about Bitcoin. A slick banker friend was excited about blockchain. It was at this point that I decided to educate myself, reading research papers and diving deep (the cryptocurrency course offered by Princeton University is excellent). 

As I conducted my research, I couldn't help but feel a tinge of sympathy for the early adopters who were now forced into a new ecosystem replete with "get-rich-quick" types, ignorant crypto-bros, scammers and despised third-parties like banks. To be fair, the early adopters weren't solely driven by a libertarian ideology. Many simply wanted to buy drugs online. In any case, many of the new entrants in the ecosystem got seriously burned when the cryptocurrency market crashed in late 2018. This seemed inevitable since there was no real basis for investing in cryptocurrency for the vast majority. As the markets crashed, I was nearing the end of my foundational education in this space. I both enjoyed reading about the nitty gritty details while also felt the urge to explore opportunities to either build products or invest in promising coins.

In this post, I want to evaluate opportunities mostly from an investment/speculation perspective i.e., if I had $100, which currencies would I buy and why? My examination is based on factors such as : the team/organization developing the currency, the use-cases or pain points addressed, the market impact in solving these pain points, the competitive advantage offered by the technology, the riskiness of the investment and the timing of the investment.

# Background

If you have some familiarity with how cryptocurrencies work, you can skip this section.
{:.hint}

I'll provide a simplified overview of how blockchains and cryptocurrencies work using Bitcoin as an example. The story begins with nodes/computers/devices which decide to install a piece of software. This software allows nodes to communicate with each other, forming a network. The software also enforces rules within the network. There are different types of nodes within this network. Fully validating nodes store the entire history of transactions that will take place by participants in the network. This ledger is called the blockchain beacause the ledger is organized as a series of blocks where each block contains many transactions. Only 1 node has the right to create a block and this right is obtained by being the first to solve a difficult, but useless, mathematical puzzle. The node which solves the puzzle first gets a mining reward (newly minted cryptocurrency). This node creates the block by adding the mining reward transaction (to itself) and also other transactions that this node "heard about" between other nodes in the network (also charging transaction fees). This node then adds the block to its own blockchain and then publishes the block to other nodes. All honest nodes have no choice but to accept this new block, as per the rules of the software that they are running. A new puzzle is generated and all mining nodes compete again to create new blocks to the blockchain. Since the blockchain is public (i.e., anyone can join the network and download the ledger), cryptocurrency cannot be spent twice.

The blockchain ledger doesn't just store transactions; it can also store smart contracts. Smart contracts are programs that are executed when a set of conditions are met. When two or more nodes agree to participate in a smart contract transaction, and if the conditions of the contract are met, then tokens are exchanged between the parties. These tokens are usually cryptocurrencies which the software accepts in its network. The software accepts these tokens as long as they conform to some standard (e.g., ERC-20) and provided that they are also developed using a prescribed software development platform. The Ethereum software for example, allows a variety of tokens developed by third-parties to be accepted for use within its network in addition to its own native currency ETH. This also means that fully validating nodes which store the Ethereum blockchain have to record all transactions involving the various tokens, not just ETH. Smart contracts are created using a programming language. The Ethereum platform offers Solidity, a turing-complete language, while Bitcoin also offers a scripting language, which is a stack-based language with limited functionality. In Ethereum's case, arbitrarity complex programs (smart contracts) can be added to the blockchain and each smart contract transaction is carried out using the computing power of the the fully validating nodes. Hence, transaction fees paid by participants of an Ethereum based smart contract factors in the time it takes to execute this smart contract (gas cost) and the price per unit of time (gas price). These transaction fees are paid in ETH, no matter the tokens being exchanged.

# Payments
## Direct to Merchants/P2P

Reading /r/cryptocurrency or cryptocurrency blogs, it seems like many readers are excited about purchasing goods online or in-store using cryptocurrency. However, it is not clear why paying with cryptocurrency would be preferable over cash or credit (leaving aside countries with hyperinflation). As a consumer, I would use cryptocurrency only if it were cheaper and easier to buy goods and services and if I also had existing conveniences like being able to access credit, reverse transactions, perform chargebacks, recover my account credentials, etc. However, for me to even access cryptocurency, I would have to either mine the currency or buy it over an exchange, which is expensive and tedious. For the vast majority, even understanding cryptocurrency is challenging, let alone accessing it or loading it into a credit card (e.g., Wirex). Managing your private keys is not intuitive for consumers.  Merchants would also have to manage volatile cryptocurrency by converting the various payments into stablecoins. Crucially, the anonymity benefits of cryptocurrency are at complete odds with regulation. Widespread adoption seems unlikely without solutions to many of these difficult problems, and even then, competing or existing solutions might outperform cryptocurrency based solutions.

### Thought Experiment

Perhaps consumers would be more willing to use cryptocurrency daily if they had the same level of convenience that most banks offer. Consumers would open an account at our fictional bank, Acme bank. This bank would manage credentials in a centralized manner so that customers wouldn't have to manage private keys. In order to obtain Acme coins, customers would deposit fiat into their accounts, and these would be automatically be converted into Acme stablecoins (e.g., Dai) by a smart contract. These Acme coins would be available for use immediately in laptop and mobile wallets. For customers, online payments become easy using these wallets, and in-store payments can be performed via NFC phones or an Acme credit card (e.g., Wirex). The various payments-middlemen would be replaced by smart contracts. Acme ATMs would allow customers to withdraw fiat by selling Acme stablecoins. For transfers from an Acme account to an account in another bank or country, or for remittance, Acme stablecoins could be used for settlement between banks. Transactions between accounts would be held in escrow (e.g., Ethereum Safe Remote Purchase) for longer if the transactions are deemed risky (e.g., cross border transactions) by machine learning algorithms, and these transactions could be cancelled by customers if needed. Acme bank would have to be compliant with the financial laws in the countries it operates. This also means that Acme bank could freeze your funds if deemed necessary. Acme bank would offer interest to customers, similar to how holding coins accumulates dividends. For all intents and purposes, Acme would function like a regular bank, except that many of its operations would be performed on the blockchain.

## Indirect via Intermediaries

Instead of paying for goods and services using cryptocurrency in a peer-to-peer setting, a more realistic scenario is for cryptocurrency to "play nice" with existing financial institutions. Already, banks are exploring blockchain based technology and even issuing their own "cryptocurrencies" (e.g., Goldman Sachs) to "improve various business operations". While this is probably more marketing speak than actual advancement, we should consider how cryptocurrency could be applied to improve payments in the B2B setting. Typically, the payments ecosystem consists of :
  1. Issuers : These are the organizations that issue credit cards to their customers (e.g., banks, credit unions, retailers).
  2. Online and physical payment gateways : Online gateways provide APIs that your websites can use for online payments (e.g., Stripe, Paypal). Physical gateways refer to point-of-sale systems that accept credit cards (e.g., Square POS, Moneris POS).
  3. Payment processors : These processors move payments from issuers to merchant banks. Processors are usually banks (called merchant/acquiring banks) but can also be ISOs which are third-parties contracted by banks for payment processing (e.g., Stripe, Square, Moneris).
  4. Payment networks : These allow communication between banks within the network (e.g., Visa). Processors rely on this network in order to process payments and perform settlement.

![_config.yml]({{ site.baseurl }}/images/plaid.png)

The diagram above shows how a payment flows through the ecosystem [1] (similar diagrams can be found in [2][3]). In this system, merchants are charged around 3% by the third-parties. Of the 3%, majority of the fees are paid to the issuing bank, and smaller fees are paid to the acquirers and the payment network providers. The settlement between the acquirer and issuer takes a few days. For remittance, a $500 billion market [4], these fees can reach 7%, and settlement is even longer [5]. For international payments, there is an additional issue where banks have to hold foreign currency in foreign accounts to mitigate exchange rate risk. This has an opportunity cost because the foreign currency is sitting idle in those foreign accounts. Can cryptocurrency solve some of these payment-related problems?

### Ripple
A company called Ripple promises solutions for speedier settlements via one of its products, called xRapid.

> xRapid : "International payments, especially in emerging markets, require businesses to hold pre-funded accounts in destination currencies. It’s a costly endeavor that ties up resources. RippleNet provides an alternative. Customers have the option to source liquidity required for cross-border payments with XRP and enable instant payments to and from any country in the world." [6]

The idea is for banks to buy XRP coins when they need it and send the XRP to other banks in the Ripple network for cross-border settlements. However, banks would not actually want to hold XRP coins for too long because XRP is very volatile and banks, being heavily regulated, cannot take such a high risk. If banks don't buy and hold XRP coins, then its unclear how regular folk (non-traders) could profit by holding XRP long term. Similar points can be made against competitors like Stellar (Stellar has applications outside of the financial industry though).

Ripple also has products like xCurrent and xVia, which allow the various players in the ecosystem to move money more efficiently and for cheaper. However, xCurrent and xVia do not use XRP. Thus, investing in Ripple (the company) makes more sense than buying the XRP, but that is a separate discussion involving comparisons with encumbent competitors like SWIFT. 

Another argument against Ripple is that most of the supply of XRP is held by the founders. There are policies in place to prevent these whales from manipulating the price of XRP but I'd still be wary placing a big bet on XRP. Useful articles about Ripple can be found online [7][8].

## Internet of Things (IoT)

One of the issues with using the blockchain as a public ledger is that transaction fees are prohibitively large for micro-payments (because high fees incentivize nodes to spend resources to create new blocks). In the age of self-driving cars, wearables, drones, robots, smart homes, etc, micro-payments will become increasingly important. Hence, researchers have developed alternatives to the blockchain which confer lower transaction fees and promise increased scalability. These newer public ledgers are organized as Directed Acyclic Graphs (DAGs).

### IOTA

IOTA is a cryptocurrency targeted at solving problems in the IoT space. Transactions are stored on a public ledger called the Tangle, which is a DAG. When a node publishes a transaction to the Tangle, it must approve two other transactions. Additionally, a cryptographic puzzle (PoW) has to be solved by this node as a means of spam prevention. This puzzle is far far easier to solve compared to the puzzle involved in Bitcoin. Due to this, transaction fees are negligible and lesser energy is wasted. Theoretically, the Tangle can also scale far better than a traditional blockchain because multiple nodes can quickly add transactions to the Tangle in parallel while on the blockchain, miners have to compete in order to gain the right to add transactions. The figure below illustrates this [9].

![_config.yml]({{ site.baseurl }}/images/dag.png)

IOTA's current network involves special nodes called coordinators which are controlled by the IOTA foundation. The purpose of these nodes is to prevent attacks on the network. However, the drawback is that the network is highly dependent on the existence of these nodes, making the network centralized. Long term, the IOTA foundation has promised to remove these coordinators from the network and ensure a decentralized design.

The IOTA foundation also periodically publishes snapshots of Tangle ledger. Nodes rely on these snapshots to avoid having to store the entire ledger (which is massive) from the beginning of time. However, this is another point of centralization in the network. To solve this, IOTA plans to incentivize nodes to store the entire history of the Tangle [10].

#### Thoughts

The current use cases of IOTA have yet to achieve widespread adoption. For example, a data marketplace was developed by the IOTA foundation where it is possible to use IOTA tokens to publish data from various IoT sensors. Other use case include autonomous vehicles which can earn money, vehicles which can pay tolls, tracking the movement of products in the supply chain, etc [11][12]. However, none of these use-cases are production-ready, despite the IOTA foundation's impressive list of industry partners.

Many of the above challenges arise due to the laundry list of difficult technical challenges which have yet to be solved by the IOTA foundation. For one, it's not clear if they will succeed in removing coordinators and snapshots anytime soon and achieve proper decentralization. In addition, their PoW puzzle is too difficult to solve for many of the weaker IoT devices. If these devices cannot publish transactions, they will not be able to join the IOTA network. Finally, the IOTA foundation has also promised to build a smart contract layer, which is going to involve signifiant development effort.

# Platforms
## Prelude

Platforms like Ethereum have unlocked human creativity on the blockchain. Developers have implemented various tokens and smart contracts on the Ethereum platform. The basic building block (pardon the pun) of the blockchain, smart contracts, can enforce contracts programmatically thus invalidating the need for trusted middlemen. When pretty web or mobile interfaces are layered on top of smart contracts to create apps, these become dApps or distributed applications (e.g., Cryptokitties). With dApps, it's typically the case that only a part of the app logic is stored and executed on the blockchain while the rest is stored in private/public repositories and executed in traditional web servers (usually UI facing logic). However, the entirety of the app logic can also be stored and executed on the blockchain. For example, DAOs (decentralized autonomous organization) are special types of dApps where the app is "an online company". In a DAO, tokens are distributed to company shareholders and these shareholders can then vote on proposals. The governance and voting rules are completely embedded in the blockchain.

Before examining specific use cases, we must note how the various tokens on a platform and the native currency interact. Using Ethereum as an example, when a company issues new tokens, buyers use ETH coins to purchase them. When smart contracts are executed, the transaction fees are also paid in ETH irrespective of the token. So if a token is wildy successful, there might be a mild impact on the price of ETH via these direct interactions. However, the price correlation beteween ETH and a token is a lot more complex, so for simplicity's sake it's probably better to examine ETH independently from a token.

## Stablecoins

Stablecoins provide a solution for the high volatility of cryptocurrency. These stablecoins are cyptocurrencies which are pegged to a stable asset like USD. There are 2 general flavors of stablecoins. IOU stablecoins are those which are backed by fiat and managed by centralized organizations. However, trusting these organizations can be a significant challenge. In the case of Tether, its tokens are backed by fiat (allegedly/unaudited) but fiat reserves are loaned out to Bitfinex, a disreputable exchange owned by the same parent company [13]. Until banks start issuing their own stablecoins someday, it's probably best to avoid IOU stablecoins. The far more interesting stablecoins are crypto-stablecoins which are backed by cryptocurrencies. These crypto-stablecoins and are managed by DAOs entirely on the blockchain and are mostly decentralized.

### MakerDAO

MakerDAO is a DAO which manages its own stablecoin called Dai, where 1 Dai is pegged to US$1 [14]. Dai can be bought on cryptocurrency exchanges just like any other cryptocurrency. However, an increase in demand for Dai will cause its price to rise. To keep the Dai price pegged to $1, MakerDAO would like to increase the supply of Dai to counteract the increase in demand. Supply is increased when new coins are minted, and new coins are minted when users send MakerDAO a collateral (in exchange for new coins). MakerDAO encourages users to send a collateral by lowering interest/fees on the collateral, allowing users to borrow Dai cheaply from MakerDAO.

#### Borrowing creates new Dai

Everytime a user borrows 1 Dai from MakerDAO, the supply of Dai increases by 1 because MakerDAO mints a new coin for the user. To buy 1 Dai, a user would typically have to pay $2 worth of ETH as a collateral. This is a parallel to the real world, where collaterals are usually worth more than the borrowed amount, especially risky collaterals. Borrowing 1 Dai creates 1 Dai debt in the MakerDAO universe. As mentioned in the previous paragraph, interest is charged on this debt, and controlling the interest rate controls the willingness of users to borrow Dai, which in turn controls the supply of Dai.

This debt has to be paid one way or another. The happy path happens when the user voluntarily repays 1 Dai debt plus interest/fees and gets the $2 ETH collateral back. This debt can be paid using the very same 1 Dai which was borrowed. The unhappy path happens when MakerDAO decides to auction off the collateral, charging a hefty fee on the proceeds, then clearing off the 1 Dai debt and returning whatever is left back to the user (a very tiny amount). Since the 1 Dai debt was forcibly paid off using the collateral, the 1 Dai that the user initially borrowed has no debt counterpart. The most typical scenario for forced liquidation of collateral is when the price of the backing cryptocurrency crashes.

#### Borrowing risks

It is entirely possible that the $2 ETH that the user put up as collateral may crash to $1.50 ETH. This increases risk for MakerDAO because the value of the collateral is falling rapidly while the 1 Dai debt remains unpaid. Hence, MakerDAO has to cover the debt and sell off the risky collateral. MakerDAO creates a new type of token called MKR and sells MKR to investors to raise the 1 Dai to cover the debt (called a debt auction). MKR represents voting power in MakerDAO, allowing holders to control interest rates, risk parameters, etc. In addition, MKR is also a store of value and buying MKR is how MakerDAO distributes profits to shareholders. After the debt auction, the price of MKR is lowered since MKR supply was increased to raise funds to cover the debt. MakerDAO will then auction the $1.50 ETH collateral to investors, raising 1.5 Dai (called a collateral auction). Finally, MakerDAO will buy 1 Dai worth of MKR (to compensate for the debt it paid off) plus 0.2 Dai worth of MKR which represents a 13% liquidation penalty plus around 0.1 Dai worth of MKR which represents interest/fees of 6% (these fees are variable). Now, only 0.2 Dai is left (1.5 - 1 - 0.2 - 0.1 = 0.2) and this is returned to the user as $0.20 ETH. In the end, the user is holding 1 Dai from before and $0.20 ETH. More examples can be found online [15]. Note that if ETH spikes instead, nothing would happen. 

#### Other risks

MakerDAO obtains the latest USD price of ETH tokens from various oracles, which are external sources of information outside the blockchain. This price information is used to peg the Dai against USD so MKR shareholders have to ensure that these oracles are accurate. In the event of serious security breaches (e.g., oracles are hacked), MKR holders trigger an emergency shutdown of MakerDAO and all price feeds are fixed. Users can now repay their debts and get all their collateral back (or whats left of it in case of a serious hack).

#### Thoughts

MakerDAO claims "merchant receipts" as a use-case in its whitepaper [16]. For example, merchants would be more willing to accept and hold stablecoins due to a lower risk compared to cryptocurrencies. However, there is nothing preventing merchants from simply converting their holdings to fiat immediately, nullifying the need for stablecoins. For P2P payments (discussed in the "Payments, Direct to Merchants/P2P" section), consumers would also have to acquire these coins in the first place which is a significant hurdle for non-cryptocurrency folk. However, for B2B payments (discussed in the "Payments, Indirect via Intermediaries" section), stablecoins could encourage businesses and financial institutions to hold and store tokens for longer periods while reaping the cross-border benefits of stable cryptocurrencies.

MakerDAO also claims "​Hedging,​ ​Derivatives,​ ​Leverage" as a potential benefit [16]. This would mostly be restricted to sophisticated traders since I can't imagine any crypto-layman attempting to seriously borrow/repay Dai as leverage. These traders play an important role in helping Dai maintain its peg to USD. Borrowing Dai causes an increase in the supply of Dai to match any increase in the demand for Dai (which happens when the price of ETH increases). Conversely, repaying Dai causes a decrease in the supply of Dai to match any decrease in the demand for Dai (which happens when the price of ETH decreases). For MKR shareholders, interest/fees on loans is great for business. Forced liquidation is also great for MKR holders due to the large liquidation penalty (unless the liquidation is due to a black swan event).

It is possible to use Dai without understanding any of the complexity of MakerDAO but it is not possible to borrow and repay Dai or take part in debt and collateral auctions without a deep understanding of MakerDAO. Similarly, investing in MKR requires a high amount of sophistication and ideally, some amount of participation in the governance of MakerDAO.

## Non-Fungible Tokens (NFTs)

Crypto-kitties popularized the notion of non-fungibility in the digital world. Crypto-kitties is a dApp where users can purchase or breed digital cats online from https://www.cryptokitties.co/. These digital cats are represented as images on the website. To buy cats, users send ETH tokens to a smart contract and receive a CK token in return. Each token has metadata attached to it which indicates the "genes" of a cat. Rarer or desirable genes influence the price of these cats. These genes ensure the uniqueness of a CK token, which makes these tokens non-fungible.

![_config.yml]({{ site.baseurl }}/images/cryptokitties.jpg)

Unlike traditional digital assets (like online videos, images), CK tokens are verifably scarce. Simply having an image of a cat doesn't prove that a user owns a cat. However, owning a CK token would prove that a user owns a cat. This is because tokens are stored on the Ethereum blockchain, and hence, cannot be copied.

Admittedly crypto-kitties may not be the best showcase for NFTs since the utility of owning digital cats is marginal at best. Moreover, the value of owning a CK token is due to the image of a cat. The mapping of a CK token to a cat image is controlled by a centralized website, and if this website goes out of business, CK tokens lose all their value. This is different from owning physical collectibles like baseball cards which can exist independently of the issuing organization. 

It is important to note that crypto-collectibles are initial attempts at utilizing NFTs and that better use-cases could gain popularity in the future [17]. For example, distributing event tickets on the blockchain can enable P2P ticket sales, eliminating fees charged by ticket issuing companies while also solving ticketing fraud [18]. However, these applications are in very early stages.

## Other Use-cases

There exist plenty of coins with a diverse set of use cases but not all of them are investable in my opinion. Some of my rejections were based on having too small a market (e.g., Aragon, most dApps), being in the R&D stage (e.g., StorJ), tough competition (e.g., BAT), a lack of personal interest (e.g., Augur, gambling dApps), associated with scams (e.g., ICOs), private cloud products (e.g., IBM Hyperledger, Amazon Managed Blockchain), private blockchains (e.g., Facebook, Goldman Sachs, Kik), etc.

# My Recommendation

The cryptocurrency ecosystem is mostly unregulated and is a hotbed for fraudulent exchanges (Mt Gox, Quadriga, BitGrail), exchanges/coins with fake volumes (there are a ton), pyramid schemes (Bitconnect, Centratech, most ICOs) and wallet-related scams (e.g., iotaseed.io). Moreover, cryptocurrencies can also contain bugs in their protocols [19]. Smart contracts are especially susceptible.

It is important not to invest more than you can afford to lose. Additionally, basic knowledge on how to securely store cryptocurrency would minimize risk from being burned by scummy exchanges. Basic research can also help. For example, understanding that the blockchain is not the same as a currency, or that **increased adoption of a coin doesn't guarantee an increase in price (and vice versa).** Keeping up to date with the latest news is also recommended.

However, if you had $100 to gamble away and wanted to invest in cryptocurrencies for profit, I would recommend putting the majority of that into Ethereum ($60), a portion into Bitcoin ($20) and a smaller chunk into Maker ($10). The remaining $10 could go anywhere as you please, but I would personally pick Stellar ($5) and Ripple ($5). The best moment to withdraw these investments would be when major news outlets restart the hype on cryptocurrencies or when regular Joes want to get in on the action.

Ethereum is great because it enables developers to build a vast array of applications on its platform and since all fees are paid in ETH, there is some justification for an investment. Bitcoin is the next logical investment, not necessarily because of usefulness but rather because the entire cryptocurrency market follows the price movement of BTC. A smaller investment in Maker is supported by my belief that stablecoins are crucial to any widespread adoption. It would be best to make an investment into Maker only after MakerDAO implements the ability to accept multiple types of collateral, not just ETH. Stellar and Ripple are solving problems in the B2B space where I believe cryptocurrency can have the highest impact in the short term. However, it would be prudent to only allocate a small amount of investment in these since they're largely centralized systems.

Coins which I considered but rejected include Nano and Litecoin (I'm not bullish on P2P payments), IOTA and Cardano (research projects), Monero and Zcash (amazing tech but cannot be regulated), NEO (centralized) and Tron and EOS (general shadyness).

# Conclusion

The average Joe isn't going to be using cryptocurrency for day-to-day payments anytime soon. It's too complicated, volatile, fundamentally difficult to manage and challenging to regulate. Any attempts to solve these problems will involve a heavy amount of centralization, defeating the whole point of decentralized cryptocurrencies. Centralized financial systems already work really well for most consumers outside of countries with hyperinflation. Even from a technological viewpoint, centralized databases can easily outshine centralized blockchains or DAGs in terms of performance and scalability.

In the B2B payments space, the complexity of managing cryptocurrency is abstracted away from regular folk. Slow and ancient banking software can be disrupted by blockchain-based software. While it is true that database based software could probably do just as well or even better, blockchain has an edge in terms of marketing/hype. However, cryptocurrencies play a secondary role in these blockchain-based banking systems. Still, a small investment is possible, much to the chagrin of libertarians.

Platforms like Ethereum enable the development of a multitude of applications, both consumer and business facing. The success of these applications have a minor, but tangible, spillover effect on the price of the native tokens of these platforms since application transaction fees are paid in the native token. Thus, a large array of applications mean that the likelihood of an increase in the price of the native token is higher than non-platform cryptocurrencies (the magnitude of this increase is a separate question). We have already examined promising applications like MakerDAO which were recently developed on Ethereum. Whether Ethereum is going to lead the pack is up for debate since the platform faces it's own challenges, like scalability.

In the grand scheme of things, will cryptocurrencies ever cross the chasm and achieve mainstream success? Macro trends like AI-based software and machines indicate that the world will become even more digital. It seems that decentralized digital currencies could very well succeed in the long term. Perhaps twenty years from now, our kids having grown up in a digital world might even be ready for digital currencies. Until then, the rest of us can try our very best to understand, develop and build upon existing ideas. And as we advance boldly into the future, perhaps some of us might even see some profit along the way!

# References

1. https://fin.plaid.com/articles/major-players-in-payment-processing
2. https://www.mastercard.us/en-us/merchants/start-accepting/payment-process.html
3. https://qr.ae/TUfmNO
4. https://www.worldbank.org/en/news/press-release/2018/04/23/record-high-remittances-to-low-and-middle-income-countries-in-2017
5. Blockchain Revolution (pg 48), Tapscott et. Al.
6. https://ripple.com/ripplenet/on-demand-liquidity/
7. https://hackernoon.com/4-alarming-reasons-ripple-might-not-be-what-you-think-9debc3c86985
8. https://www.reddit.com/r/CryptoCurrency/comments/ayiuww/14_common_misunderstandings_about_ripple_and_xrp/
9. https://blog.iota.org/on-the-tangle-white-papers-proofs-airplanes-and-local-modifiers-44683aff8fea
10. https://medium.com/konfid-io-blockchain-reports/iota-report-decoding-the-tangle-part-1-a7705c458583
11. https://blog.iota.org/modelling-new-business-models-with-iota-fadd53c6a192
12. https://www.youtube.com/watch?v=Gr-LstcDcAw
13. https://www.reddit.com/r/CryptoCurrency/comments/b0wmuo/tether_once_again_pulls_a_sneaky_update/
14. https://www.reddit.com/r/CryptoCurrency/comments/b15hrp/with_the_news_tether_isnt_gasp_backed_11_with_usd/
15. https://www.reddit.com/r/MakerDAO/comments/8efk5q/faq_possibly_everything_you_ever_wanted_to_know/
16. https://makerdao.com/da/whitepaper
17. https://www.forbes.com/sites/andrewrossow/2018/08/08/with-blockchain-technology-you-gotta-collect-them-all/#77cb5e891b70
18. https://guts.tickets/
19. https://www.reddit.com/r/CryptoCurrency/comments/avj06n/
