# Solidity Developer Interview Questions

## Basic Questions

1. **What is the difference between `memory` and `storage` in Solidity?**
   - `storage` is persistent and stored on the blockchain, like a computer's hard drive
   - `memory` is temporary and cleared between external function calls, like RAM
   - `storage` is more expensive in terms of gas costs
   - Variables declared in functions are `memory` by default
   - State variables are `storage` by default

2. **What is the difference between `private`, `internal`, `public`, and `external` functions?**
   - `private`: Can only be accessed within the contract where they are defined
   - `internal`: Can be accessed by the contract where they are defined and child contracts
   - `public`: Can be accessed both internally (within the contract) and externally (outside the contract)
   - `external`: Can only be accessed from outside the contract where they are defined
   - Important for security to choose appropriate visibility modifiers to prevent unauthorized access

3. **Approximately, how large can a smart contract be?**
   - Maximum contract size is 24,576 bytes (introduced in EIP-170 with Spurious Dragon hard-fork)
   - This limit exists to prevent denial-of-service (DOS) attacks
   - The limit helps manage computational costs for nodes that need to store and process contract bytecode
   - When exceeded, contracts may not be deployable on Mainnet
   - Solutions include enabling optimizer, reducing revert strings, or using libraries

4. **What is the difference between CREATE, CREATE2, and CREATE3?**
   - CREATE calculates contract address using sender's address and nonce

   - CREATE2 uses sender's address, salt (user-defined), and contract's init code
   - CREATE2 allows for more reliable predetermined contract addresses
   - CREATE2 is particularly useful in counterfactual systems
   - CREATE2 gives developers flexibility to choose desirable addresses by adjusting salt values
   
   - CREATE3 is a community proposal that builds on CREATE2
   - CREATE3 separates contract creation into two steps: deploying proxy then implementation
   - CREATE3 generates addresses that only depend on sender and salt (not init code)
   - CREATE3 allows updating contract code while keeping same address
   - CREATE3 is useful for cross-chain deployments with identical addresses

5. **What major change with arithmetic happened with Solidity 0.8.0?**
   - Automatic checks for arithmetic underflows and overflows were added
   - Before 0.8.0, integer underflows/overflows were silent and didn't cause errors
   - After 0.8.0, transactions revert if arithmetic operations would cause under/overflow
   - Previously, developers needed to use libraries like SafeMath for these checks
   - This change improved security by preventing common arithmetic vulnerabilities