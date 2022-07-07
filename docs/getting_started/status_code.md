---
tags:
  - testnet 2.0
  - parallelchain light client
  - status code
---

# Status Code

Inside the receipt, you can see the `status_code` in digit, and the gas consumed for the transaction.

Transaction receipt status code defines the status code in the transaction receipts. It indicates the reason for error for situations such as token transactions, contract execution or contract deployment, etc.

Categorization on Receipt Status Code

- 1x - Pre-Inclusion Decision failures. Not included in blocks.
- 2x - Deploy errors.
- 3x - EtoC errors (not in internal transaction)
- 4x - Internal transaction errors


|Code|Status|Description|
|:---|:---|:---|
|00|Success|Successful transaction|
|10| Wrong Nonce | Incorrect account nonce is used in transaction. Each nonce should only be used once and in sequential order.|
|11| Not Enough Balance For Gas Limit | Not enough balance to pay for gas limit.|
|12| Not Enough Balance For Transfer | Not enough balance to pay for transfer.|
|13| Pre-Execution Gas Exhausted | Gas limit was insufficient to cover pre-execution costs, or set the gas limit too low. |
|20| Disallowed Opcode | Fail to loal contract because the contract bytecode contains disallowed opcodes.|
|21| Cannot Compile | Contract cannot be compiled into machine code (it is probably invalid WASM).|
|22| No Exported ContractMethod | Contract does not export the METHOD_CONTRACT method. |
|23| Other Deploy Error | Deployment failed for some other reasons.|
|30| Execution Proper Gas Exhausted | Gas limit was insufficient to cover execution proper costs.|
|31| Runtime Error| Runtime error during execution proper of the entree smart contract.|
|40| Internal Execution Proper Gas Exhaustion | Gas limit was insufficient to cover execution proper costs of an internal transaction.|
|41| Internal Runtime Error | Runtime error during execution proper of an internal transaction.|
|42| Internal Not Enough Balance For Transfer | Not enough balance to pay for transfer in an internal transaction.|
|43| Other Error| Other error.|