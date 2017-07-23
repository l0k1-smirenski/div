> |![](https://s17.postimg.org/waef6bisv/div_logo.svg.png) | <h1>DIV</h1>
> | --- | ---
> |     | Monetising Governance, Infrastructure and Content for Decentralised Distributed Social Networks
> |     | Loki Verloren, July 2017

Abstract
===
DIV is a multi-tier Byzantine Fault Tolerant Database system designed to engage users, via monetisation incentives, in every aspect of the system, from open self-governance of code, curation, content creation, content hosting, search services, archiving, validation, query services, short and long messaging and extensible in the future to expand to include classified advertising, cryptocurrency exchange, custom databases from very large to small and extreme low latency for multiplayer gaming, privacy services, and whatever the community that grows out of it decides it wants to add.

Contents
===
[TOC]

Introduction
===

Since the appearance of Steem in the 'Blockchain' space, there has also been a number of related and competing systems emerging such as Yours.Network, Synereo, LBRY and most reecently Ark. The goal of these systems is to stimulate the engagement of users in the production of content and monetising the content according to the subjective evaluation of peers.

Steem, the first, and by far most successful of these platforms, fails to deliver in some critical areas promised in its White Paper, specifically, in flattening the distribution curve of tokens in order to facilitate the rapid growth of userbase (due to lack of effective subdivision of content into affinity groups and lack of search engines), in the lack of monetisation of database services, in its failure to create a fluid and adaptive governance system, and in so many ways, yet, however, there is no doubt that the "Inflation Tax" model of how to incentivise users to join the network by distributing a portion to both curator/creators, and to the validators, known as Witnesses in Steem jargon.

DIV is primarily aiming to supersede Steem, by omitting its most egregious flaws, and adding the elements that it needs in order to be both scalable and elastic, by monetising all relevant infrastructure, including the service of web based interfaces, search engines, content hosting, and hosting of the codebase and documentation in a monetisable structure that eliminates the inertia of Steem's governance system and the Prisoner's dilemma in the Witness election, and in the forum voting system.

A key guiding principle is that there should be no outside party to the userbase, no early advantage beyond simple early adoption, and that the community operates its own governance mechanisms, including code development, moderation and policing against abusive behaviour such as plagiarism, trolling and racketeering, to facilitate the development of 'Departments' in which organisational structures can be formed to facilitate coordinated, many-party activities related to funding and promoting the platform.

Overall Architecture of DIV
===

DIV splits roles into multiple tiers with differing levels of frequency of activity, versus comprehensiveness of storage in the database. 

Database Services
---

* **Validators** have the most time critical role in rapidly assessing, confirming and propagating new transactions, they are the Write Side of the back end for clients.
* **Replicators** distribute the newest data and maintain 3 months of data, and are paid to respond to queries to provide results for user's client applications to cover the great majority of activity on the backend on the Read Side.
* **Archivists** specialise in storing a complete historical archive and answering queries passed on by Replicators when the query falls outside of the 3 month period. This is much less frequent, but per operation far more expensive, as is the infrastructure required.

> By breaking the levels of service into these three tiers, latency can be minimised where it is critical, and where completeness is required, it can be provided for, at significantly higher latency, where the client needs complete data rather than fast answers..

Application Services
---

This includes the six initial and essential databases systems for DIV:

1. [The Registry](#the-registry)
2. [Refractory Token Ledger](#refratory-token-ledger)
3. [The Forum](#the-forum)
4. [Messaging](#messaging)
5. [Database Code Repository](#database-code-repository)
6. [Interface Code Repository](#interface-code-repository)

A key deficiency of Steem is the lack of accessibility and participation in governance, of users to the Database and Interface codebases, the complete lack of an integrated short and long form, transient messaging system, it only implements a Forum, and it does it within a large monolithic database, where the voting system for the forum.

All of these data stores exist within distinct database instances within the node, which minimises the amount of indexing overhead required for each. Merkle trees are used where the data in one database relates to another, such as the Registry being referred to by all others (this is the user registry), and the Forum and Code Repositories are referred to by the ledger when the votes stored in these databases results in a claim for a payout from the pool of new tokens.

With the exception of Messaging, these 6 components are governed and regulated by the Database Services. The messaging system has a more fluid architecture which is peer to peer and does not include monetisation, but instead simply has rules about transaction rates per accounts, propagation patterns, caching patterns, that are designed to optimise for latency and minimising on redundancy beyond necessary to ensure successful delivery of messages.

The Registry
===
At the centre of all cryptographically secured, distributed database systems, are cryptographic accounts. The registry is a simple table containing usernames, and the public keys that prove authority of the user to perform operations using the account.

Linked External Accounts
---

The Registry also will have the infrastructure in protocol and datatypes enabled to enable, in the future, automated control verifications for external, non-cryptographically secured accounts, such as Facebook, Reddit, Twitter, and others, as well as the more direct and easier to verify cryptographic accounts, whose addition and amendments by account holders can be directly signed using the public keys of the cryptographic accounts.

###Mandatory Proof of Control

This proof of ownership is necessary in order to prevent the spammy registration of many attached accounts, as the cost of proving ownership eliminates the ability to arbitrarily claim to control an arbitrary number of accounts that the user does not in fact have any control of.

The Refractory Token Ledger
===
Instead of using the traditional double entry ledger accounting system, the primary currency in DIV functions more like a property registry. There is multiple reasons why this has been chosen, the main one being that the growth of the database is highly predictable, because the number of tokens only expands up to a maximum level, but it does not expand without activity driving it, and no more than 3-5 past transactions are required for 100% confidence a double spend is not occurring, whereas splitting double ledger tokens endlessly expand in size, unrelated to the rate of issuance, but driven by activity.

When tokens are issued, the amount issued is split into progressive half denominations 8 times, to produce a payment that can pay to the precision of 1/256th of the total amount in the payout. In general this will mean that change will not often be required, however, it will be required, and this is one of the purposes of having the deposit contract.

Activity Driven Issuance
---
There is in fact no true economic justification for issuance of new tokens in any monetary system at all. Whether supply is growing, shrinking, or unchanging, makes no difference in the long run, but in the short term, zero effective new tokens makes economic calculation simpler, because it is unchanging against the background of every other commodity in the economy. Increasing supply benefits the recipients of new tokens, decreasing it benefits the holders of deposits. The RTL will apply both depending on the demand for services.

However, in order to use issuance as a way to monetise especially public infrastructure, the price of these services is hidden in the background of a slowly declining value, steadily increasing supply of tokens. Holders of the tokens are implicitly funding the network as shown in the decline in the value of their assets.

> Thus the subject of the 'correct rate of issue' is a constant source of debate for monetarists, who argue over entirely arbitrary ratios, usually percentages of compounding, an flat rates.

The Refractory Token Ledger has a built-in mechanism by which a rate of issuance at the baseline can be pegged to the relative rate at which the tokens are issued - because of the need to change the larger tokens into smaller ones, an ongoing background task of reshuffling large liquid tokens with smaller, locked deposit tokens, allows a mechanism for paying interest to holders of deposits, which tends to track the total amount of liquidity needed to facilitate ongoing trade, at the same time as directly, randomly, rewarding depositors.

It will be explained later in the section about content and hosting monetisation, how the use of a moving average will regulate the issuance of new tokens to fund the content production incentivisation also, in a way that does not create an artificial flat issuance rate, but rather one that drops the relative share issued during busy times, and raises it during quiet times, acting as a contrary influence against attempts to game the issuance system and the inevitable 'peak hour' and 'graveyard shift' that results. 

Like Off-Peak tarrifs for energy and telecoms services, the value in busy times is decreased, and increased during quiet times, encouraging a smoother bandwidth utilisation in the network. The price will naturally equilibriate because when liquidity rises, the supply rises, when liquidity falls, the supply falls. All else being equal, this will mean that excess activity will 'punish' by a drop off in value and insufficient activity will incentivise an increase in activity via a rise in value of the liquid tokens.

Liquid Tokens and Stake Contracts
---
As exists in the Steem Ledger, except contracted to only include one of each type, and no hedging contract (as it is a design error to place this in the ledger). This also simplifies the process of calculating rewards, as Steem requires a base currency called VESTS, to implement both Steem and Steem Backed Dollars, and Steem is used to implement the Steem Power contract.

The liquid token is called DIV Coins or DIVC as a ticker, and the Illiquid Vote Power instrument is called DIV Stakes or DIVS. Coins can be transferred freely to any other account at zero cost (as the cost of running the network is accounted for in the payments to the Validators). Steem has therefore in fact 2 different currencies, whereas DIV will only have one, which greatly simplifies computation of rewards for the various network functions.

Stake liquidity Restrictions
---
In order to restrict the rate at which users can reverse their staking of Coins into Stake contracts, deposit is instant, but withdrawal can only take place at a rate of 1% per day, paid daily.

This also eliminates the advantage that may be gained by users seeking to draw down when everyone else is buying in. Liquidity is very important in marketplaces, and daily payments on ledgers is standard in the banking industry. Weekly payouts cause a cyclic price fluctuation that will flatten in the case of a daily payout, due to trade opportunities being able to be seized by any holder of liquid tokens.

Change Operations to pay Interest on Stake
---
Since it may sometimes happen that a user's collection of tokens does not have exact change to allow a transaction, instead, Validators preemptively break the larger tokens in smaller accounts with the smaller tokens of larger accounts. It is very simple to establish how to redistribute the different sized tokens, because if there is one in each progressive power of two denomination, any payment can be made. 

As tokens automatically exchanged between accounts in order to ensure a minimum capability to spend any amount that can be denominated in the total and down to the smallest token in circulation require an available pool that is not in circulation, it is part of the purpose of DIVS to enable this, and the amount of a change operation, divided by 100, is paid in new tokens to holders whose stake allows this maintenance of adequate change for any amount of payment without the user needing to concern themselves with how the Refractory token mechanism, while providing a reason to pay interest to stake, and determining how much it needs to be done (driven by the amount of money being transferred around.

Note that these operations are not just randomly chosen, stake in accounts to be used to provide change, must have no incoming or outgoing user initiated transfers between each other, so users can't deliberately spend in order to win interest payments. On average every user will end up earning about the same thing over time, but in the short term the variance can be very wide. However, the bigger one's DIVS pool, the more likely to get a payment, rewarding the use of stake.

The providers of change also are selected by a strict criteria that there can be zero transfers, or votes in the various content monetisation databases, between the change provider and the recipient. The change operation is marked as enacted as one of the delegated responsibilities of Validators, and thus via the Validator Election process, a proper chain of authority is created. These operations do not count as business between two accounts.

There is inevitably accounts which never make any interactions with each other, and by choosing such distant nodes in the financial network graph, users can be assured that this interest payment mechanism cannot be gamed with spammy, intentionally change requiring operations. Instead, after an account makes a payment, the Validator examines their pool of DIVC to ensure that it has at least 1 of every denomination from smallest to largest, and if it finds holes in the supply, it makes a change operation that is invisible to the account needing the change, and appears as an interest payment of 1% of the reqired change quantity, to the DIVS of the change provider. 