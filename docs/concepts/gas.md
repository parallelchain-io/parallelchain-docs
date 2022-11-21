---
tags:
  - testnet 3
  - gas
---

# Gas

**Gas**: Gas is a representation of cost incurred by computational resources per transaction. ParallelChain F assigns a cost to every transaction through gas metering. 

Gas costs are divided into two components:

- The compute costs in terms of latency or number of CPU cycles expended by the WASM opcodes required to execute the transaction.
- The storage costs in bytes required to store/update data in the world state.
- The compute costs for precompile functions used by developers to deploy smart contracts on ParallelChain F.

WASM supports over 500 opcodes which aid sequential, vectorized, memory, logical operations and exception handling. ParallelChain F ecosystem categorizes these opcodes on basis of their operations. Some opcode families induce non-determinism when executed. These are identified and disallowed in the ParallelChain F ecosystem with the help of a custom middleware. 

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

| Opcodes | Gas Cost |
|:--- |:--- |
I32Const  |0|
I64Const  |0|

**Type parameteric operators**

| Opcodes | Gas Cost | 
|:--- |:--- |
Drop |2| 
Select |3|

**Flow control**

| Opcodes | Gas Cost |
|:--- |:--- |
Nop, Unreachable, Else, Loop, If  |0| 
Br, BrTable, Call, CallIndirect, Return |2|
BrIf |3|
  
**Registers**

| Opcodes | Gas Cost |
|:--- |:--- |
GlobalGet, GlobalSet, LocalGet, LocalSet  |3|
  
**Reference Types**

| Opcodes | Gas Cost |
|:--- |:--- |
RefIsNull, RefFunc, RefNull, ReturnCall, ReturnCallIndirect |2| 
  
**Exception Handling**

| Opcodes | Gas Cost |
|:--- |:--- |
CatchAll, Throw, Rethrow, Delegate |2|

**Bulk Memory Operations**

| Opcodes | Gas Cost |
|:--- |:--- |
ElemDrop, DataDrop  |1|
TableInit |2| 
MemoryCopy, MemoryFill, TableCopy, TableFill  |3| 

**Memory Operations**

| Opcodes | Gas Cost |
|:--- |:--- |
|I32Load, I64Load, I32Store, I64Store, I32Store8, I32Store16, I32Load8S, I32Load8U, I32Load16S, I32Load16U, I64Load8S, I64Load8U, I64Load16S, I64Load16U, I64Load32S, I64Load32U, I64Store8, I64Store16, I64Store32 |3|   

**32 and 64-bit Integer Arithmetic Operations**

| Opcodes | Gas Cost |
|:--- |:--- |
|I32Add, I32Sub, I64Add, I64Sub, I64LtS, I64LtU, I64GtS, I64GtU, I64LeS, I64LeU, I64GeS, I64GeU, I32Eqz, I32Eq, I32Ne, I32LtS, I32LtU, I32GtS, I32GtU, I32LeS, I32LeU, I32GeS, I32GeU, I64Eqz, I64Eq, I64Ne, I32And, I32Or, I32Xor, I64And, I64Or, I64Xor, |1|
|I32Shl, I32ShrU, I32ShrS, I32Rotl, I32Rotr, I64Shl, I64ShrU, I64ShrS, I64Rotl, I64Rotr,  |2|
|I32Mul, I64Mul |3|
|I32DivS, I32DivU, I32RemS, I32RemU, I64DivS, I64DivU, I64RemS, I64RemU |80|
|I32Clz, I64Clz |105|
  
**Type Casting & Truncation Operations**

| Opcodes | Gas Cost |
|:--- |:--- |
|I32WrapI64, I32Extend8S, I32Extend16S, I64ExtendI32S, I64ExtendI32U, I64Extend8S, I64Extend16S, I64Extend32S |3| 


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
