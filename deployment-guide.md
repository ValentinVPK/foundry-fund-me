# Foundry Deployment Guide

## Dependencies

### Foundry DevOps Tools

```solidity
import {DevOpsTools} from "lib/foundry-devops/src/DevOpsTools.sol";
```

- Provides deployment management
- Enables contract verification
- Helps track recent deployments
- Manages deployment artifacts

### Chainlink Contracts

```solidity
import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
```

- Required for price feed integration
- Provides network-specific addresses
- Enables mainnet/testnet deployment

## Script Structure

### Basic Script Template

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {Script, console} from "forge-std/Script.sol";
import {FundMe} from "../src/FundMe.sol";

contract DeployFundMe is Script {
    function run() external returns (FundMe) {
        vm.startBroadcast();

        // Deploy contract
        FundMe fundMe = new FundMe();

        vm.stopBroadcast();
        return fundMe;
    }
}
```

## Deployment Commands

### Local Deployment

```bash
# Deploy to local Anvil
forge script script/DeployFundMe.s.sol:DeployFundMe --rpc-url http://localhost:8545 --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 --broadcast
```

### Testnet Deployment

```bash
# Deploy to Sepolia
forge script script/DeployFundMe.s.sol:DeployFundMe --rpc-url $SEPOLIA_RPC_URL --account sepoliaKey --broadcast --verify --etherscan-api-key $ETHERSCAN_API_KEY
```

## Environment Setup

### .env File

```env
SEPOLIA_RPC_URL=https://sepolia.infura.io/v3/your-api-key
ETHERSCAN_API_KEY=your-etherscan-api-key
```

### Makefile Commands

```makefile
deploy-sepolia:
    forge script script/DeployFundMe.s.sol:DeployFundMe --rpc-url $(SEPOLIA_RPC_URL) --account sepoliaKey --broadcast --verify --etherscan-api-key $(ETHERSCAN_API_KEY)
```

## Contract Verification

### Manual Verification

```bash
# Verify on Etherscan
forge verify-contract <deployed_address> FundMe --chain sepolia --watch
```

### Automatic Verification

```bash
# Add --verify flag to deployment
forge script script/DeployFundMe.s.sol:DeployFundMe --rpc-url $SEPOLIA_RPC_URL --account sepoliaKey --broadcast --verify
```

## Interaction Scripts

### Funding Contract

```solidity
contract FundFundMe is Script {
    function fundFundMe(address mostRecentlyDeployed) public {
        FundMe(payable(mostRecentlyDeployed)).fund{value: 0.01 ether}();
    }

    function run() external {
        address mostRecentlyDeployed = DevOpsTools.get_most_recent_deployment("FundMe", block.chainid);
        vm.startBroadcast();
        fundFundMe(mostRecentlyDeployed);
        vm.stopBroadcast();
    }
}
```

### Withdrawing Funds

```solidity
contract WithdrawFundMe is Script {
    function withdrawFundMe(address mostRecentlyDeployed) public {
        vm.startBroadcast();
        FundMe(payable(mostRecentlyDeployed)).withdraw();
        vm.stopBroadcast();
    }

    function run() external {
        address mostRecentlyDeployed = DevOpsTools.get_most_recent_deployment("FundMe", block.chainid);
        withdrawFundMe(mostRecentlyDeployed);
    }
}
```

## Deployment Best Practices

1. **Environment Variables**

   - Use .env for sensitive data
   - Never commit private keys
   - Use different keys for different networks

2. **Verification**

   - Always verify contracts
   - Use automatic verification when possible
   - Keep constructor arguments handy

3. **Testing**

   - Test deployment scripts locally first
   - Use Anvil for local testing
   - Test on testnet before mainnet

4. **Gas Optimization**
   - Monitor gas costs
   - Use gas estimation
   - Optimize constructor arguments

## Common Deployment Patterns

### 1. Using DevOps Tools

```solidity
import {DevOpsTools} from "lib/foundry-devops/src/DevOpsTools.sol";

// Get most recent deployment
address mostRecentlyDeployed = DevOpsTools.get_most_recent_deployment("FundMe", block.chainid);
```

### 2. Constructor Arguments

```solidity
// Deploy with constructor arguments
FundMe fundMe = new FundMe(priceFeedAddress);
```

### 3. Multi-Step Deployment

```solidity
function run() external {
    vm.startBroadcast();

    // Deploy dependencies first
    MockV3Aggregator priceFeed = new MockV3Aggregator(8, 200000000000);

    // Deploy main contract
    FundMe fundMe = new FundMe(address(priceFeed));

    vm.stopBroadcast();
}
```

## Troubleshooting

1. **Failed Deployments**

   - Check RPC URL
   - Verify account balance
   - Check gas limits

2. **Verification Issues**

   - Verify constructor arguments
   - Check compiler version
   - Verify optimization settings

3. **Network Issues**
   - Use reliable RPC providers
   - Check network status
   - Monitor gas prices
