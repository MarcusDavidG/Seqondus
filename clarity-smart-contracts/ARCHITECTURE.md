# Architecture Documentation

This document provides an overview of the project architecture, design patterns, and organizational structure of the Clarity smart contracts collection.

## Table of Contents

- [Project Overview](#project-overview)
- [Directory Structure](#directory-structure)
- [Contract Categories](#contract-categories)
- [Design Patterns](#design-patterns)
- [Dependencies](#dependencies)
- [Development Workflow](#development-workflow)
- [Testing Architecture](#testing-architecture)
- [Deployment Strategy](#deployment-strategy)

## Project Overview

This repository is a comprehensive collection of Clarity smart contracts demonstrating various patterns, use cases, and implementations on the Stacks blockchain.

### Goals

- **Educational**: Provide examples of Clarity contract patterns
- **Experimental**: Test new ideas and approaches
- **Reusable**: Offer building blocks for other projects
- **Well-tested**: Maintain high test coverage
- **Documented**: Clear documentation for all contracts

### Technology Stack

- **Language**: Clarity (smart contracts)
- **Development Tool**: Clarinet
- **Testing Framework**: Vitest + Clarinet SDK
- **Runtime**: TypeScript/Node.js (for tests and scripts)
- **Blockchain**: Stacks 2.0

## Directory Structure

```
clarity-smart-contracts/
│
├── contracts/                    # Smart contract source files
│   ├── experiments/              # Experimental contracts
│   ├── tokens/                   # Token implementations
│   ├── rockets/                  # Rocket system contracts
│   ├── license/                  # License management
│   └── sips/                     # SIP trait implementations
│
├── test/                         # Modern test suite
│   ├── clarunit.test.ts          # Clarunit tests
│   ├── panic.test.ts             # Panic contract tests
│   └── *.test.ts                 # Other test files
│
├── test-old/                     # Legacy tests (deprecated)
│   ├── unit/                     # Old unit tests
│   └── integration/              # Old integration tests
│
├── docs/                         # Documentation
│   ├── escrow.md                 # Escrow contract docs
│   ├── flip-coin.md              # Flip coin docs
│   ├── tokens/                   # Token documentation
│   ├── rockets/                  # Rocket system docs
│   └── guides/                   # Developer guides
│
├── scripts/                      # Deployment and utility scripts
│   ├── deploy.ts                 # General deployment
│   ├── flip-coin-deploy-all.ts  # Flip coin deployment
│   └── *.ts                      # Other scripts
│
├── settings/                     # Network configurations
│   └── Devnet.toml               # Devnet settings
│
├── deployments/                  # Deployment plans
│   └── default.simnet-plan.yaml # Simnet deployment plan
│
├── src/                          # Client libraries
│   └── client/                   # Contract interaction clients
│
├── owned-profiles/               # Sub-project for profiles
│   ├── contracts/                # Profile contracts
│   ├── tests/                    # Profile tests
│   └── src/                      # Profile client code
│
├── Clarinet.toml                 # Clarinet configuration
├── package.json                  # Node.js dependencies
├── vitest.config.js              # Test configuration
└── tsconfig.json                 # TypeScript configuration
```

## Contract Categories

### 1. Experiments (`contracts/experiments/`)

Experimental contracts exploring various concepts:

#### Flip Coin System
- **flip-coin.clar**: Random boolean generation based on block hash
- **flip-coin-jackpot.clar**: Betting game with jackpot mechanism
- **flip-coin-at-two.clar**: Two-player betting game
- **flip-coin-tax-office.clar**: Tax trait for gambling contracts
- **flip-coin-delegate.clar**: Delegation pattern for flip coin

**Pattern**: Random number generation, betting mechanics, trait implementation

#### Data Structures
- **kv-store.clar**: Key-value storage implementation
- **lists.clar**: List manipulation examples
- **history.clar**: Historical data tracking

**Pattern**: Data structure implementations in Clarity

#### Registry Contracts
- **animal-kingdom.clar**: Animal registry with properties
- **company-registry.clar**: Company registration system
- **todo-registry.clar**: Todo list management

**Pattern**: Registry pattern with CRUD operations

#### Other Experiments
- **panic.clar**: Error handling demonstrations
- **trait-conversion.clar**: Trait conversion examples
- **trait-conversion-router.clar**: Routing with trait conversion

### 2. Tokens (`contracts/tokens/`)

Token implementations following various standards:

#### Fungible Tokens
- **fungible-token.clar**: Basic FT implementation (ERC-20 style)
- **hodl-token.clar**: Token with holding incentives
- **perishable-token.clar**: Token that expires over time
- **beeple.clar**: Named token implementation

**Pattern**: SIP-010 fungible token standard

#### Non-Fungible Tokens
- **non-fungible-token.clar**: Basic NFT implementation
- **friedger-pool-nft.clar**: Pool-based NFT system

**Pattern**: SIP-009 non-fungible token standard

#### Escrow
- **escrow.clar**: Simple escrow for token transfers

**Pattern**: Third-party custody and conditional transfers

### 3. Rockets (`contracts/rockets/`)

A complete NFT marketplace system:

- **rocket-token.clar**: Fungible token for the rocket ecosystem
- **rocket-factory.clar**: NFT minting factory
- **rocket-market.clar**: Marketplace for trading rocket NFTs

**Pattern**: Factory pattern, marketplace mechanics, multi-contract system

### 4. License (`contracts/license/`)

- **license.clar**: License management for UI applications

**Pattern**: Access control, subscription management

### 5. SIPs (`contracts/sips/`)

Standard trait definitions:

- **ft-trait.clar**: Fungible token trait (SIP-010)
- **nft-trait.clar**: Non-fungible token trait (SIP-009)

**Pattern**: Interface definitions for standard compliance

## Design Patterns

### 1. Registry Pattern

Used in: animal-kingdom, company-registry, todo-registry

```clarity
;; Define a map for storing entities
(define-map entities
  {id: uint}
  {name: (string-ascii 50), data: uint})

;; Register new entity
(define-public (register (name (string-ascii 50)))
  (let ((id (+ (var-get next-id) u1)))
    (map-set entities {id: id} {name: name, data: u0})
    (var-set next-id id)
    (ok id)))

;; Get entity
(define-read-only (get-entity (id uint))
  (map-get? entities {id: id}))
```

### 2. Factory Pattern

Used in: rocket-factory

```clarity
;; Mint new NFT
(define-public (mint-nft)
  (let ((token-id (+ (var-get last-token-id) u1)))
    (try! (nft-mint? nft-asset token-id tx-sender))
    (var-set last-token-id token-id)
    (ok token-id)))
```

### 3. Marketplace Pattern

Used in: rocket-market

```clarity
;; List NFT for sale
(define-public (list-nft (token-id uint) (price uint))
  (begin
    (asserts! (is-eq (nft-get-owner? nft-asset token-id) (some tx-sender)) 
              ERR-NOT-OWNER)
    (map-set listings {token-id: token-id} {price: price, seller: tx-sender})
    (ok true)))

;; Buy listed NFT
(define-public (buy-nft (token-id uint))
  (let ((listing (unwrap! (map-get? listings {token-id: token-id}) ERR-NOT-LISTED)))
    (try! (stx-transfer? (get price listing) tx-sender (get seller listing)))
    (try! (nft-transfer? nft-asset token-id (get seller listing) tx-sender))
    (map-delete listings {token-id: token-id})
    (ok true)))
```

### 4. Escrow Pattern

Used in: escrow

```clarity
;; Deposit funds
(define-public (deposit (amount uint))
  (begin
    (try! (stx-transfer? amount tx-sender (as-contract tx-sender)))
    (var-set balance (+ (var-get balance) amount))
    (ok true)))

;; Release funds when conditions met
(define-public (release)
  (begin
    (asserts! (and (var-get buyer-ok) (var-get seller-ok)) ERR-NOT-APPROVED)
    (try! (as-contract (stx-transfer? (var-get balance) tx-sender seller)))
    (ok true)))
```

### 5. Trait Pattern

Used throughout for interfaces:

```clarity
;; Define trait
(define-trait token-trait
  (
    (transfer (uint principal principal) (response bool uint))
    (get-balance (principal) (response uint uint))
  ))

;; Implement trait
(impl-trait .token-trait.token-trait)

;; Use trait
(define-public (use-token (token <token-trait>))
  (contract-call? token transfer u100 tx-sender recipient))
```

### 6. Access Control Pattern

Used in: license, various admin functions

```clarity
;; Define owner
(define-constant CONTRACT-OWNER tx-sender)

;; Check ownership
(define-private (is-owner)
  (is-eq tx-sender CONTRACT-OWNER))

;; Protected function
(define-public (admin-function)
  (begin
    (asserts! (is-owner) ERR-NOT-AUTHORIZED)
    ;; Admin logic
    (ok true)))
```

### 7. State Machine Pattern

Used in: escrow (pending -> approved -> completed)

```clarity
;; Define states
(define-data-var state (string-ascii 20) "pending")

;; Transition function
(define-public (approve)
  (begin
    (asserts! (is-eq (var-get state) "pending") ERR-INVALID-STATE)
    (var-set state "approved")
    (ok true)))
```

## Dependencies

### Contract Dependencies

Defined in `Clarinet.toml`:

```toml
[contracts.flip-coin-at-two]
path = "contracts/experiments/flip-coin-at-two.clar"
depends_on = ["flip-coin", "flip-coin-tax-office"]
```

### External Dependencies

```toml
requirements = [
    { contract_id = "SP2PABAF9FTAJYNFZH93XENAJ8FVY99RRM50D2JG9.nft-trait" },
    { contract_id = "SP3FBR2AGK5H9QBDH3EEN6DF8EK8JY7RX8QJ5SVTE.sip-010-trait-ft-standard" },
]
```

### Node.js Dependencies

Key dependencies from `package.json`:

- `@hirosystems/clarinet-sdk`: Clarinet SDK for testing
- `@stacks/transactions`: Stacks transaction library
- `vitest`: Testing framework
- `typescript`: Type checking

## Development Workflow

### 1. Contract Development

```
Create Contract → Write Tests → Run Tests → Deploy to Testnet → Test on Testnet → Deploy to Mainnet
```

### 2. Testing Workflow

```
Write Test → Run Locally → Check Coverage → Fix Issues → Commit
```

### 3. Deployment Workflow

```
Generate Plan → Review Plan → Deploy to Testnet → Verify → Deploy to Mainnet
```

## Testing Architecture

### Test Structure

```
test/
├── clarunit.test.ts          # Clarunit framework tests
├── panic.test.ts             # Specific contract tests
└── [contract].test.ts        # Per-contract test files
```

### Test Layers

1. **Unit Tests**: Test individual functions in isolation
2. **Integration Tests**: Test contract interactions
3. **End-to-End Tests**: Test complete workflows

### Test Environment

- **Simnet**: Simulated blockchain environment
- **Accounts**: Pre-configured test accounts
- **Block Manipulation**: Control block height for testing

## Deployment Strategy

### Environments

1. **Local (Simnet)**: Development and testing
2. **Devnet**: Local network for integration testing
3. **Testnet**: Public test network
4. **Mainnet**: Production network

### Deployment Process

1. **Generate Deployment Plan**:
   ```bash
   clarinet deployment generate --testnet
   ```

2. **Review Plan**:
   - Check contract order
   - Verify initialization parameters
   - Review dependencies

3. **Deploy**:
   ```bash
   clarinet deployment apply --testnet
   ```

4. **Verify**:
   - Check contract deployment
   - Test contract functions
   - Verify state

### Deployment Plans

Located in `deployments/`:
- `default.simnet-plan.yaml`: Simnet deployment
- Generated plans for testnet/mainnet

## Contract Interaction Flow

### Example: Token Transfer

```
User → Frontend → Stacks.js → Stacks Node → Smart Contract → State Update → Event Emission
```

### Example: NFT Marketplace

```
1. Seller lists NFT (rocket-market.list-nft)
2. Buyer purchases NFT (rocket-market.buy-nft)
   ├── Transfer STX to seller
   ├── Transfer NFT to buyer
   └── Remove listing
3. Events emitted for indexing
```

## Security Considerations

### Access Control

- Owner checks for admin functions
- Caller verification for sensitive operations
- Trait-based permissions

### Input Validation

- Amount checks (> 0, <= max)
- Principal validation
- State validation

### Error Handling

- Explicit error codes
- Proper unwrapping
- State rollback on failure

## Future Architecture

### Planned Improvements

1. **Modular Contract System**: More reusable components
2. **Upgrade Patterns**: Contract upgradeability
3. **Cross-Contract Communication**: Better inter-contract calls
4. **Event System**: Comprehensive event emission
5. **Gas Optimization**: Reduce execution costs

### Scalability Considerations

- Efficient data structures
- Minimal storage usage
- Optimized function calls
- Batch operations where possible

## Contributing to Architecture

When adding new contracts:

1. Follow existing patterns
2. Document design decisions
3. Add comprehensive tests
4. Update this architecture document
5. Consider reusability

## Resources

- [Clarity Language Reference](https://docs.stacks.co/clarity/)
- [Stacks Documentation](https://docs.stacks.co/)
- [Clarinet Documentation](https://github.com/hirosystems/clarinet)
- [SIP Standards](https://github.com/stacksgov/sips)

## Questions?

For architecture-related questions:
1. Review this document
2. Check contract-specific documentation
3. Open an issue with the `architecture` label

---

This architecture is designed to be flexible, maintainable, and educational. Contributions that improve the architecture are welcome! 🏗️
