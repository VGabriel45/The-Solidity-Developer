# Solidity Smart Contract Best Practices

## 1. Check-Effects-Interactions Pattern
- Always follow CEI (Checks-Effects-Interactions) pattern
- First perform all checks (require statements, validations)
- Then make state changes to your contract
- Finally interact with other contracts/addresses
- This prevents malicious contracts from re-entering your contract in an inconsistent state

