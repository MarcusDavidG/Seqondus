# Testing Guide

This guide covers testing practices, patterns, and tools for Clarity smart contracts in this repository.

## Table of Contents

- [Overview](#overview)
- [Testing Framework](#testing-framework)
- [Running Tests](#running-tests)
- [Writing Tests](#writing-tests)
- [Testing Patterns](#testing-patterns)
- [Best Practices](#best-practices)
- [Coverage and Reports](#coverage-and-reports)
- [Debugging Tests](#debugging-tests)
- [Examples](#examples)

## Overview

Testing is crucial for smart contract development. This project uses:

- **Vitest**: Modern testing framework
- **Clarinet SDK**: Simnet environment for contract testing
- **TypeScript**: Type-safe test code

### Why Test?

- ✅ Verify contract behavior
- ✅ Catch bugs early
- ✅ Document expected behavior
- ✅ Enable safe refactoring
- ✅ Build confidence in deployments

## Testing Framework

### Vitest

Vitest is a fast, modern testing framework with:
- Fast execution
- Watch mode for development
- Coverage reporting
- TypeScript support
- Compatible with Jest API

### Clarinet SDK

The Clarinet SDK provides:
- Simnet environment (simulated blockchain)
- Contract deployment and interaction
- Account management
- Block manipulation
- Cost analysis

## Running Tests

### Basic Commands

```bash
# Run all tests
yarn test

# Run tests with coverage and cost reports
yarn test:report

# Run tests in watch mode (auto-rerun on changes)
yarn test:watch

# Run specific test file
yarn vitest run test/clarunit.test.ts

# Run tests matching a pattern
yarn vitest run -t "transfer"
```

### Test Output

```bash
✓ test/clarunit.test.ts (5)
  ✓ fungible-token (3)
    ✓ should transfer tokens successfully
    ✓ should fail with insufficient balance
    ✓ should approve and transfer-from
  ✓ escrow (2)
    ✓ should deposit funds
    ✓ should complete escrow

Test Files  1 passed (1)
Tests  5 passed (5)
```

## Writing Tests

### Test File Structure

Create test files in the `test/` directory with `.test.ts` extension:

```typescript
import { describe, expect, it } from "vitest";

describe("contract-name", () => {
  it("should perform expected behavior", () => {
    // Test implementation
  });
});
```

### Basic Test Example

```typescript
import { describe, expect, it } from "vitest";
import { Cl } from "@stacks/transactions";

describe("fungible-token", () => {
  it("should get total supply", () => {
    const result = simnet.callReadOnlyFn(
      "fungible-token",
      "get-total-supply",
      [],
      accounts.get("deployer")!
    );
    
    expect(result.result).toBeOk(Cl.uint(300));
  });
});
```

### Test Anatomy

```typescript
it("should transfer tokens successfully", () => {
  // 1. ARRANGE - Set up test data
  const sender = accounts.get("wallet_1")!;
  const recipient = accounts.get("wallet_2")!;
  const amount = 100;
  
  // 2. ACT - Execute the function
  const result = simnet.callPublicFn(
    "fungible-token",
    "transfer-token",
    [Cl.uint(amount), Cl.principal(recipient)],
    sender
  );
  
  // 3. ASSERT - Verify the result
  expect(result.result).toBeOk(Cl.bool(true));
  
  // 4. VERIFY STATE - Check side effects
  const balance = simnet.callReadOnlyFn(
    "fungible-token",
    "get-balance",
    [Cl.principal(recipient)],
    sender
  );
  expect(balance.result).toBeOk(Cl.uint(amount));
});
```

## Testing Patterns

### 1. Testing Public Functions

```typescript
describe("public functions", () => {
  it("should call public function", () => {
    const caller = accounts.get("wallet_1")!;
    
    const result = simnet.callPublicFn(
      "contract-name",
      "function-name",
      [Cl.uint(100), Cl.principal(caller)],
      caller
    );
    
    expect(result.result).toBeOk(Cl.bool(true));
  });
});
```

### 2. Testing Read-Only Functions

```typescript
describe("read-only functions", () => {
  it("should read contract state", () => {
    const result = simnet.callReadOnlyFn(
      "contract-name",
      "get-value",
      [Cl.principal(accounts.get("wallet_1")!)],
      accounts.get("wallet_1")!
    );
    
    expect(result.result).toBeUint(100);
  });
});
```

### 3. Testing Error Cases

```typescript
describe("error handling", () => {
  it("should fail with insufficient balance", () => {
    const sender = accounts.get("wallet_1")!;
    const amount = 1000000; // More than balance
    
    const result = simnet.callPublicFn(
      "fungible-token",
      "transfer-token",
      [Cl.uint(amount), Cl.principal(accounts.get("wallet_2")!)],
      sender
    );
    
    expect(result.result).toBeErr(Cl.uint(1)); // ERR-INSUFFICIENT-BALANCE
  });
});
```

### 4. Testing Access Control

```typescript
describe("access control", () => {
  it("should only allow owner to call admin function", () => {
    const unauthorized = accounts.get("wallet_2")!;
    
    const result = simnet.callPublicFn(
      "contract-name",
      "admin-function",
      [],
      unauthorized
    );
    
    expect(result.result).toBeErr(Cl.uint(100)); // ERR-NOT-AUTHORIZED
  });
  
  it("should allow owner to call admin function", () => {
    const owner = accounts.get("deployer")!;
    
    const result = simnet.callPublicFn(
      "contract-name",
      "admin-function",
      [],
      owner
    );
    
    expect(result.result).toBeOk(Cl.bool(true));
  });
});
```

### 5. Testing State Changes

```typescript
describe("state changes", () => {
  it("should update balance after transfer", () => {
    const sender = accounts.get("wallet_1")!;
    const recipient = accounts.get("wallet_2")!;
    const amount = 50;
    
    // Get initial balances
    const initialSenderBalance = simnet.callReadOnlyFn(
      "fungible-token",
      "get-balance",
      [Cl.principal(sender)],
      sender
    );
    
    // Transfer
    simnet.callPublicFn(
      "fungible-token",
      "transfer-token",
      [Cl.uint(amount), Cl.principal(recipient)],
      sender
    );
    
    // Check final balances
    const finalSenderBalance = simnet.callReadOnlyFn(
      "fungible-token",
      "get-balance",
      [Cl.principal(sender)],
      sender
    );
    
    const finalRecipientBalance = simnet.callReadOnlyFn(
      "fungible-token",
      "get-balance",
      [Cl.principal(recipient)],
      sender
    );
    
    // Verify changes
    expect(finalSenderBalance.result).toBeOk(
      Cl.uint(Number(initialSenderBalance.result) - amount)
    );
    expect(finalRecipientBalance.result).toBeOk(Cl.uint(amount));
  });
});
```

### 6. Testing Events

```typescript
describe("events", () => {
  it("should emit transfer event", () => {
    const sender = accounts.get("wallet_1")!;
    const recipient = accounts.get("wallet_2")!;
    
    const result = simnet.callPublicFn(
      "fungible-token",
      "transfer-token",
      [Cl.uint(100), Cl.principal(recipient)],
      sender
    );
    
    expect(result.events).toHaveLength(1);
    expect(result.events[0].event).toBe("ft_transfer_event");
  });
});
```

### 7. Testing Multiple Transactions

```typescript
describe("multiple transactions", () => {
  it("should handle sequential transfers", () => {
    const wallet1 = accounts.get("wallet_1")!;
    const wallet2 = accounts.get("wallet_2")!;
    const wallet3 = accounts.get("wallet_3")!;
    
    // Transfer 1: wallet1 -> wallet2
    simnet.callPublicFn(
      "fungible-token",
      "transfer-token",
      [Cl.uint(50), Cl.principal(wallet2)],
      wallet1
    );
    
    // Transfer 2: wallet2 -> wallet3
    const result = simnet.callPublicFn(
      "fungible-token",
      "transfer-token",
      [Cl.uint(30), Cl.principal(wallet3)],
      wallet2
    );
    
    expect(result.result).toBeOk(Cl.bool(true));
    
    // Verify final state
    const wallet3Balance = simnet.callReadOnlyFn(
      "fungible-token",
      "get-balance",
      [Cl.principal(wallet3)],
      wallet1
    );
    expect(wallet3Balance.result).toBeOk(Cl.uint(30));
  });
});
```

### 8. Testing Block Height

```typescript
describe("block height dependent", () => {
  it("should behave differently at different heights", () => {
    // Mine blocks to advance height
    simnet.mineEmptyBlocks(10);
    
    const result = simnet.callReadOnlyFn(
      "flip-coin",
      "flip-coin-at",
      [Cl.uint(simnet.blockHeight)],
      accounts.get("wallet_1")!
    );
    
    expect(result.result).toBeBool(true); // or false, depending on hash
  });
});
```

## Best Practices

### 1. Test Organization

```typescript
describe("ContractName", () => {
  describe("initialization", () => {
    it("should initialize with correct values", () => {
      // Test initialization
    });
  });
  
  describe("public functions", () => {
    describe("transfer", () => {
      it("should transfer successfully", () => {});
      it("should fail with insufficient balance", () => {});
      it("should fail with invalid recipient", () => {});
    });
    
    describe("approve", () => {
      it("should approve spender", () => {});
      it("should fail with zero amount", () => {});
    });
  });
  
  describe("read-only functions", () => {
    it("should get balance", () => {});
    it("should get total supply", () => {});
  });
});
```

### 2. Use Descriptive Test Names

```typescript
// ❌ Bad
it("test1", () => {});

// ✅ Good
it("should transfer tokens from sender to recipient", () => {});
it("should fail when transferring more than balance", () => {});
it("should emit transfer event on successful transfer", () => {});
```

### 3. Test One Thing at a Time

```typescript
// ❌ Bad - Testing multiple things
it("should transfer and approve", () => {
  // Transfer test
  // Approve test
});

// ✅ Good - Separate tests
it("should transfer tokens", () => {
  // Only transfer test
});

it("should approve spender", () => {
  // Only approve test
});
```

### 4. Use Setup and Teardown

```typescript
describe("contract with setup", () => {
  let deployer: string;
  let wallet1: string;
  let wallet2: string;
  
  beforeEach(() => {
    deployer = accounts.get("deployer")!;
    wallet1 = accounts.get("wallet_1")!;
    wallet2 = accounts.get("wallet_2")!;
  });
  
  it("should use setup values", () => {
    // Use deployer, wallet1, wallet2
  });
});
```

### 5. Test Edge Cases

```typescript
describe("edge cases", () => {
  it("should handle zero amount", () => {});
  it("should handle maximum uint", () => {});
  it("should handle same sender and recipient", () => {});
  it("should handle empty string", () => {});
});
```

### 6. Test Boundary Conditions

```typescript
describe("boundary conditions", () => {
  it("should handle minimum valid value", () => {});
  it("should handle maximum valid value", () => {});
  it("should fail just below minimum", () => {});
  it("should fail just above maximum", () => {});
});
```

## Coverage and Reports

### Generate Coverage Report

```bash
yarn test:report
```

This generates:
- Line coverage
- Function coverage
- Branch coverage
- Cost analysis

### Coverage Output

```
File                  | % Stmts | % Branch | % Funcs | % Lines
----------------------|---------|----------|---------|--------
contracts/
  fungible-token.clar |   95.00 |    90.00 |  100.00 |   95.00
  escrow.clar         |   88.00 |    85.00 |   90.00 |   88.00
```

### Cost Analysis

The cost report shows:
- Execution costs for each function call
- Read/write costs
- Total costs per test

## Debugging Tests

### 1. Use Console Logging

```typescript
it("should debug test", () => {
  const result = simnet.callPublicFn(
    "contract-name",
    "function-name",
    [Cl.uint(100)],
    accounts.get("wallet_1")!
  );
  
  console.log("Result:", result);
  console.log("Events:", result.events);
});
```

### 2. Run Single Test

```bash
# Run only tests matching pattern
yarn vitest run -t "should transfer tokens"
```

### 3. Use VS Code Debugger

1. Set breakpoints in test file
2. Open Debug panel
3. Run "Debug Current Test File"
4. Step through code

### 4. Inspect Simnet State

```typescript
it("should inspect state", () => {
  // Get contract state
  const state = simnet.getContractState("contract-name");
  console.log("Contract state:", state);
  
  // Get account info
  const account = simnet.getAccountInfo(accounts.get("wallet_1")!);
  console.log("Account info:", account);
});
```

## Examples

### Complete Test File Example

```typescript
import { describe, expect, it, beforeEach } from "vitest";
import { Cl } from "@stacks/transactions";

describe("fungible-token", () => {
  let deployer: string;
  let wallet1: string;
  let wallet2: string;
  
  beforeEach(() => {
    deployer = accounts.get("deployer")!;
    wallet1 = accounts.get("wallet_1")!;
    wallet2 = accounts.get("wallet_2")!;
  });
  
  describe("initialization", () => {
    it("should have correct total supply", () => {
      const result = simnet.callReadOnlyFn(
        "fungible-token",
        "get-total-supply",
        [],
        deployer
      );
      expect(result.result).toBeOk(Cl.uint(300));
    });
  });
  
  describe("transfer", () => {
    it("should transfer tokens successfully", () => {
      const amount = 50;
      
      const result = simnet.callPublicFn(
        "fungible-token",
        "transfer-token",
        [Cl.uint(amount), Cl.principal(wallet2)],
        wallet1
      );
      
      expect(result.result).toBeOk(Cl.bool(true));
    });
    
    it("should fail with insufficient balance", () => {
      const amount = 1000000;
      
      const result = simnet.callPublicFn(
        "fungible-token",
        "transfer-token",
        [Cl.uint(amount), Cl.principal(wallet2)],
        wallet1
      );
      
      expect(result.result).toBeErr(Cl.bool(false));
    });
  });
});
```

## Additional Resources

- [Vitest Documentation](https://vitest.dev/)
- [Clarinet SDK Documentation](https://github.com/hirosystems/clarinet)
- [Clarity Testing Guide](https://docs.stacks.co/clarity/testing)
- [CONTRIBUTING.md](CONTRIBUTING.md) - Contribution guidelines

## Questions?

If you have questions about testing:
1. Check this guide
2. Review existing test files
3. Open an issue with the `question` label

Happy testing! 🧪
