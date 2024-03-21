---
tags:
  - gas
  - base fee formula
  - gas estimation
---

# Gas

**Gas** is a representation of the cost incurred by resources per transaction in the ParallelChain Mainnet ecosystem. Every transaction is assigned a cost through gas metering.

There are different categories of gas cost in the ParallelChain Mainnet ecosystem:

- WASM opcode execution inside a contract call
- Reading and writing WASM memory from host functions inside a contract call
- Transaction-related data storage
- World state storage and access
- Cryptographic operations

Details about gas can be found in [ParallelChain Mainnet Protocol](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/Gas.md).

## How are Gas Fees Calculated?

**Gas Fee** here refers to the amount that is paid with the account's balance for the gas consumed in transaction execution. It can be generalized by the following equation:

```text
Gas Fee = Gas Consumption x (Base Fee Per Gas + Priority Fee Per Gas)
```

- `Gas Consumption` is the gas unit consumed during the transaction execution. 
- `Base Fee Per Gas` is the dynamically adjusted value that depends on the traffic of the network at the time of transaction execution. Please see the section [Base Fee Formula](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/Blockchain.md#Base-fee-formula) in ParallelChain Mainnet Protocol for details.
- `Priority Fee Per Gas` is the amount specified in the transaction by the signer, which is transferred to block producer, on per-gas basis.

## Gas Estimation

Transaction can fail for setting the Gas limit too low. There is no absolute answer to what gas limit should be specified. In general, the gas limit must be **greater than Transaction Inclusion Cost** and **smaller than Block Gas Limit (500000000)**.

The following table summaries on estimated gas consumption of different transaction commands:

|Command|Estimation|
|:---|:---|
|Transfer|166,350|
|Deploy Contract|size of contract bytecode * 2600 |
|Call Contract| Varies a lot depends on contract implementation|
|Create Deposit| 216,370 |
|Topup Deposit| 185,630 |
|Set Deposit Setting| 159,560 |
|Withdraw Deposit| 4,000,000 (varies a lot depends on the state of the pools)|
|Stake| 4,000,000 (varies a lot depends on the state of the pools)|
|Unstake| 4,000,000 (varies a lot depends on the state of the pools)|

*The above values are calulated by including the transaction inclusion cost (Assume one transaction with one command). It is estimated based on experiments in ParallelChain Mainnet version as of the date on 11-May-2023.*

