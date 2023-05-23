---
title: 'Orbiter: A Secure, Fast, Low Gas Decentralized Cross Rollup Bridge Protocol'
author: Orbiter Finance
fontsize: 9pt
geometry: margin=1.5cm
date: \textit{Pre-release, \today}
abstract: |
	In this paper, we propose a optimistic interoperability , secure, low-gas, low-latency decentralized architectures for transfer of standard assets between rollup Layer2. consolidating the security through ZK-SNAKRS.
urlcolor: cyan
bibliography: yellowpaper.bib
classoption:
    - twocolumn
    # - onecloumn
header-includes:
    - \usepackage{amsmath}
    - \usepackage{fancyhdr}
    - \usepackage{graphicx}
    - \usepackage{supertabular}
    - \usepackage{booktabs}
    - \usepackage{array}
    - \usepackage{tabularx}
    - \pagestyle{plain}
    - \fancyhead[RE,LO]{}
    - \fancyhead[LE,Ro]{}
    - \fancyhead[CO,CE]{}
    - \fancyfoot[CO,CE]{}
    - \fancyfoot[LE,RO]{\thepage}
---

# PREVIOUS WORK

Vitalik Buterin had proposed a easy decentralizd cross-layer-2 bridge[@vbeasyl2brdige].

Hop[@hopwhitepaper] is a previous cross rollup bridge.

Nomad is a previous optimistic interoperability cross-chain protocol.


# DESIGN PRINCINPLES
Orbiter Protocol's design follows the flowing principles:

- **Secure.** The priority of security is the highest, mainly around the security of funds involved in the roles in the protocol layer.
- **Decentralized.** The assets pledged by the role will not be seized by any role in the agreement, and any evil behavior will be punished by the mechanism of the protocol layer.
- **Efficiency Sensitive.** Efficiency of capital utilization and gas fees for users in the process of cross-chain execution.

# OVERVIEW

Orbiter aims to build a secure, decentralized and efficiency sensitive cross rollup bridge on Ethereum ecology, and raises the interoperability of the standard assets among these rollup environments. These requirements will dictate the following properties:

- Every transaction that the user interacts with the protocol must be a simple transfer transaction, ETH transaction or ERC20 transfer transaction, rather than interacting with a certain contract of the protocol. The user's cross-chain intention is reflected in the series of numbers at the end of the transfer amount. The rules and constraints of this series of numbers are controlled by the protocol layer, which is Orbiter's smart contract.

- After receiving the user's cross-chain intention, the cross-chain market maker needs to respond to the intention, that is, perform the corresponding operation on the target chain.
- Whether a user or a cross-chain market maker sends a transaction that does not follow the protocol, that is, a malicious behavior, both the challenger and the challenged can prove the legitimacy of their own transactions in an efficient and low-cost way through zero-knowledge proof.

![Cross Rollup Transaction Flow](diagrams/cross-rollup-flow.png)

# THE ORBITER PROTOCOL

## Role Defination 

- **User.** User who uses the cross-environment interoperability systems.
- **Maker.** Provider who provide the cross-rollup service.
- **Dealer.** Getting incentived by providing a decentralized frontend.
- **Submitter.** The role used to submit the root of the Dealer revenue tree.
- **Orbiter DAO.** Is to provide a transparent and dencentrailzed framework for managing and operating the project.

## Orbiter Transactions

The user sends a specific amount of ERC20 or native token to the maker, the amount of witch corresponds to the following parts

- **Transfer Amounts**: the amount that the user would have needed to cross chain; formally $\mathrm{D_{ta}}$.
- **Trading Fee**: fees paid to the platform and Maker that is charged as a percentage of the transfer amount; formally $\mathrm{D_{tf}}$.
- **Witholding Fee**: the fee prepaid to Maker to pay the gas fee for the destination network transfer; formally $\mathrm{D_{wf}}$.
- **Destination Chain ID**: the target rollup network sepcified by the user,$\mathrm{D_{dci}}$, which is maintained uniformly and globally on L1 by the orbiter protocol, $\mathcal{C}$.
- **Dealer ID**: formally $\mathrm{D_{di}}$.
- **EBC ID** : formally $\mathrm{D_{ei}}$.

The source transaction execution time can be verified in source block chain, $\mathrm{TIMESTAMP_{S}}$.

