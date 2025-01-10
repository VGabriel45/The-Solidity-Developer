# Gas Optimization Techniques

## 1. Storage Optimization

### Storage Costs
- Initial storage write costs 20,000 gas (SSTORE opcode)
- Modifying storage costs 5,000 gas
- Reading from storage costs 200 gas (SLOAD opcode)
- Storage variable declarations have no gas cost (only initialization costs gas)

### Best Practices
- Cache storage variables in memory within functions
- Calculate all updates using memory variables before updating storage
- Pack multiple storage variables into single 32-byte slots when possible
- Pack struct members efficiently to minimize slots used
- Don't initialize variables to zero values (e.g. use `uint256 i;` instead of `uint256 i = 0;`)
- Use `constant` and `immutable` for values that don't change

### Storage Refunds
- Zero out storage variables when no longer needed (refunds 15,000 gas)
- Using `selfdestruct` refunds 24,000 gas (limited to half of gas used in call)

### Data Types and Packing
- Use `bytes32` when possible (most optimized storage type)
- Use smallest possible fixed bytes type (`bytes1` to `bytes32`) if length is limited
- `bytes32` is cheaper than `string`
- Variable packing only works in storage, not in memory or calldata
- Packing function arguments or local variables provides no savings
- Small integers (uint8, etc) use full 32-byte slots in isolation
- Real savings come from packing multiple small variables into single slots

### Inheritance & Variable Packing
- Variables in child contracts can be packed with parent variables
- Variable order follows C3 linearization (child variables come after parent)
- Strategic ordering of inherited contracts can optimize packing
- Consider variable layouts across inheritance chain

### Memory vs Storage
- Avoid unnecessary copying between memory and storage
- Use storage pointers for arrays instead of copying to memory
- Memory costs increase quadratically with size
- Memory operations:
  - First 32 bytes: 3 gas
  - Next 32 bytes: 3 gas
  - Subsequent 32 bytes: ~3 gas + quadratic cost
- Consider tradeoffs between storage and memory based on:
  - Array size
  - Number of operations
  - Access patterns

### Function Optimizations
- Use `external` over `public` when possible (calldata vs memory)
- Make functions `payable` if they don't handle ETH (saves gas)
- Order frequently called functions earlier in contract
- Reduce parameter count and size
- Consider inline functions vs modifiers based on usage

### Advanced Techniques
- Use unchecked blocks for safe arithmetic
- Minimize contract-to-contract calls (EXTCODESIZE is expensive)
- Use libraries for complex, reusable logic
- Consider ERC1167 minimal proxy for deploying multiple instances
- Use Merkle proofs for validating large datasets efficiently

Each optimization should be carefully considered against code readability and maintenance. Not all optimizations are worth the added complexity.
