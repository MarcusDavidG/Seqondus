# Setup Guide

This guide will help you set up your development environment for working with Clarity smart contracts in this repository.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Development Tools](#development-tools)
- [Running Tests](#running-tests)
- [Using Gitpod](#using-gitpod)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before you begin, ensure you have the following installed on your system:

### Required Software

1. **Node.js** (v16 or higher)
   - Download from [nodejs.org](https://nodejs.org/)
   - Verify installation:
     ```bash
     node --version
     npm --version
     ```

2. **Yarn** Package Manager
   - Install via npm:
     ```bash
     npm install -g yarn
     ```
   - Verify installation:
     ```bash
     yarn --version
     ```

3. **Clarinet** - Clarity Development Tool
   - Install on macOS/Linux:
     ```bash
     curl -L https://github.com/hirosystems/clarinet/releases/latest/download/clarinet-linux-x64.tar.gz | tar xz
     sudo mv clarinet /usr/local/bin/
     ```
   - Install on Windows:
     ```bash
     # Download from GitHub releases
     # https://github.com/hirosystems/clarinet/releases
     ```
   - Verify installation:
     ```bash
     clarinet --version
     ```

4. **Git**
   - Download from [git-scm.com](https://git-scm.com/)
   - Verify installation:
     ```bash
     git --version
     ```

### Recommended Software

1. **Visual Studio Code**
   - Download from [code.visualstudio.com](https://code.visualstudio.com/)
   - Recommended extensions:
     - Clarity (by Hiro Systems)
     - Clarity LSP
     - GitLens
     - ESLint
     - Prettier

2. **GitHub CLI** (optional, for PR management)
   ```bash
   # macOS
   brew install gh
   
   # Linux
   # See: https://github.com/cli/cli/blob/trunk/docs/install_linux.md
   
   # Windows
   # Download from: https://github.com/cli/cli/releases
   ```

## Installation

### 1. Clone the Repository

```bash
# Clone the repository
git clone https://github.com/friedger/clarity-smart-contracts.git
cd clarity-smart-contracts
```

### 2. Install Dependencies

```bash
# Install Node.js dependencies
yarn install
```

This will install:
- `@hirosystems/clarinet-sdk` - Clarinet SDK for testing
- `@stacks/transactions` - Stacks transaction library
- `vitest` - Testing framework
- TypeScript and other development dependencies

### 3. Verify Installation

Run the test suite to ensure everything is set up correctly:

```bash
yarn test
```

If all tests pass, your environment is ready! ✅

## Project Structure

Understanding the project structure will help you navigate the codebase:

```
clarity-smart-contracts/
├── contracts/              # Smart contracts
│   ├── experiments/        # Experimental contracts
│   │   ├── flip-coin.clar
│   │   ├── animal-kingdom.clar
│   │   └── ...
│   ├── tokens/            # Token contracts
│   │   ├── fungible-token.clar
│   │   ├── non-fungible-token.clar
│   │   └── ...
│   ├── rockets/           # Rocket system contracts
│   │   ├── rocket-token.clar
│   │   ├── rocket-factory.clar
│   │   └── rocket-market.clar
│   ├── license/           # License management
│   │   └── license.clar
│   └── sips/              # SIP trait implementations
│       ├── ft-trait.clar
│       └── nft-trait.clar
├── test/                  # Test files
│   ├── clarunit.test.ts
│   ├── panic.test.ts
│   └── ...
├── test-old/              # Legacy tests
│   ├── unit/
│   └── integration/
├── docs/                  # Documentation
│   ├── escrow.md
│   ├── flip-coin.md
│   └── ...
├── scripts/               # Deployment scripts
├── settings/              # Network settings
├── Clarinet.toml          # Clarinet configuration
├── package.json           # Node.js dependencies
└── vitest.config.js       # Test configuration
```

### Key Files

- **Clarinet.toml**: Defines all contracts and their dependencies
- **package.json**: Node.js dependencies and scripts
- **vitest.config.js**: Test configuration for Vitest
- **tsconfig.json**: TypeScript configuration

## Development Tools

### Clarinet Commands

Clarinet is the primary tool for Clarity development:

```bash
# Check contract syntax
clarinet check

# Open Clarity REPL
clarinet console

# Run Clarinet tests
clarinet test

# Generate deployment plan
clarinet deployment generate --testnet

# Deploy to testnet
clarinet deployment apply --testnet
```

### NPM Scripts

The project includes several useful npm scripts:

```bash
# Run all tests
yarn test

# Run tests with coverage and cost reports
yarn test:report

# Run tests in watch mode (auto-rerun on file changes)
yarn test:watch
```

### VS Code Integration

If using VS Code, you can:

1. **Run tests from the editor:**
   - Open the Testing sidebar (beaker icon)
   - Run individual tests or test suites

2. **Debug tests:**
   - Set breakpoints in test files
   - Use the Debug panel to run tests with debugging

3. **Clarity syntax highlighting:**
   - Install the Clarity extension
   - Get syntax highlighting and basic IntelliSense

## Running Tests

### Unit Tests

Unit tests run in the Clarinet simnet environment:

```bash
# Run all tests
yarn test

# Run specific test file
yarn vitest run test/clarunit.test.ts

# Run tests in watch mode
yarn test:watch
```

### Test Structure

Tests use Vitest and the Clarinet SDK:

```typescript
import { describe, expect, it } from "vitest";

describe("contract-name", () => {
  it("should perform expected behavior", () => {
    const result = simnet.callPublicFn(
      "contract-name",
      "function-name",
      [Cl.uint(100)],
      accounts.get("wallet_1")!
    );
    
    expect(result.result).toBeOk(Cl.bool(true));
  });
});
```

### Coverage Reports

Generate test coverage reports:

```bash
yarn test:report
```

This creates a coverage report showing:
- Line coverage
- Function coverage
- Branch coverage
- Cost analysis for contract calls

## Using Gitpod

This repository is Gitpod-ready for cloud-based development:

### Quick Start with Gitpod

1. **Open in Gitpod:**
   - Click the Gitpod button in the README
   - Or visit: `https://gitpod.io/#https://github.com/friedger/clarity-smart-contracts`

2. **Automatic Setup:**
   - Gitpod automatically installs all dependencies
   - Environment is ready in minutes
   - No local installation required

3. **Features:**
   - VS Code in the browser
   - Pre-configured environment
   - Integrated terminal
   - Git integration

### Gitpod Configuration

The repository includes:
- `.gitpod.yml`: Gitpod configuration
- `.gitpod.Dockerfile`: Custom Docker image

## Working with Contracts

### Creating a New Contract

1. **Create the contract file:**
   ```bash
   touch contracts/my-contract.clar
   ```

2. **Add to Clarinet.toml:**
   ```toml
   [contracts.my-contract]
   path = "contracts/my-contract.clar"
   ```

3. **Write your contract:**
   ```clarity
   ;; My Contract
   ;; Description of what it does
   
   (define-public (my-function)
     (ok true))
   ```

4. **Create tests:**
   ```bash
   touch test/my-contract.test.ts
   ```

5. **Run tests:**
   ```bash
   yarn test
   ```

### Deploying Contracts

#### To Testnet

1. **Generate deployment plan:**
   ```bash
   clarinet deployment generate --testnet
   ```

2. **Review the plan:**
   - Check `deployments/default.testnet-plan.yaml`

3. **Deploy:**
   ```bash
   clarinet deployment apply --testnet
   ```

#### To Mainnet

⚠️ **Warning**: Always test thoroughly on testnet first!

1. **Generate mainnet plan:**
   ```bash
   clarinet deployment generate --mainnet
   ```

2. **Review carefully**

3. **Deploy:**
   ```bash
   clarinet deployment apply --mainnet
   ```

## Environment Configuration

### Network Settings

Network configurations are in `settings/`:

- `settings/Devnet.toml`: Local development network
- Configure accounts, balances, and network parameters

### Keys Management

⚠️ **Security Warning**: Never commit private keys to version control!

- `keys.json` and `keys2.json` are for local testing only
- Use environment variables for production keys
- Add sensitive files to `.gitignore`

## Troubleshooting

### Common Issues

#### 1. Clarinet Not Found

**Problem:** `clarinet: command not found`

**Solution:**
```bash
# Verify Clarinet is in PATH
which clarinet

# If not found, reinstall or add to PATH
export PATH=$PATH:/path/to/clarinet
```

#### 2. Node Version Issues

**Problem:** Tests fail with Node.js errors

**Solution:**
```bash
# Check Node version
node --version

# Should be v16 or higher
# Use nvm to switch versions:
nvm install 16
nvm use 16
```

#### 3. Dependency Installation Fails

**Problem:** `yarn install` fails

**Solution:**
```bash
# Clear cache and reinstall
rm -rf node_modules
rm yarn.lock
yarn cache clean
yarn install
```

#### 4. Tests Fail to Run

**Problem:** Tests don't execute

**Solution:**
```bash
# Verify Clarinet configuration
clarinet check

# Rebuild dependencies
yarn install

# Run with verbose output
yarn test --reporter=verbose
```

#### 5. Contract Syntax Errors

**Problem:** Contract won't compile

**Solution:**
```bash
# Check syntax with Clarinet
clarinet check

# Review error messages carefully
# Common issues:
# - Missing parentheses
# - Incorrect function signatures
# - Type mismatches
```

### Getting Help

If you encounter issues:

1. **Check Documentation:**
   - Review this guide
   - Check [CONTRIBUTING.md](CONTRIBUTING.md)
   - Read contract-specific docs in `docs/`

2. **Search Issues:**
   - Check [GitHub Issues](https://github.com/friedger/clarity-smart-contracts/issues)
   - Look for similar problems

3. **Ask for Help:**
   - Open a new issue with the `question` label
   - Provide error messages and steps to reproduce
   - Include your environment details

4. **Community Resources:**
   - [Stacks Discord](https://discord.gg/stacks)
   - [Stacks Forum](https://forum.stacks.org/)
   - [Clarity Documentation](https://docs.stacks.co/clarity/)

## Next Steps

Now that your environment is set up:

1. **Explore the contracts** in the `contracts/` directory
2. **Read the documentation** in `docs/`
3. **Run the tests** to see how contracts work
4. **Try modifying** a contract and running tests
5. **Create your own** contract following the patterns you see

For more information:
- [CONTRIBUTING.md](CONTRIBUTING.md) - How to contribute
- [TESTING.md](TESTING.md) - Detailed testing guide
- [ARCHITECTURE.md](ARCHITECTURE.md) - Project architecture

Happy coding! 🚀
