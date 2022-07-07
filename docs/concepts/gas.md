---
tags:
  - testnet 2.0
  - gas
---

# Gas
---

**Gas**: Gas is a representation of cost incurred by computational resources per transaction. ParallelChain F assigns a cost to every transaction through gas metering. 

Gas costs are divided into two components:

- The compute costs in terms of latency or number of CPU cycles expended by the WASM opcodes required to execute the transaction.
- The storage costs in bytes required to store/update data in the world state.
- The compute costs for precompile functions used by developers to deploy smart contracts on ParallelChain F.

WASM supports a total of 513 opcodes which aid sequential, vectorized, memory, logical operations and exception handling. ParallelChain F ecosystem categorizes these opcodes on basis of their operations. Some opcode families induce non-determinism when executed. These are identified and disallowed in the ParallelChain F ecosystem with the help of a custom middleware. 

## WASM Opcode Categories
---

ParallelChain F divides WASM opcodes into three major categories:

- **Opcodes that support mathematical operations** on integer and floating point data types, logical operations, exception 
    handling, memory and constants [For example get_local, i32.add, f32.add, i32.load, i32.store etc.].

- **Opcodes that support atomic operations** through WASM threads [For example i64.atomic.rmw8.add_u, atomic.notify, etc.].For 
    more information, see [here](https://github.com/WebAssembly/threads/blob/master/proposals/threads/Overview.md).

- **Opcodes that support Fixed-Width SIMD operations** [For example i32x4.add, i16x8.ne, f32x4.add etc.]. For more information, 
   see [here](https://github.com/WebAssembly/simd/blob/main/proposals/simd/SIMD.md).


A subset of these opcodes are known to induce non-determinism when executed.These include floating point operations, fixed width SIMD and atomic operations with WASM threads. For more information about non-determinism in WebAssembly, see [here](https://github.com/WebAssembly/design/blob/main/Nondeterminism.md).__All non-deterministic opcodes are currently disabled in the ParallelChain F ecosystem__. Therefore, transactions consisting of non-deterministic opcodes in their WebAssembly, will generate a [Receipt Status Code](../getting_started/status_code.md) "Disallowed Opcode", when executed.


## Opcode Gas costs
---

Gas fees for each WASM opcode family are defined in terms of the total latency of the corresponding x86-64 Assembly Instructions, each opcode is translated into. For defining latency of each x86-64 Assembly Instruction, we refer to one specific CPU model from Coffee Lake Refresh, Intel's 9th Generation microprocessor family (family: 06, model: 9E). These costs have been tabulated below.

   **Constants**
    
   | Opcode | Gas Cost |
   |:--- |:--- |
   I32Const  |0|
   I64Const  |0|

   **Type parameteric operators**
  
   | Opcode | Gas Cost | 
   |:--- |:--- |
   Drop |2| 
   Select |3|

   **Flow control**
  
   | Opcode | Gas Cost |
   |:--- |:--- |
   Nop |0|
   Unreachable |0|
   Else |0|
   Loop  |0|
   If  |0| 
   Br |2|
   BrTable |2| 
   Call |2| 
   CallIndirect |2|
   Return |2|
   BrIf |3|
     
   **Registers**
    
   | Opcode | Gas Cost |
   |:--- |:--- |
   GlobalGet  |3|
   GlobalSet |3|
   LocalGet  |3|
   LocalSet  |3|
      
   **Reference Types**
    
   | Opcode | Gas Cost |
   |:--- |:--- |
   RefIsNull |2| 
   RefFunc |2|
   RefNull |2| 
   ReturnCall |2| 
   ReturnCallIndirect |2| 
      
   **Exception Handling**
    
   | Opcode | Gas Cost |
   |:--- |:--- |
   CatchAll |2| 
   Throw |2| 
   Rethrow |2| 
   Delegate |2|

   **Bulk Memory Operations**
    
   | Opcode | Gas Cost |
   |:--- |:--- |
   ElemDrop  |1|
   DataDrop  |1|
   TableInit |2| 
   MemoryCopy |3|
   MemoryFill |3|
   TableCopy  |3| 
   TableFill  |3| 

   **Memory Operations**
   
   | Opcode | Gas Cost |
   |:--- |:--- |
   I32Load |3|
   I64Load |3|
   I32Store |3|
   I64Store |3|
   I32Store8 |3| 
   I32Store16 |3|
   I32Load8S |3|
   I32Load8U |3|
   I32Load16S |3|
   I32Load16U |3|
   I64Load8S |3| 
   I64Load8U |3|
   I64Load16S |3|
   I64Load16U |3|
   I64Load32S |3|
   I64Load32U |3|
   I64Store8 |3|
   I64Store16 |3|
   I64Store32 |3|   

   **32 and 64-bit Integer Arithmetic Operations**
   
   | Opcode | Gas Cost |
   |:--- |:--- |
   I32Add|1|
   I32Sub|1|
   I64Add|1|
   I64Sub|1|
   I64LtS|1| 
   I64LtU|1| 
   I64GtS|1| 
   I64GtU|1| 
   I64LeS|1|
   I64LeU|1|
   I64GeS|1| 
   I64GeU|1| 
   I32Eqz|1|
   I32Eq|1|
   I32Ne|1|
   I32LtS|1| 
   I32LtU|1|
   I32GtS|1|
   I32GtU|1|
   I32LeS|1|
   I32LeU|1|
   I32GeS|1|
   I32GeU|1|
   I64Eqz|1|
   I64Eq|1|
   I64Ne|1|
   I32And|1|
   I32Or|1|
   I32Xor|1|
   I64And|1|
   I64Or|1|
   I64Xor|1|
   I32Shl |2|
   I32ShrU |2| 
   I32ShrS |2|
   I32Rotl |2|
   I32Rotr |2| 
   I64Shl |2|
   I64ShrU |2|
   I64ShrS |2|
   I64Rotl |2|
   I64Rotr  |2|
   I32Mul |3| 
   I64Mul |3|
   I32DivS |80|
   I32DivU |80|
   I32RemS |80| 
   I32RemU |80| 
   I64DivS |80| 
   I64DivU |80| 
   I64RemS |80| 
   I64RemU |80|
   I32Clz |105| 
   I64Clz |105|
      
   **Type Casting & Truncation Operations**
   
   | Opcode | Gas Cost |
   |:--- |:--- |
   I32WrapI64 |3|
   I32Extend8S |3| 
   I32Extend16S |3|
   I64ExtendI32S |3|  
   I64ExtendI32U |3| 
   I64Extend8S |3| 
   I64Extend16S |3| 
   I64Extend32S |3| 


## Storage Gas Costs
---

The storage costs defined in ParallelChain F:

| Definition | Operation | Cost 
|:--- |:--- |:--- |
TX_BASE_GAS                                                  |Paid by every transaction. Offsets the cost of reading and then writing 4 specified world state keys (from_account/NTxsFromAccount, from_account/Balance, validator_account/Balance, to_account/Balance).|44200
BLOCKCHAIN_DATA_BASE                                         |Base cost of writes to blockchain transaction data.|100|
BLOCKCHAIN_DATA_BYTE                                         |Cost of writes to the blockchain transaction data per byte.|100|
WORLD_STATE_READ_WRITE_BASE                                  |Base cost of reads and writes on the World State.|100|
WORLD_STATE_READ_KEY_BYTE                                    |Cost of reading from the World State *per key byte*.|150|
WORLD_STATE_READ_VALUE_BYTE                                  |Cost of reading from the World State *per value byte*.|1|
WORLD_STATE_WRITE_KEY_BYTE                                   |Cost of writing into the World State *per key byte*.|150|
WORLD_STATE_WRITE_VALUE_BYTE                                 |Cost of writing into the World State *per value byte*.|150|


## Gas Costs for Precompiles
---

Gas fees for precompile functions are defined according in terms of the total latency of the emitted x86-64 Assembly Instructions when these 
functions are executed.
For more information on the gas costs for precompiles. Please see ["Precompiles Gas Costs"](../../smart_contract_sdk/advance/precompiles)
