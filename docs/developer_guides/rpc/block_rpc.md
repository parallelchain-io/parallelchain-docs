---
tags:
  - Tools
  - ParallelChain RPC API
---

# ParallelChain RPC API

## Block RPCs

---

### block

Get a block by its block hash.

#### Request

```rust
struct BlockRequest {
    block_hash: CryptoHash,
}
```

#### Response

```rust
struct BlockResponse {
    block: Option<Block>,
}
```

---

### block_header

Get a block header by its block hash.

#### Request

```rust
struct BlockHeaderRequest {
    block_hash: CryptoHash,
}
```

#### Response

```rust
struct BlockHeaderResponse {
    block_header: Option<BlockHeader>,
}
```

---

### block_height_by_hash

Get the height of the block with a given block hash. 

#### Request

```rust
struct BlockHeightByHashRequest {
    block_hash: CryptoHash,
}
```

#### Response

```rust
struct BlockHeightByHashResponse {
    block_hash: CryptoHash,
    block_height: Option<BlockHeight>,
}
```

---

### block_hash_by_height

Get the hash of a block at a given height.

#### Request

```rust
struct BlockHashByHeightRequest {
    block_height: BlockHeight,
}
```

#### Response

```rust
struct BlockHashByHeightResponse {
    block_height: BlockHeight,
    block_hash: Option<CryptoHash>,
}
```

---

### highest_committed_block

Get the hash of the highest committed block. 

#### Request

None.

#### Response

```rust
struct HighestCommittedBlockResponse {
    block_hash: Option<CryptoHash>,
}
```