\begin{align*}
\begin{gathered}
    \dot{T}_{S} \equiv (T_{S}, \mathrm{D}, \mathrm{TIMESTAMP_{S}}, \dot{M}) \\
    \equiv \mathrm{D_{ta}} + \mathrm{D_{tf}} + \mathrm{D_{wf}} + \mathrm{D_{dci}} + \mathrm{D_{di}} + \mathrm{D_{ei}} \to \mathrm{D} \ \ \land \\
    \mathrm{M_{min}} \leq \mathrm{D_{ta}} \leq \mathrm{M_{max}} \ \ \land \\ 
    \mathrm{D_{dci}} \in \mathcal{C_{M}}  
\end{gathered}
\end{align*}

the maker who response for the user's requirement, the following is its corresponding constraints

- **Maker Address**: formally $\mathrm{M_{A}}$. for all of the address of the selected maker, formally $\mathcal{M_{A}}$.
- **Supporting Chain List**: formally $\mathcal{C_{M}}$.
- **Supporting Token List**: formally $\mathcal{T_{M}}$.
- **Repay Time**: formally $\mathrm{REPAYTIME}$.
- **Transfer Amount Range**: formally $\mathrm{M_{min}}$, $\mathrm{M_{max}}$.

It can be formally defined as :

\begin{align*} 
\begin{gathered}
    \dot{M} \equiv M(\mathrm{M_{A}},\mathrm{M_{min}},\mathrm{M_{max}},\mathrm{REPAYTIME},\mathcal{C_{M}}, \mathcal{T_{M}} ) \ \ \land \\
    \mathrm{M_{A}} \in \mathcal{M_{A}} \ \ \land \\
    \mathcal{C_{M}} \subset \mathcal{C} \ \ \land \\
    \mathcal{T_{M}} \subset \mathcal{T}
\end{gathered}
\end{align*}

![User Transfer Amount Structure](./diagrams/amount-structure.png)

Destination transaction, when Maker perceives that the user sends a transaction to the maker address according to the agreed protocol, it needs to transfer a specific amount to the user's address in the target network within the specified time range, $\mathrm{REPAYTIME}$.

It can be formally defined as 

\begin{align*} 
\begin{gathered}
    \dot{T}_{D} \equiv  (T_{D},\mathrm{REPAYTIME}) \\
    \equiv D_{ta} \ \ \land \\
    \mathrm{TIMESTAMP_{D}} - \mathrm{TIMESTAMP_{S}} \leq \mathrm{REPAYTIME}  
\end{gathered}
\end{align*}



source chain transaction, $T_{S}$, target chain transaction, $T_{D}$

Formally, we can refer to a cross rollup transaction as $\dot{T}$:

$$ \tag{1} \dot{T} \equiv (\dot{T}_{S}, \dot{T}_{D}, \dot{M}) \equiv  (T_{S}, T_{D}, M) $$

## CORE CONTRACT 

All smart contracts below are deployed on the Ethereum mainnet:

- **MDC**: Maker Deposit Contract, keeping Makers's margin, handling the arbitration for user.

- **EBC**: Event Binding Contract, storing the margin rules and Makers' charging standards.

- **ZK-SPV**: Zero Knowledge Simple Payment Verification. Prove the existence and rationality of Orbiter cross-chain Tx through zero-knowledge proof technology. Existence means that both source transaction and target transaction can be proved on L1 that they actually happened on the corresponding L2, and rationality means It can prove the intention of the user of SrcTx, and the result of the maker’s payment in DstTx conforms to specific rules.
- **FeeManager**: Maintain the information of all Dealers, manage and update the benefits that Dealers get from Makers, and ensure the correctness of revenue status updates through the arbitration penalty mechanism.
- **DaoManager**: Maintain some global parameters of the protocol layer.

## Off Chain Compoment

- Maker Client
- Submitter Client

## SECURITY MODEL

Orbiter protocol aims to solve the cross-rollup problems instead of the cross-chain issues. The cross-chain project's primary goal is to ensure the security of transactions between two unique chains and avoid the 51% attack. But the cross-rollup project uses the same Ethereum data layer with each rollup which can naturally prevent the 51% attack. Based on this, Orbiter comes up with a cross-rollup mechanism that can inherit the security of Ethereum L2[@rollupbridgesecurity].

# ZK-SPV

**SPV**. Simplified Payment Verification, firstly proposed in the BitCoin's whitepaper[@bitcoinwhitepaper]. It allows a transaction recipient to prove that the sender has control of the source funds of the payment they are offering without downloading the full Blockchain, by utilising the properties of Merkle proofs[@bitcoinwikikspv].

