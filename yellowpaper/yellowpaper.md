---
title: 'Orbiter: A Secure, Fast, Low Gas Decentralized Cross Rollup Bridge Protocol'
author: Orbiter Finance
fontsize: 9pt
geometry: margin=2cm
date: \textit{Pre-release, \today}
abstract: |
	With the continuous expansion and development of the second-layer network ecology of Ethereum, such as Arbitrum, Optimism, zkSync, StarkNet, etc, users cross rollup transacations. In this paper, we propose a secure, low-gas, low-latency decentralized architectures for transfer of standard assets between rollup Layer2.
urlcolor: cyan
bibliography: yellowpaper.bib
classoption:
    - twocolumn
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


# DESIGN PRINCINPLES
Orbiter Protocol's design follows the flowing principles:

- **Secure.**
- **Decentralized.**
- **Efficiency Sensitive.**

# OVERVIEW

Orbiter aims to build a secure, decentralized and efficiency sensitive cross rollup bridge on Ethereum ecology, and raises the interoperability of the standard assets among these rollup environments. These requirements will dictate the following properties:

- first
- second


# THE ORBITER PROTOCOL


## Orbiter Transactions

User sends a specific amount of ERC20 or native token to the maker, the amount contants the following parts

- _Transfer Amounts_ 
- _Witholding Fee_ 
- _Destination Chain ID_ 
- _Dealre ID_ 
- _EBC ID_ 

## CORE CONTRACT 

- **MDC**: Maker Deposit Contract 

- **EBC**: Event Binding Contract 

- **ZK-SPV**: Zero Knowledge Simple Payment Verification. Prove the existence and rationality of Orbiter cross-chain Tx through zero-knowledge proof technology. Existence means that both SrcTx and DstTx can prove on L1 that they actually happened on the corresponding L2, and rationality means It can prove the intention of the user of SrcTx, and the result of the maker’s payment in DstTx conforms to specific rules.
- **FeeManager**
- **DaoManager**

## Off Chain Compoment

- Maker Client

# ZK-SPV

## Prove Primitives



# TX ARBITRATION
# SECURITY MODEL

# FUTURE IMPROVEMENTS

# References\
