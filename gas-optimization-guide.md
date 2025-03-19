# Solidity Gas Optimization Guide

## Storage Optimization

### 1. Pack Variables

```solidity
// Bad - wastes 31 bytes
uint8 a;
uint256 b;
uint8 c;

// Good - packs variables together
uint256 b;
uint8 a;
uint8 c;
```

### 2. Use Appropriate Data Types

```solidity
// Bad - using uint256 for small numbers
uint256 smallNumber = 5;

// Good - using uint8 for small numbers
uint8 smallNumber = 5;
```

### 3. Use Mappings Instead of Arrays

```solidity
// Bad - expensive to iterate
address[] public funders;

// Good - O(1) access
mapping(address => uint256) public addressToAmountFunded;
```

## Memory Optimization

### 1. Use Memory Instead of Storage

```solidity
// Bad - expensive storage read
uint256[] storage array = storageArray;

// Good - cheaper memory copy
uint256[] memory array = storageArray;
```

### 2. Use Calldata for Read-Only Parameters

```solidity
// Bad - copies to memory
function processArray(uint256[] memory array)

// Good - reads directly from calldata
function processArray(uint256[] calldata array)
```

## Function Optimization

### 1. Use View/Pure Functions

```solidity
// Bad - state-changing function
function getBalance() public returns (uint256)

// Good - view function
function getBalance() public view returns (uint256)
```

### 2. Batch Operations

```solidity
// Bad - multiple transactions
function fund() public payable
function withdraw() public

// Good - single transaction
function fundAndWithdraw() public payable
```

### 3. Use Events Instead of Storage

```solidity
// Bad - expensive storage
mapping(address => bool) public hasVoted;

// Good - event-based tracking
event Voted(address indexed voter);
```

## Loop Optimization

### 1. Cache Array Length

```solidity
// Bad - reads length on each iteration
for (uint256 i = 0; i < array.length; i++)

// Good - caches length
uint256 length = array.length;
for (uint256 i = 0; i < length; i++)
```

### 2. Use Unchecked Blocks

```solidity
// Bad - expensive overflow check
for (uint256 i = 0; i < length; i++)

// Good - unchecked for known safe operations
for (uint256 i = 0; i < length;) {
    unchecked {
        ++i;
    }
}
```

## Gas Measurement

### 1. Using Forge Snapshot

```bash
# Create gas snapshot
forge snapshot

# Compare with previous snapshot
forge snapshot --diff
```

### 2. Manual Gas Measurement

```solidity
uint256 gasStart = gasleft();
// ... operation ...
uint256 gasEnd = gasleft();
uint256 gasUsed = (gasStart - gasEnd) * tx.gasprice;
```

## Common Gas-Saving Patterns

### 1. Bit Packing

```solidity
// Bad - multiple storage slots
bool isActive;
uint256 timestamp;
uint8 version;

// Good - packed into one slot
uint256 packedData; // bits 0-7: version, bit 8: isActive, bits 9-255: timestamp
```

### 2. Use Events for Historical Data

```solidity
// Bad - storing all historical data
mapping(uint256 => Transaction) public transactions;

// Good - emit events for historical data
event TransactionExecuted(address indexed from, uint256 amount);
```

### 3. Optimize String Operations

```solidity
// Bad - expensive string concatenation
string memory result = string(abi.encodePacked(str1, str2));

// Good - use bytes32 for fixed-length strings
bytes32 public constant NAME = "FundMe";
```

## Best Practices

1. **Storage Access**

   - Minimize storage reads
   - Cache storage values in memory
   - Use events for historical data

2. **Function Design**

   - Keep functions small and focused
   - Use view/pure when possible
   - Batch operations when logical

3. **Data Structures**

   - Use mappings over arrays
   - Pack variables efficiently
   - Choose appropriate data types

4. **Testing**
   - Test gas costs
   - Compare optimization results
   - Monitor gas usage in production