**zk-SNARKS**. Zero-Knowledge Succinct Non-Interactive Argument of Knowledge, which is widely used in the blockchain community for its features of privacy and scaling. We currently only talk about the latter.

zk-SNARKS consists of the following cryptographic primitives:

- $\mathrm{Setup}(1^{\lambda}) \rightarrow \mathrm{Params}$. Given the security paramater $\lambda$, the mapping $e:\mathrm{G_1} \times \mathrm{G_2} \rightarrow \mathrm{G_T}$ is a nondegenerate bilinear pairing. $\mathrm{G_1}, \mathrm{G_2}$ are cyclic groups in prime order $p$, and their generators are $\mathrm{g_1}, \mathrm{g_2}$ respectively, $\mathrm{Params} = (p, \mathrm{G_1}, \mathrm{G_2}, \mathrm{G_T}, \mathrm{g_1}, \mathrm{g_2}, e)$.
- $\mathrm{KeyGen}(\mathrm{Params}, \mathrm{C}) \rightarrow (\mathcal{K}_p, \mathcal{K}_v)$. Given the arithmetic circuit $\mathrm{C}$, convert $\mathrm{C}$ into a polynomial relationship $\mathrm{R}_c$, and then generate the proving key $\mathrm{K}_p$ and verification key $\mathrm{K}_v$.
- $\mathrm{ProofComputation}(\mathrm{K}_p, x, w) \rightarrow \pi$. Given the proving key $\mathrm{K}_p$, the public commitment $x$, and the secret witness $w$, generate a zk proof $\pi$ about $x$ and $w$ through the circuit $\mathrm{C}$.
- $\mathrm{Verification}(\mathrm{K}_v, x, \pi) \rightarrow \mathrm{rlt}$. Given the verification key $\mathrm{K}_v$, public commitment $x$, and zk proof $\pi$, output binary bit $\mathrm{rlt}$; $\mathrm{rlt} = 1$ when the proof is legitimate; otherwise $\mathrm{rlt} = 0$.

**Recrusive ZKP**

**Basic Concept**. There are two types of rollups: optimistic rollups and zk-rollups. Their implementations are quite different, which also leads to their spv implementations are also very different[@spv_on_eth_l2].


Use ZK-SNARK cryptography technology to reduce the gas consumed by the proof of transaction validity

## Prove Primitives

The challenger should provide the ZK Proof of the source transaction, for the exsitence of that.

**Challenge Task**. The challenger should create the challenge task through smart contract firstly , locking the margin of the maker which is challenged , the creating time is $t_{0}$, the challenger should deposit some amount of margin, formally $\mathcal{M}_{C}$ formally

$$
\mathcal{T} \equiv (T_{S}, t_{0})
$$

**Proof Generation**. The proof computation function $\hat{C}$ for user's intention with address $a$: $\mathrm{I_{a}}$ is defined as:
\begin{align*} 
\begin{gathered}
    p^{z1}(\mathrm{I_{a}}) \equiv \hat{C}(T_{S}, \mathrm{TIMESTAMP_{S}}, \mathrm{D}, \mathrm{M_{A}}, \dot{M}, \mathcal{K}_{z1})
\end{gathered}
\end{align*}

$\mathcal{K}_{z1}$ is the proving key of the circuit.

**Proof Verification**. To verify a validity proof $p^{z1}$ genenrated by user $a$'s intention, the verification time is $t_{1}$:
\begin{align*} 
\begin{gathered}
    v^{z1}(\mathrm{I_{a}}) \equiv (\hat{V}(p^{z1}(\mathrm{I_{a}}),a,\mathcal{K}_{v1}), t_{1})
\end{gathered}
\end{align*}

$v^{z1}$ is the result of verification of a proof ,which would be stored on Ethereum. $\mathcal{K}_{v1}$ is the verification key of the circuit.

$$
\dot{\mathrm{SPV}}(\mathrm{I_{a}}) \equiv \mathrm{SPV}((p,v)^{z1}(\mathrm{I_{a}}))
$$

If the maker has reponsed to the user $a$'s intention, $\mathrm{R_{a}}$, then the target transaction's proof computation can be formalized as:
\begin{align*} 
\begin{gathered}
    p^{z1}(\mathrm{R_{a}}) \equiv \hat{C}(T_{D}, \mathrm{TIMESTAMP_{D}}, \mathrm{D_{ta}}, \mathrm{M_{A}}, \dot{M}, \mathcal{K}_{z1})
