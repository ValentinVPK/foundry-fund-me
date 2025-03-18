# Solidity Storage Guide

## Storage Basics

- Storage is permanent data storage on the blockchain
- Each contract has its own storage space
- Storage is expensive (gas-intensive)
- Data persists between function calls and transactions

## Storage Layout

- Storage is organized as a key-value store
- Each storage slot is 32 bytes (256 bits)
- Variables are packed together to save space
- Storage slots start from 0 and increment

## Storage vs Memory

- **Storage**: Permanent, expensive, persists on blockchain
- **Memory**: Temporary, cheap, exists only during function execution
- **Calldata**: Read-only, cheapest, exists only during function execution

## Storage Slots

- Each state variable gets a slot
- Variables smaller than 32 bytes can share slots
- Packing rules:
  - Variables are packed from right to left
  - New slot starts when:
    - Current slot is full
    - Next variable doesn't fit
    - Next variable is a different type

## Useful Commands

```bash
# View storage slot
cast storage <contract_address> <slot_number>

# View specific variable
cast storage <contract_address> <slot_number> --rpc-url <rpc_url>

# View all storage
cast storage <contract_address> --rpc-url <rpc_url>
```

## Storage Optimization Tips

1. Pack variables together when possible
2. Use smaller data types
3. Order variables by size
4. Use memory for temporary data
5. Use calldata for read-only function parameters

## Example

```solidity
contract StorageExample {
    uint256 a;      // slot 0
    uint128 b;      // slot 1 (first half)
    uint128 c;      // slot 1 (second half)
    bytes32 d;      // slot 2
    uint8 e;        // slot 3 (first byte)
    uint8 f;        // slot 3 (second byte)
    // ... rest of slot 3 is empty
}
```

## Common Storage Patterns

1. **Mapping Storage**:

   - Uses keccak256 hash of key + slot number
   - Each key-value pair gets its own slot

2. **Array Storage**:

   - Length stored at slot number
   - Elements stored sequentially starting at keccak256(slot)

3. **Struct Storage**:
   - Packed according to size

- Follows same rules as individual variables
