# Pasifika Stacks Exchange AMM Backend

A Bitcoin-secured Automated Market Maker (AMM) built on Stacks blockchain, designed specifically for the Pasifika Web3 Tech Hub ecosystem. This backend provides the smart contract infrastructure for decentralized token swapping, liquidity provision, and yield farming with Bitcoin-level security.

## Overview

The Pasifika Stacks Exchange AMM is a proof-of-concept decentralized exchange that enables:

- **Token Swapping**: Automated market making with constant product formula
- **Liquidity Provision**: Pool creation and liquidity management
- **Yield Farming**: Rewards for liquidity providers
- **Bitcoin Security**: Built on Stacks blockchain for Bitcoin-secured transactions

## Architecture

### Smart Contracts

#### Core AMM Contract (`contracts/amm.clar`)
The main AMM contract implementing:
- **Pool Management**: Create and manage liquidity pools
- **Liquidity Operations**: Add and remove liquidity with proper ratio calculations
- **Token Swapping**: Execute swaps using constant product formula (x * y = k)
- **Fee Collection**: Configurable fees for each pool
- **Position Tracking**: Track user liquidity positions

#### Mock Token Contract (`contracts/mock-token.clar`)
A SIP-010 compliant fungible token for testing and development purposes.

### Key Features

- **Constant Product AMM**: Uses the proven x * y = k formula
- **Configurable Fees**: Each pool can have custom fee structures
- **Minimum Liquidity**: Prevents dust attacks with minimum liquidity requirements
- **Position Management**: Tracks individual user positions in pools
- **Token Ordering**: Ensures consistent token ordering for pool identification

## Quick Start

### Prerequisites