\end{gathered}
\end{align*}
The corresponded verification is, it's time is $t_{2}$:
\begin{align*} 
\begin{gathered}
    v^{z1}(\mathrm{R_{a}}) \equiv (\hat{V}(p^{z1}(\mathrm{R_{a}}),a,\mathcal{K}_{v}), t_{2})
\end{gathered}
\end{align*}

The mechanism of challenge can be formalized as:
\begin{align*} 
\begin{gathered}
    \mathcal{C}_{hallenge}(\mathrm{I_{a}}) \equiv \dot{\mathrm{SPV}}(\mathrm{I_{a}}) \land \dot{\mathrm{SPV}}(\mathrm{R_{a}})
\end{gathered}
\end{align*}

The winner of the challenge is defined as $\mathcal{W}$

The final adjudication can be formalized as:

\begin{equation}
    \mathcal{A}_{djudication}(\mathrm{I_{a}}) \equiv \left\{
    \begin{aligned} 
\mathcal{W} \to \mathcal{C}_{hallenger} \\
\mathcal{W} \to \mathcal{M}_{aker}
    \end{aligned}
    \right.
\end{equation}

# DEALER FEE

In many decentralized projects, when some front-end projects are subject to censorship and offline, resulting in users unable to directly use the decentralized product.

**Motivation.** We designed a set of incentive mechanisms to allow third-party organizations to deploy front-ends that support the Orbiter cross-chain bridge protocol.

Dealer use the dealer id to identify itself, and register its fee rate to the fee manager contract.

## Submitter Consensus Election

Nodes need to pledge part of the funds first to meet the basic requirements of becoming a Submitter

## Transaction Mapping

We accurately map cross-chain transactions to L1 blocks through a set of rules, which must satisfies sequentially unique property.
The withdraw time of source chain and target chain is $\mathrm{WT_{S}}$, $\mathrm{WT_{D}}$, the mapping block time can be defined as:
\begin{align*} 
\begin{gathered}
     f(\dot{T}) = \{\mathrm{TIMESTAMP_{S}} + \mathrm{WT_{S}}, \mathrm{TIMESTAMP_{D}} + \mathrm{WT_{D}}\}_{max} \\ 
    + 1DAY
\end{gathered}
\end{align*}

All transactions meeting this condition are guaranteed to be finalized. The Block of L1 with its block number $n$, noted as $\mathrm{B}$, its correspoding block number, $\mathrm{N_{block}}$, and there is a timestamp of this block, noted as $\mathrm{TIMESTAMP_{B}}$, there exesits a constraint
$$
\mathrm{TIMESTAMP_{B_{n}}} \leq f(\dot{T}) < \mathrm{TIMESTAMP_{B_{n+1}}}
$$

Then several cross transaction can be mapped to a specific block on L1
$$
\dot{\mathrm{BT}} \equiv \mathrm{BT_{(n,m)}} \equiv \{\dot{T_1},\dot{T_2},\ldots,\dot{T_m}\} \xrightarrow[]{map} \mathrm{B_n} 
$$


## Profit Tree

We use a Merkle tree to represent the income status of all dealers.

The key is hashed from Address, $\mathrm{A}$, Chain Id, $\mathrm{C}$, Token Address, $\mathrm{T_{A}}$:
$$
\mathrm{KEY}(\mathrm{A}) = \mathrm{HASH}(\mathrm{A},\mathrm{C}, \mathrm{T_{A}})
$$

Its amount is $\mathrm{Amt}$, the value is:
$$
\mathrm{VALUE}(\mathrm{A}) = \mathrm{HASH}(\mathrm{KEY}(\mathrm{A}), \mathrm{Amt})
$$

_State Transition_, formally $\mathrm{ST}$:
$$
\dot{\mathrm{ST}} \equiv \mathrm{ST_{(n,n+1)}} \equiv \mathrm{S_{n}} \xrightarrow[\{\dot{T_1},\dot{T_2},\ldots,\dot{T_m}\}]{\mathrm{B_{n+1}}, \mathrm{BT_{(n+1,m)}}} \mathrm{S_{n+1}} 
$$

_State Transition Tree Root_, formally $\mathrm{STR}$, $s$, $e$ represents Start, End:
\begin{align*} 
\begin{gathered}
    \dot{\mathrm{STR}} \equiv \mathrm{STR}(\mathrm{s},\mathrm{e}) \to \{(\mathrm{B_{s}}, \mathrm{S_{s}}), (\mathrm{B_{s+1}}, \mathrm{S_{s+1}}),\ldots, (\mathrm{B_{e}}, \mathrm{S_{e}}) \} \\
    \land \{ \mathrm{ST_{(s-1,s)}}, \mathrm{ST_{(s,s+1)}},\ldots,\mathrm{ST_{(e-1,e)}} \}
\end{gathered}
\end{align*}

## Submit Profit Tree

\begin{align*} 
\begin{gathered}
    \mathcal{S}_{ubmit}(\mathrm{B_{s}}, \mathrm{B_{e}}, \mathrm{STR_{(s,e)}}, \mathrm{S_{e}})  
\end{gathered}
\end{align*}

The submitter finally update state of the balance of all dealers to the Fee Manager Contract.

## Challenge Protocol

After submitter submits the update status, it will be reserved for one hour to time as an open challenge time. During this time period, anyone can challenge the submitter.

### Phase 1: Challenge initiation

The Challenger initiates the challenge process from the Fee Manager contract and pledges a certain margin, at which point the Submitter's margin is also locked and needs to respond to the challenge process

### Phase 2: Bisection on Blocks

Submitter submits the middle block number and status. The $\mathrm{Pair_{m_1}}$ can be verified by merkel proof.
\begin{align*} 
\begin{gathered}
    \mathrm{Pair_{m_1}} \equiv (\mathrm{B_{m_1}}, \mathrm{S_{m_1}})  \\ 
    \ \ \land \ m_1=\lfloor (s+e)/2 \rfloor \\ 
    \ \ \land \ \hat{\mathrm{V}}(\mathrm{Pair_{m_1}}, \mathrm{STR_{(s,e)}})
\end{gathered}
\end{align*}

Challenger needs to respond to $\mathrm{Pair_{m_1}}$. Now there are two cases:

- If Challenger aggress with Submitter's $\mathrm{Pair_{m_1}}$, the protocol has identified a smaller dispute, $\mathrm{Pair_{m_2}}, m_2= \lfloor (m_1+e)/2 \rfloor$. 
- If Challenger disagrees with Submitter's $\mathrm{Pair_{m_1}}$, the protocol has identified a smaller dispute, $\mathrm{Pair_{m_{2^{\prime}}}}, m_{2^{\prime}} = \lfloor (s+m_1)/2 \rfloor$.

Submitter and Challenger will finally $\mathrm{Pair_f}$ at most $\lceil log_{2}^{e-s} \rceil$ rounds.

### Phase 3: Two-steps Proof

At this time, the Submitter needs to prove the integrity of the $\mathrm{BT_{(f,m)}}$, as well as the proof of the continuous existence of the Pair, $\mathcal{K}_{z2}$ is the proving key of the circuit:
\begin{align*} 
\begin{gathered}
    p^{z2}(f,m) \equiv \hat{C}(\{ \dot{T_1},\ldots,\dot{T_m}\}, \mathrm{B_f},\mathcal{K}_{z2}) \equiv \\ 
    \mathrm{BT}_{(f,m)} \land \mathrm{ST}_{(f-1, f)}  \land \mathrm{ST}_{(f,f+1)}
\end{gathered}
\end{align*}

the proof of $\mathrm{BT_{(f,m)}}$ should be verified in a specific time on L1, $\mathcal{K}_{v2}$ is the verification key of the circuit:
\begin{align*} 
\begin{gathered}
    v^{z2}(f,m) \equiv (\hat{V}(p^{z2}(f,m),\mathcal{K}_{v2}), t_{1})
\end{gathered}
\end{align*}

If the Submitter can't provide the proof in a specific time, the winner will be the Challenger.

If the Submitter provide the proof, the Challenger should provide the proof of the lost $\dot{T_l}$,  $\mathcal{K}_{z3}$ is the proving key of the circuit:
\begin{align*} 
\begin{gathered}
    p^{z3}(l) \equiv \hat{C}(\mathrm{B_f},\dot{T_l}, \mathcal{K}_{z3}) \equiv \\ 
    \dot{T_l} \notin \{ \dot{T_1},\ldots,\dot{T_m} \} \\
    \land \ \ \dot{T_l} \xrightarrow[]{map} \mathrm{B_f}
\end{gathered}
\end{align*}
 
The proof of $\dot{T_l}$ should be verified in a specific time, $\mathcal{K}_{v3}$ is the verification key of the circuit:
\begin{align*} 
\begin{gathered}
    v^{z3}(l) \equiv (\hat{V}(p^{z3}(l),\mathcal{K}_{v3}), t_{1})
\end{gathered}
\end{align*}


# FUTURE IMPROVEMENTS

# REFERENCES\
