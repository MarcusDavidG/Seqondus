# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Comprehensive documentation suite
  - CONTRIBUTING.md - Contribution guidelines
  - CODE_OF_CONDUCT.md - Community guidelines
  - SECURITY.md - Security policy and best practices
  - SETUP.md - Detailed setup instructions
  - TESTING.md - Testing guide and patterns
  - ARCHITECTURE.md - Project architecture documentation
  - CHANGELOG.md - Version history (this file)
- Enhanced README.md with better structure
- Contract-specific documentation in docs/ directory
- Developer guides for common tasks
- API reference documentation

### Changed
- Migrated to modern testing framework (Vitest + Clarinet SDK)
- Updated test structure and organization
- Improved project organization

### Deprecated
- Legacy test files in test-old/ directory (kept for reference)

## [1.0.0] - Initial Release

### Added
- Core smart contracts collection
  - Fungible token implementations
  - Non-fungible token implementations
  - Escrow contract
  - Flip coin betting games
  - Rocket NFT marketplace system
  - License management contract
  - Various experimental contracts
- Basic documentation
  - README.md with project overview
  - docs/escrow.md - Escrow contract documentation
  - docs/flip-coin.md - Flip coin documentation
- Testing infrastructure
  - Unit tests
  - Integration tests
- Deployment scripts
- Gitpod configuration for cloud development
- SIP trait implementations (SIP-009, SIP-010)

### Contract Categories

#### Tokens
- fungible-token.clar - Basic fungible token (ERC-20 style)
- non-fungible-token.clar - Basic NFT implementation
- hodl-token.clar - Token with holding incentives
- perishable-token.clar - Expiring token
- beeple.clar - Named token
- friedger-pool-nft.clar - Pool-based NFT

#### Experiments
- flip-coin.clar - Random boolean generation
- flip-coin-jackpot.clar - Betting with jackpot
- flip-coin-at-two.clar - Two-player betting
- flip-coin-tax-office.clar - Tax trait
- flip-coin-delegate.clar - Delegation pattern
- animal-kingdom.clar - Animal registry
- company-registry.clar - Company registration
- todo-registry.clar - Todo list management
- kv-store.clar - Key-value storage
- lists.clar - List operations
- history.clar - Historical tracking
- panic.clar - Error handling examples
- trait-conversion.clar - Trait conversion
- trait-conversion-router.clar - Routing with traits

#### Rockets
- rocket-token.clar - Rocket ecosystem token
- rocket-factory.clar - NFT minting factory
- rocket-market.clar - NFT marketplace

#### License
- license.clar - License management for applications

#### SIPs
- ft-trait.clar - Fungible token trait (SIP-010)
- nft-trait.clar - Non-fungible token trait (SIP-009)

#### Escrow
- escrow.clar - Simple escrow for transfers

### Sub-Projects

#### Owned Profiles
- Complete profile management system
- Commission-based marketplace
- Integration with external NFTs

## Version History Notes

### Versioning Strategy

This project follows [Semantic Versioning](https://semver.org/):

- **MAJOR** version for incompatible contract changes
- **MINOR** version for new contracts or features (backwards compatible)
- **PATCH** version for bug fixes and documentation

### Release Process

1. Update CHANGELOG.md with changes
2. Update version in package.json
3. Create git tag
4. Deploy to testnet for verification
5. Deploy to mainnet (if applicable)
6. Create GitHub release

### Migration Guides

When breaking changes occur, migration guides will be provided in the release notes.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to contribute to this project.

## Security

See [SECURITY.md](SECURITY.md) for security policy and vulnerability reporting.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Changelog Guidelines

### For Maintainers

When updating this changelog:

1. **Add entries under [Unreleased]** for ongoing work
2. **Use these categories**:
   - `Added` - New features or contracts
   - `Changed` - Changes to existing functionality
   - `Deprecated` - Features that will be removed
   - `Removed` - Removed features
   - `Fixed` - Bug fixes
   - `Security` - Security improvements

3. **Format entries** as:
   ```markdown
   - Brief description of change [#PR-number](link-to-pr)
   ```

4. **When releasing**:
   - Move [Unreleased] items to new version section
   - Add release date
   - Create git tag
   - Update package.json version

### For Contributors

When submitting PRs:
- Add a changelog entry under [Unreleased]
- Use the appropriate category
- Keep descriptions clear and concise
- Link to relevant issues/PRs

### Example Entry Format

```markdown
## [1.1.0] - 2024-01-15

### Added
- New staking contract with reward distribution [#123](link)
- Support for batch transfers in fungible-token [#124](link)

### Changed
- Improved gas efficiency in rocket-market [#125](link)
- Updated escrow contract with timeout mechanism [#126](link)

### Fixed
- Fixed overflow issue in hodl-token [#127](link)
- Corrected access control in license contract [#128](link)

### Security
- Added reentrancy protection to escrow [#129](link)
```

---

**Note**: This changelog started with the comprehensive documentation update. Previous changes were not formally tracked but are reflected in the git history.