- [Node.js](https://nodejs.org/) (v16 or higher)
- [Clarinet](https://github.com/hirosystems/clarinet) (Stacks development toolkit)
- [Git](https://git-scm.com/)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd pasifika-stacks-exchange
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Verify Clarinet installation**
   ```bash
   clarinet --version
   ```

### Development Setup

1. **Check contract syntax**
   ```bash
   clarinet check
   ```

2. **Run tests**
   ```bash
   npm test
   ```

3. **Run tests with coverage**
   ```bash
   npm run test:report
   ```

4. **Watch mode for development**
   ```bash
   npm run test:watch
   ```

## Testing

The project includes comprehensive unit tests using Vitest and Clarinet SDK:

### Test Structure
- `tests/amm.test.ts` - Core AMM functionality tests
- `tests/mock-token.test.ts` - Token contract tests

### Running Tests

```bash
# Run all tests
npm test

# Run tests with coverage and cost analysis
npm run test:report

# Watch mode (auto-run tests on file changes)
npm run test:watch
```

### Test Coverage
Tests cover:
- Pool creation and validation
- Liquidity addition and removal
- Token swapping mechanics
- Error handling and edge cases
- Fee calculations
- Position tracking

## Deployment

### Network Configurations

The project supports deployment to multiple networks:

- **Simnet** (Local simulation): `deployments/default.simnet-plan.yaml`
- **Devnet** (Development network): `deployments/default.devnet-plan.yaml`
- **Testnet** (Public test network): `deployments/default.testnet-plan.yaml`

### Deployment Commands

1. **Deploy to Simnet (local testing)**
   ```bash
   clarinet integrate
   ```

2. **Deploy to Devnet**
   ```bash
   clarinet deployment generate --devnet
   clarinet deployment apply --devnet
   ```

3. **Deploy to Testnet**
   ```bash
   # Generate deployment plan with cost strategy
   clarinet deployment generate --testnet --low-cost
   
   # Apply deployment to testnet
   clarinet deployment apply --testnet
   ```
   
   **Prerequisites for Testnet Deployment:**
   - Create `settings/Testnet.toml` with your wallet configuration
   - Ensure your testnet wallet has sufficient STX tokens
   - Get testnet STX from faucets:
     - [Stacks Explorer Faucet](https://explorer.stacks.co/sandbox/faucet?chain=testnet)
     - [Hiro Explorer Faucet](https://explorer.hiro.so/sandbox/faucet?chain=testnet)

### Current Deployment Status

**Successfully deployed on Stacks Testnet** 

**Contract Addresses:**
- **AMM Contract**: `ST1KQ3KDWYE3B4WMY0WQ7SP7EYX0842PR64K3DBE2.amm`
- **Mock Token**: `ST1KQ3KDWYE3B4WMY0WQ7SP7EYX0842PR64K3DBE2.mock-token`
- **Mock Token 2**: `ST1KQ3KDWYE3B4WMY0WQ7SP7EYX0842PR64K3DBE2.mock-token-2`

**Deployment Details:**
- **Network**: Stacks Testnet
- **Deployer Address**: `ST371ZDY8AP0B5H4F3RTWWSWGBAXS87YKTB34RPHZ`
- **Status**: Active and fully functional
- **Frontend Integration**: Contract addresses synchronized
- **Last Updated**: August 16, 2025

## Configuration

### Clarinet Configuration (`Clarinet.toml`)

```toml
[project]
name = 'amm'
description = 'Pasifika Stacks Exchange AMM'
authors = []
telemetry = true
cache_dir = './.cache'

[contracts.amm]
path = 'contracts/amm.clar'
clarity_version = 3
epoch = 3.0
```

### Contract Constants

- `MINIMUM_LIQUIDITY`: 1000 (prevents dust attacks)
- `FEES_DENOM`: 10000 (fee denominator for percentage calculations)

## API Reference

### Core Functions

#### `create-pool`
Creates a new liquidity pool for a token pair.

```clarity
(create-pool (token-0 <ft-trait>) (token-1 <ft-trait>) (fee uint))
```

**Parameters:**
- `token-0`: First token contract
- `token-1`: Second token contract  
- `fee`: Fee in basis points (e.g., 500 = 0.5%)

#### `add-liquidity`
Adds liquidity to an existing pool.

```clarity
(add-liquidity (token-0 <ft-trait>) (token-1 <ft-trait>) (fee uint) 
               (amount-0-desired uint) (amount-1-desired uint) 
               (amount-0-min uint) (amount-1-min uint))
```

#### `remove-liquidity`
Removes liquidity from a pool.

```clarity
(remove-liquidity (token-0 <ft-trait>) (token-1 <ft-trait>) (fee uint) (liquidity uint))
```

#### `swap`
Executes a token swap.

```clarity
(swap (token-0 <ft-trait>) (token-1 <ft-trait>) (fee uint) 
      (input-amount uint) (zero-for-one bool))
```

### Read-Only Functions

#### `get-pool-data`
Returns pool information for a given pool ID.

#### `get-pool-id`
Generates pool ID from token pair and fee.

#### `get-position-liquidity`
Returns user's liquidity position in a specific pool.

## Frontend Integration

### Contract Address Synchronization

After successful deployment, contract addresses are automatically synchronized with the frontend:

```bash
# Run from frontend directory
node scripts/save-contract-addresses.js
```

**Generated Files:**
- `deployed_contracts/contract-addresses.json` - Contract metadata and addresses
- `deployed_contracts/contract-addresses.ts` - TypeScript definitions with helper functions
- `deployed_contracts/deployment-summary.json` - Deployment summary and statistics

**Features:**
- Automatic contract discovery from deployment plans
- Type-safe contract address access
- Network-aware contract resolution
- Helper functions for contract principal generation

### Frontend Integration

The AMM is fully integrated with the Pasifika Web3 frontend:

**Integration Details:**
- **Frontend Repository**: `pasifika-web3-fe`
- **Contract Files**: Auto-generated in `deployed_contracts/`
- **Development Server**: `http://localhost:3000`
- **Wallet Integration**: Stacks Connect (Hiro Wallet, Xverse)
- **Network**: Configured for Stacks Testnet

**Testing the Integration:**
```bash
# Start frontend development server
cd ../pasifika-web3-fe
npm run dev
```

## Security Considerations

### Smart Contract Security
- **Reentrancy Protection**: Uses check-effects-interactions pattern
- **Integer Overflow Protection**: Clarity's built-in overflow protection
- **Access Control**: Proper permission checks for sensitive operations
- **Input Validation**: Comprehensive parameter validation

### Testing Security
- **Unit Tests**: 100% function coverage
- **Edge Case Testing**: Boundary condition testing
- **Error Handling**: Comprehensive error scenario testing

## Contributing

### Development Workflow

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make changes and test**
   ```bash
   npm test
   clarinet check
   ```

4. **Commit and push**
   ```bash
   git commit -m "Add your feature"
   git push origin feature/your-feature-name
   ```

5. **Create a Pull Request**

### Code Standards

- **Clarity Style**: Follow Clarity best practices
- **Testing**: All new features must include tests
- **Documentation**: Update README for significant changes
- **Comments**: Document complex logic in contracts

## Project Structure

```
pasifika-stacks-exchange/
├── contracts/
│   ├── amm.clar              # Main AMM contract
│   └── mock-token.clar       # Test token contract
├── tests/
│   ├── amm.test.ts           # AMM contract tests
│   └── mock-token.test.ts    # Token contract tests
├── deployments/
│   ├── default.simnet-plan.yaml    # Simnet deployment
│   ├── default.devnet-plan.yaml    # Devnet deployment
│   └── default.testnet-plan.yaml   # Testnet deployment
├── settings/
│   ├── Devnet.toml           # Devnet configuration
│   └── Testnet.toml          # Testnet configuration (gitignored)
├── Clarinet.toml             # Project configuration
├── package.json              # Node.js dependencies
├── tsconfig.json             # TypeScript configuration
├── vitest.config.js          # Test configuration
└── README.md                 # This file
```

## Network Information

### Stacks Testnet
- **Network**: Stacks Testnet
- **Explorer**: https://explorer.hiro.so/?chain=testnet
- **API**: https://api.testnet.hiro.so/

### Required Traits
- **SIP-010**: `SP3FBR2AGK5H9QBDH3EEN6DF8EK8JY7RX8QJ5SVTE.sip-010-trait-ft-standard`

## Roadmap

### Current Status: Proof of Concept 
- [x] Core AMM functionality
- [x] Pool creation and management
- [x] Liquidity operations
- [x] Token swapping
- [x] Testnet deployment
- [x] Frontend integration

### Future Enhancements
- [ ] Multi-hop swapping
- [ ] Flash loans
- [ ] Governance token integration
- [ ] Advanced fee structures
- [ ] Mainnet deployment
- [ ] Audit and security review

## Support

### Getting Help
- **Issues**: Create GitHub issues for bugs or feature requests
- **Documentation**: Check this README and inline code comments
- **Community**: Join Pasifika Web3 Tech Hub discussions

### Common Issues

**Q: Tests failing with "Contract not found"**
A: Run `clarinet check` to verify contract syntax first.

**Q: Deployment fails**
A: Ensure you have sufficient STX tokens for deployment fees.

**Q: Frontend can't connect to contracts**
A: Run the contract address sync script from the frontend directory:
```bash
cd ../pasifika-web3-fe
node scripts/save-contract-addresses.js
```

**Q: Deployment plan has wrong wallet address**
A: Regenerate the deployment plan with the correct cost strategy:
```bash
clarinet deployment generate --testnet --low-cost
```




