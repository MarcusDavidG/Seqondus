# Contributing to Clarity Smart Contracts

Thank you for your interest in contributing to this Clarity smart contracts collection! This document provides guidelines and instructions for contributing.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Pull Request Process](#pull-request-process)
- [Documentation](#documentation)
- [Community](#community)

## Code of Conduct

This project adheres to a Code of Conduct that all contributors are expected to follow. Please read [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) before contributing.

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v16 or higher)
- [Yarn](https://yarnpkg.com/) package manager
- [Clarinet](https://github.com/hirosystems/clarinet) - Clarity development tool
- [Git](https://git-scm.com/)
- A code editor (VS Code recommended with Clarity extensions)

### Setting Up Your Development Environment

1. **Fork the repository** on GitHub

2. **Clone your fork:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/clarity-smart-contracts.git
   cd clarity-smart-contracts
   ```

3. **Add upstream remote:**
   ```bash
   git remote add upstream https://github.com/friedger/clarity-smart-contracts.git
   ```

4. **Install dependencies:**
   ```bash
   yarn install
   ```

5. **Verify your setup:**
   ```bash
   yarn test
   ```

For detailed setup instructions, see [SETUP.md](SETUP.md).

## How to Contribute

### Types of Contributions

We welcome various types of contributions:

- **Bug fixes** - Fix issues in existing contracts
- **New contracts** - Add new smart contract implementations
- **Documentation** - Improve or add documentation
- **Tests** - Add or improve test coverage
- **Examples** - Provide usage examples
- **Performance improvements** - Optimize contract efficiency
- **Security improvements** - Enhance contract security

### Finding Issues to Work On

- Check the [Issues](https://github.com/friedger/clarity-smart-contracts/issues) page
- Look for issues labeled `good first issue` or `help wanted`
- Feel free to create new issues for bugs or feature requests

## Development Workflow

### 1. Create a Branch

Create a new branch for your work:

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-fix
```

Branch naming conventions:
- `feature/` - New features or contracts
- `fix/` - Bug fixes
- `docs/` - Documentation updates
- `test/` - Test additions or improvements
- `refactor/` - Code refactoring

### 2. Make Your Changes

- Write clean, readable code
- Follow the coding standards (see below)
- Add tests for new functionality
- Update documentation as needed
- Keep commits focused and atomic

### 3. Test Your Changes

Run the test suite to ensure everything works:

```bash
# Run all tests
yarn test

# Run tests with coverage
yarn test:report

# Run tests in watch mode
yarn test:watch
```

### 4. Commit Your Changes

Follow our commit message guidelines (see below).

### 5. Push and Create a Pull Request

```bash
git push origin your-branch-name
```

Then create a Pull Request on GitHub.

## Coding Standards

### Clarity Code Style

1. **Naming Conventions:**
   - Use kebab-case for function names: `get-balance`, `transfer-token`
   - Use descriptive names that indicate purpose
   - Constants should be in UPPER-KEBAB-CASE: `MAX-SUPPLY`, `CONTRACT-OWNER`

2. **Comments:**
   - Add comments for complex logic
   - Document public functions with their purpose, parameters, and return values
   - Include contract-level documentation at the top of each file

3. **Structure:**
   - Group related functions together
   - Place constants at the top
   - Define data structures before functions
   - Order: constants → data vars → maps → private functions → public functions

4. **Error Handling:**
   - Use descriptive error codes
   - Define error constants at the top of the contract
   - Always handle potential errors with `match` or `unwrap` variants

### Example Contract Structure:

```clarity
;; Contract Title
;; Brief description of what the contract does
;;
;; For more details see docs/contract-name.md

;; Constants
(define-constant CONTRACT-OWNER tx-sender)
(define-constant ERR-NOT-AUTHORIZED (err u100))
(define-constant ERR-INVALID-AMOUNT (err u101))

;; Data Variables
(define-data-var total-supply uint u0)

;; Data Maps
(define-map balances principal uint)

;; Private Functions
(define-private (is-owner)
  (is-eq tx-sender CONTRACT-OWNER))

;; Public Functions
(define-public (transfer (amount uint) (recipient principal))
  ;; Function implementation
  (ok true))

;; Read-only Functions
(define-read-only (get-balance (account principal))
  (default-to u0 (map-get? balances account)))
```

### TypeScript/JavaScript Code Style

- Use TypeScript for test files
- Follow standard ESLint rules
- Use async/await for asynchronous operations
- Add type annotations where helpful

## Testing Guidelines

### Writing Tests

1. **Test Coverage:**
   - Write tests for all public functions
   - Test both success and failure cases
   - Test edge cases and boundary conditions

2. **Test Structure:**
   - Use descriptive test names
   - Follow Arrange-Act-Assert pattern
   - Keep tests focused and independent

3. **Test Files:**
   - Place unit tests in `test/` directory
   - Name test files with `.test.ts` extension
   - Mirror the contract structure in test organization

### Example Test:

```typescript
import { describe, expect, it } from "vitest";

describe("fungible-token contract", () => {
  it("should transfer tokens successfully", () => {
    // Arrange
    const sender = accounts.get("wallet_1")!;
    const recipient = accounts.get("wallet_2")!;
    const amount = 100;

    // Act
    const result = simnet.callPublicFn(
      "fungible-token",
      "transfer",
      [Cl.uint(amount), Cl.principal(recipient)],
      sender
    );

    // Assert
    expect(result.result).toBeOk(Cl.bool(true));
  });
});
```

For more testing details, see [TESTING.md](TESTING.md).

## Commit Message Guidelines

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

### Format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types:

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `test`: Adding or updating tests
- `refactor`: Code refactoring
- `style`: Code style changes (formatting, etc.)
- `chore`: Maintenance tasks
- `perf`: Performance improvements

### Examples:

```
feat(tokens): add perishable token contract

Implement a token that expires after a certain time period.
Includes transfer restrictions based on expiration.

Closes #123
```

```
fix(escrow): correct balance calculation

Fixed an issue where the balance was not properly updated
after a successful transfer.
```

```
docs(readme): add setup instructions

Added detailed setup instructions for new contributors.
```

## Pull Request Process

### Before Submitting

1. ✅ Ensure all tests pass
2. ✅ Update documentation if needed
3. ✅ Add tests for new functionality
4. ✅ Follow coding standards
5. ✅ Rebase on latest main branch
6. ✅ Write clear commit messages

### PR Description Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Performance improvement
- [ ] Other (please describe)

## Testing
Describe the tests you ran and how to reproduce them

## Checklist
- [ ] My code follows the project's style guidelines
- [ ] I have performed a self-review of my code
- [ ] I have commented my code where necessary
- [ ] I have updated the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix/feature works
- [ ] New and existing tests pass locally

## Related Issues
Closes #(issue number)
```

### Review Process

1. A maintainer will review your PR
2. Address any requested changes
3. Once approved, your PR will be merged
4. Your contribution will be credited in the changelog

### After Your PR is Merged

1. Delete your branch (optional)
2. Pull the latest changes from main
3. Celebrate! 🎉

## Documentation

Good documentation is crucial. When contributing:

- Update relevant documentation files
- Add inline code comments for complex logic
- Create new documentation for new contracts
- Include usage examples
- Update the README if adding new features

Documentation files are located in:
- `docs/` - Contract-specific documentation
- `docs/guides/` - Developer guides
- Root level - Core documentation (README, CONTRIBUTING, etc.)

## Community

### Getting Help

- **GitHub Issues** - For bug reports and feature requests
- **Discussions** - For questions and general discussion
- **Documentation** - Check existing docs first

### Recognition

Contributors are recognized in:
- GitHub contributors list
- CHANGELOG.md for significant contributions
- Project README for major features

## Questions?

If you have questions about contributing, feel free to:
- Open an issue with the `question` label
- Start a discussion on GitHub Discussions
- Review existing documentation

Thank you for contributing to Clarity Smart Contracts! 🚀
