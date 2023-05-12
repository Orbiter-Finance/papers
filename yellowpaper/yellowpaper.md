---
title: 'Orbiter: A Secure, Fast, Low Gas Decentralized Cross Rollup Bridge Protocol'
author: Orbiter Finance
fontsize: 9pt
geometry: margin=1.5cm
date: \textit{Pre-release, \today}
abstract: |
	With the continuous expansion and development of the second-layer network ecology of Ethereum, such as Arbitrum, Optimism, zkSync, StarkNet, etc, users cross rollup transacations. In this paper, we propose a optimistic interoperability , secure, low-gas, low-latency decentralized architectures for transfer of standard assets between rollup Layer2.
urlcolor: cyan
bibliography: yellowpaper.bib
classoption:
    - twocolumn
    # - onecloumn
header-includes:
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

Vitalik Buterin had proposed a easy decentralizd cross-layer-2 bridge.


# DESIGN PRINCINPLES
Orbiter Protocol's design follows the flowing principles:

- **Secure.** The priority of security is the highest, mainly around the security of funds involved in the roles in the protocol layer.
- **Decentralized.** The assets pledged by the role will not be seized by any role in the agreement, and any evil behavior will be punished by the mechanism of the protocol layer.
- **Efficiency Sensitive.** Efficiency of capital utilization and gas fees for users in the process of cross-chain execution.

# OVERVIEW

Orbiter aims to build a secure, decentralized and efficiency sensitive cross rollup bridge on Ethereum ecology, and raises the interoperability of the standard assets among these rollup environments. These requirements will dictate the following properties:

- first
- second


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

- **MDC**: Maker Deposit Contract 

- **EBC**: Event Binding Contract 

- **ZK-SPV**: Zero Knowledge Simple Payment Verification. Prove the existence and rationality of Orbiter cross-chain Tx through zero-knowledge proof technology. Existence means that both source transaction and target transaction can be proved on L1 that they actually happened on the corresponding L2, and rationality means It can prove the intention of the user of SrcTx, and the result of the maker’s payment in DstTx conforms to specific rules.
- **FeeManager**: Maintain the information of all Dealers, manage and update the benefits that Dealers get from Makers, and ensure the correctness of revenue status updates through the arbitration penalty mechanism.
- **DaoManager**

## Off Chain Compoment

- Maker Client

# ZK-SPV

Use ZK-SNARK cryptography technology to reduce the gas consumed by the proof of transaction validity

## Prove Primitives

The challenger should provide the ZK Proof of the source transaction, for the exsitence of that.

**Proof Generation**. The proof computation function $\hat{C}$ for user's intention with address $a$: $\mathrm{I_{a}}$ is defined as:
\begin{align*} 
\begin{gathered}
    p^{z}(\mathrm{I_{a}}) \equiv \hat{C}(T_{S}, \mathrm{TIMESTAMP_{S}}, \mathrm{D}, \mathrm{M_{A}}, \dot{M}, \mathcal{K}_{z})
\end{gathered}
\end{align*}

$\mathcal{K}_{z}$ is the proving key of the circuit.

**Proof Verification**. To verify a validity proof $p^{z}$ genenrated by user $a$'s intention
\begin{align*} 
\begin{gathered}
    v^{z}(\mathrm{I_{a}}) \equiv \hat{V}(p^{z},a,\mathcal{K}_{v})
\end{gathered}
\end{align*}

$v^{z}$ is the result of verification of a proof ,which would be storage on Ethereum. $\mathcal{K}_{v}$ is the verification key of the circuit.

\begin{align*} 
\begin{gathered}
    p^{z}(\mathrm{R_{a}}) \equiv \hat{C}(T_{D}, \mathrm{TIMESTAMP_{D}}, \mathrm{D_{ta}}, \mathrm{M_{A}}, \dot{M}, \mathcal{K}_{z})
\end{gathered}
\end{align*}

\begin{align*} 
\begin{gathered}
    v^{z}(\mathrm{R_{a}}) \equiv \hat{V}(p^{z},a,\mathcal{K}_{v})
\end{gathered}
\end{align*}

The mechanism of challenge can be formalized as:
\begin{align*} 
\begin{gathered}
    \mathcal{C}_{hallenge}(\mathrm{I_{a}}) \equiv \mathrm{SPV}((p,v)^{z}(\mathrm{I_{a}})) \land \mathrm{SPV}((p,v)^{z}(\mathrm{R_{a}}))
\end{gathered}
\end{align*}

The final adjudication can be formalized as:

\begin{align*} 
\begin{gathered}
    \mathcal{C}_{hallenge}(\mathrm{I_{a}}) \equiv \mathrm{SPV}((p,v)^{z}(\mathrm{I_{a}})) \land \mathrm{SPV}((p,v)^{z}(\mathrm{R_{a}}))
\end{gathered}
\end{align*}

$$
    \mathcal{A}_{djudication} (\mathrm{I_{a}}) \equiv \left\{ 
    \begin{aligned}
    \end{aligned}
    \right
$$

# TX ARBITRATION
# SECURITY MODEL

# FUTURE IMPROVEMENTS

# REFERENCES\
