---
tags:
  - testnet 2.0
  - TXPLL
---

# TXPLL

## What is TXPLL?
---

**TXPLL** is token, the currency used within ParallelChain Testnet ecosystem.

## Denomination
---

The lowest denomination of the **TXPLL** token is named as **Gray**. Users will have to buy **TXPLL** tokens to indirectly 
pay for computation and storage costs within the ParallelChain Testnet Virtual Machine (VM). 

The goal of defining a denomination for the **TXPLL** token is to standardize its scale with all currencies in the 
financial system. The denomination table, and its relationship with gas costs are defined in the upcoming sections.


ParallelChain Testnet implements denomination of **TXPLL** down to the 8-th place of decimal, the base or the smallest
unit being **Gray**. The name of the denominations, their abbreviation and their value in **TXPLL** , have been 
defined below in the following table.

| Denomination | Abbreviation  | Value in TXPLL
|:---          |:---           |:---
|Gray          |Gr             | 0.00000001 TXPLL|
|KiloGray      |KGr            | 0.00001 TXPLL|  
|MegaGray      |MGr            | 0.01 TXPLL| 
|TXPLL         |TXPLL          | 1 TXPLL| 
|TeraGray      |TGr            | 10000 TXPLL| 
|PetaGray      |PGr            | 10000000 TXPLL| 

## Gas Costs and TXPLL
---

Transactions on  ParallelChain Testnet require some cost for utilization of computational resources. To charge for these 
costs, ParallelChain Testnet implements gas price units in terms of **TXPLL** (i.e. 1 **TXPLL** equals to 1 gas). This is done to dynamically 
balance the actual cost of gas consumed during transactions when the price of the token increases or decreases due to economic factors. 
Based on the conversion factor between **TXPLL** and gas units, it is assumed that 1 CPU execution cycle is equal to 1 **Gray** or 
1 second of CPU execution is equal to 10^8 gas.

