# Clarity Smart Contracts

[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/friedger/clarity-smart-contracts)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Stacks](https://img.shields.io/badge/Stacks-2.0-5546FF)](https://www.stacks.co/)
[![Clarity](https://img.shields.io/badge/Clarity-Smart%20Contracts-00D4FF)](https://clarity-lang.org/)

A comprehensive collection of smart contracts written in [Clarity](https://docs.stacks.co/clarity/) for the [Stacks blockchain](https://www.stacks.co/). This repository serves as an educational resource, experimental playground, and collection of reusable contract patterns.

## 📋 Table of Contents

- [Overview](#overview)
- [Quick Start](#quick-start)
- [Smart Contracts](#smart-contracts)
- [Documentation](#documentation)
- [Development](#development)
- [Testing](#testing)
- [Contributing](#contributing)
- [Community Resources](#community-resources)
- [License](#license)

## 🌟 Overview

This repository contains a diverse collection of Clarity smart contracts demonstrating:

- ✅ **Token Standards**: Fungible (SIP-010) and Non-Fungible (SIP-009) token implementations
- ✅ **DeFi Patterns**: Escrow, staking, and marketplace mechanics
- ✅ **Gaming Contracts**: Random number generation and betting systems
- ✅ **Registry Systems**: Data storage and management patterns
- ✅ **Access Control**: License and permission management
- ✅ **Advanced Patterns**: Traits, delegation, and contract composition

### Why This Repository?

- 📚 **Learn Clarity**: Study real-world contract examples
- 🔧 **Build Faster**: Use proven patterns and implementations
- 🧪 **Experiment**: Test new ideas in a well-structured environment
- 🤝 **Contribute**: Join a community of Clarity developers

## 🚀 Quick Start

### Prerequisites

- [Node.js](https://nodejs.org/) (v16+)
- [Yarn](https://yarnpkg.com/)
- [Clarinet](https://github.com/hirosystems/clarinet)

### Installation

```bash
# Clone the repository
git clone https://github.com/friedger/clarity-smart-contracts.git
cd clarity-smart-contracts

# Install dependencies
yarn install

# Run tests
yarn test
```

### Using Gitpod

Click the Gitpod badge above or visit:
```
https://gitpod.io/#https://github.com/friedger/clarity-smart-contracts
```

For detailed setup instructions, see [SETUP.md](SETUP.md).

## 📦 Smart Contracts

### Token Contracts (`contracts/tokens/`)

#### Fungible Tokens
- **[fungible-token.clar](contracts/tokens/fungible-token.clar)** - Basic fungible token (ERC-20 style)
  - Transfer, approve, and allowance functionality
  - SIP-010 compatible
  
- **[hodl-token.clar](contracts/tokens/hodl-token.clar)** - Token with holding incentives
  - Rewards for long-term holders
  - Time-locked transfers

- **[perishable-token.clar](contracts/tokens/perishable-token.clar)** - Expiring token
  - Time-based token expiration
  - Automatic value decay

- **[beeple.clar](contracts/tokens/beeple.clar)** - Named token implementation

#### Non-Fungible Tokens
- **[non-fungible-token.clar](contracts/tokens/non-fungible-token.clar)** - Basic NFT
  - Mint, transfer, and ownership tracking
  - SIP-009 compatible

- **[friedger-pool-nft.clar](contracts/tokens/friedger-pool-nft.clar)** - Pool-based NFT system

#### Escrow
- **[escrow.clar](contracts/tokens/escrow.clar)** - Simple escrow for token transfers
  - Third-party custody
  - Conditional release
  - [📖 Documentation](docs/escrow.md) | [🎥 Demo Video](https://www.youtube.com/watch?v=uZH1V-FNJIs)

### Experimental Contracts (`contracts/experiments/`)

#### Flip Coin System
A complete betting game system with multiple contracts:

- **[flip-coin.clar](contracts/experiments/flip-coin.clar)** - Random boolean generation
  - Uses block hash for randomness
  - Read-only random function

- **[flip-coin-jackpot.clar](contracts/experiments/flip-coin-jackpot.clar)** - Betting with jackpot
  - Progressive jackpot mechanism
  - Winner-takes-all gameplay

- **[flip-coin-at-two.clar](contracts/experiments/flip-coin-at-two.clar)** - Two-player betting
  - Head-to-head matches
  - Automatic payout

- **[flip-coin-tax-office.clar](contracts/experiments/flip-coin-tax-office.clar)** - Tax trait
  - Gambling tax implementation
  - Revenue distribution

- **[flip-coin-delegate.clar](contracts/experiments/flip-coin-delegate.clar)** - Delegation pattern

[📖 Full Documentation](docs/flip-coin.md)

#### Registry Contracts
- **[animal-kingdom.clar](contracts/experiments/animal-kingdom.clar)** - Animal registry with properties
- **[company-registry.clar](contracts/experiments/company-registry.clar)** - Company registration system
- **[todo-registry.clar](contracts/experiments/todo-registry.clar)** - Todo list management

#### Data Structures
- **[kv-store.clar](contracts/experiments/kv-store.clar)** - Key-value storage
- **[lists.clar](contracts/experiments/lists.clar)** - List manipulation examples
- **[history.clar](contracts/experiments/history.clar)** - Historical data tracking

#### Other Experiments
- **[panic.clar](contracts/experiments/panic.clar)** - Error handling demonstrations
- **[trait-conversion.clar](contracts/experiments/trait-conversion.clar)** - Trait conversion examples
- **[trait-conversion-router.clar](contracts/experiments/trait-conversion-router.clar)** - Routing with traits

### Rocket System (`contracts/rockets/`)

A complete NFT marketplace ecosystem:

- **[rocket-token.clar](contracts/rockets/rocket-token.clar)** - Fungible token for ecosystem
  - Buy tokens with STX
  - SIP-010 compatible

- **[rocket-factory.clar](contracts/rockets/rocket-factory.clar)** - NFT minting factory
  - Factory pattern implementation
  - Automated minting

- **[rocket-market.clar](contracts/rockets/rocket-market.clar)** - NFT marketplace
  - List and buy NFTs
  - Escrow-free trading

### License Management (`contracts/license/`)

- **[license.clar](contracts/license/license.clar)** - License management for applications
  - Subscription management
  - Expiring and perpetual licenses
  - Payment processing

### SIP Traits (`contracts/sips/`)

Standard trait definitions:

- **[ft-trait.clar](contracts/sips/ft-trait.clar)** - Fungible token trait (SIP-010)
- **[nft-trait.clar](contracts/sips/nft-trait.clar)** - Non-fungible token trait (SIP-009)

## 📚 Documentation

### Core Documentation
- [📖 SETUP.md](SETUP.md) - Detailed setup and installation guide
- [📖 CONTRIBUTING.md](CONTRIBUTING.md) - How to contribute to this project
- [📖 TESTING.md](TESTING.md) - Testing guide and best practices
- [📖 ARCHITECTURE.md](ARCHITECTURE.md) - Project architecture and design patterns
- [📖 SECURITY.md](SECURITY.md) - Security policy and best practices
- [📖 CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) - Community guidelines
- [📖 CHANGELOG.md](CHANGELOG.md) - Version history and changes

### Contract Documentation
- [📖 Escrow Contract](docs/escrow.md) - Simple token transfer via 3rd party
- [📖 Flip Coin Contract](docs/flip-coin.md) - Random coin flipping and betting games

### Developer Guides
- [📖 API Reference](docs/API.md) - Complete API documentation (coming soon)
- [📖 Deployment Guide](docs/guides/deploying-contracts.md) - How to deploy contracts (coming soon)
- [📖 Best Practices](docs/guides/clarity-best-practices.md) - Clarity coding standards (coming soon)

## 🛠️ Development

### Project Structure

```
clarity-smart-contracts/
├── contracts/          # Smart contract source files
│   ├── experiments/    # Experimental contracts
│   ├── tokens/        # Token implementations
│   ├── rockets/       # Rocket NFT system
│   ├── license/       # License management
│   └── sips/          # SIP trait definitions
├── test/              # Test files
├── docs/              # Documentation
├── scripts/           # Deployment scripts
└── settings/          # Network configurations
```

### Available Scripts

```bash
# Run all tests
yarn test

# Run tests with coverage and cost reports
yarn test:report

# Run tests in watch mode (auto-rerun on changes)
yarn test:watch

# Check contract syntax
clarinet check

# Open Clarity REPL
clarinet console
```

### Creating a New Contract

1. Create your contract file in the appropriate directory
2. Add it to `Clarinet.toml`
3. Write tests in `test/`
4. Run tests with `yarn test`
5. Document your contract

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## 🧪 Testing

This project uses [Vitest](https://vitest.dev/) with the [Clarinet SDK](https://github.com/hirosystems/clarinet) for testing.

### Running Tests

```bash
# Run all tests
yarn test

# Run specific test file
yarn vitest run test/clarunit.test.ts

# Generate coverage report
yarn test:report
```

### Writing Tests

```typescript
import { describe, expect, it } from "vitest";
import { Cl } from "@stacks/transactions";

describe("my-contract", () => {
  it("should perform expected behavior", () => {
    const result = simnet.callPublicFn(
      "my-contract",
      "my-function",
      [Cl.uint(100)],
      accounts.get("wallet_1")!
    );
    
    expect(result.result).toBeOk(Cl.bool(true));
  });
});
```

For comprehensive testing documentation, see [TESTING.md](TESTING.md).

## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. 🐛 **Report bugs** - Open an issue with details
2. 💡 **Suggest features** - Share your ideas
3. 📝 **Improve documentation** - Help others understand
4. 🔧 **Submit pull requests** - Add new contracts or fix issues

### Contribution Process

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Write or update tests
5. Commit your changes (`git commit -m 'Add amazing feature'`)
6. Push to your branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## 🌐 Community Resources

### Other Clarity Projects

- [Swapr](https://github.com/psq/swapr) - Trustless token exchange
- [Flexr](https://github.com/psq/flexr) - Elastic supply token
- [Stackstarter](https://github.com/MarvinJanssen/stackstarter) - Crowdfunding platform
- [Marketplace](https://github.com/friedger/clarity-marketplace) - NFT marketplace
- [Loopbomb Marketplace](https://github.com/radicleart/clarity-market) - Art NFT marketplace
- [Profit Sharing Token](https://github.com/friedger/clarity-profit-sharing-token) - Token with profit sharing
- [Blind Poll](https://github.com/zexxlin/clarity-blind-poll) - Anonymous polling
- [Stacks Loans](https://github.com/richardmichel/stacks-loans) - Fixed-rate loans

### Hackathon Winners

- [Clarity Hackathon 1 Winners](https://community.blockstack.org/clarity-winners#overall)
- [Stacks 2.0 Hackathon Winners](https://blog.blockstack.org/announcing-the-winners-of-clarity-hack/)

### Learning Resources

- [Clarity Language Reference](https://docs.stacks.co/clarity/)
- [Stacks Documentation](https://docs.stacks.co/)
- [Clarity Book](https://book.clarity-lang.org/)
- [Stacks Discord](https://discord.gg/stacks)
- [Stacks Forum](https://forum.stacks.org/)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2018 Friedger Müffke

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

## 🙏 Acknowledgments

- Original [Clarity tutorials](https://github.com/blockstack/clarity-js-sdk/tree/master/packages/clarity-tutorials) by Blockstack
- The Stacks community for continuous support and feedback
- All contributors who have helped improve this project

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/friedger/clarity-smart-contracts/issues)
- **Discussions**: [GitHub Discussions](https://github.com/friedger/clarity-smart-contracts/discussions)
- **Discord**: [Stacks Discord](https://discord.gg/stacks)

---

**⚠️ Disclaimer**: These contracts are provided for educational and experimental purposes. Always audit contracts thoroughly before using them in production. See [SECURITY.md](SECURITY.md) for more information.

**Made with ❤️ by the Clarity community**
