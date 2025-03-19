# Foundry Testing Guide

## Dependencies

### Forge Standard Library (forge-std)

```solidity
import {Test, console} from "forge-std/Test.sol";
```

- Provides testing utilities
- Includes console logging
- Offers VM manipulation functions

### Chainlink Contracts

```solidity
import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
```

- Used for price feed testing
- Provides interfaces for Chainlink contracts
- Enables mock price feed creation

## Types of Tests

1. **Unit Tests**

   - Testing individual components in isolation
   - Fast and focused
   - Located in `test/unit/`

2. **Integration Tests**

   - Testing how components work together
   - More complex setup
   - Located in `test/integration/`

3. **Forked Tests**
   - Testing against live networks
   - Simulates real environment
   - Uses `--fork-url` flag

## Foundry Testing Features

### VM (Virtual Machine) Functions

```solidity
// Set the next caller
vm.prank(address)

// Set the next caller and value
vm.deal(address, amount)

// Set the next caller and value in one call
vm.deal(address, amount)

// Set gas price
vm.txGasPrice(price)

// Record gas usage
uint256 gasStart = gasleft();
// ... code ...
uint256 gasEnd = gasleft();
uint256 gasUsed = (gasStart - gasEnd) * tx.gasprice;

// Expect a revert
vm.expectRevert();

// Set block timestamp
vm.warp(timestamp);

// Set block number
vm.roll(blockNumber);
```

### Test Setup

```solidity
function setUp() external {
    // Runs before each test
    // Setup test environment
    // Deploy contracts
    // Set initial state
}
```

### Test Modifiers

```solidity
modifier funded() {
    vm.prank(USER);
    fundMe.fund{value: SEND_VALUE}();
    _;
}

// Usage
function testSomething() public funded {
    // Test code here
}
```

### Assertions

```solidity
// Equality
assertEq(a, b);

// Inequality
assertNotEq(a, b);

// Greater than
assertGt(a, b);

// Less than
assertLt(a, b);

// Greater than or equal
assertGe(a, b);

// Less than or equal
assertLe(a, b);
```

## Best Practices

1. **Test Organization**

   - One test file per contract
   - Clear test names
   - Group related tests together

2. **Test Coverage**

   - Test happy path
   - Test edge cases
   - Test error conditions
   - Test state changes

3. **Gas Testing**

   - Use `forge snapshot` for gas comparisons
   - Test gas optimization changes
   - Monitor gas costs in tests

4. **Fork Testing**
   - Test against mainnet state
   - Use specific block numbers
   - Handle network issues gracefully

## Common Testing Patterns

1. **State Setup**

```solidity
function setUp() external {
    DeployFundMe deployFundMe = new DeployFundMe();
    fundMe = deployFundMe.run();
    vm.deal(USER, STARTING_BALANCE);
}
```

2. **Event Testing**

```solidity
vm.expectEmit(true, true, true, true);
emit Funded(address, amount);
```

3. **Access Control Testing**

```solidity
vm.expectRevert();
vm.prank(USER);
contract.ownerOnlyFunction();
```

4. **Gas Optimization Testing**

```solidity
uint256 gasStart = gasleft();
functionCall();
uint256 gasEnd = gasleft();
uint256 gasUsed = (gasStart - gasEnd) * tx.gasprice;
```

## Running Tests

```bash
# Run all tests
forge test

# Run specific test file
forge test --match-path test/unit/FundMeTest.t.sol

# Run specific test function
forge test --match-test testWithdrawFromMultipleFunders

# Run tests with verbosity
forge test -vvvv

# Run tests with gas reporting
forge test --gas-report
```
