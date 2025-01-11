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

6. **What special CALL is required for proxies to work?**
   - `DELEGATECALL` is required for proxy contracts to work
   - It preserves msg.sender, msg.value, and other context from the calling contract
   - Runs in the context of the calling contract instead of the called contract
   - Enables proxy contract to modify its own state using implementation contract's logic
   - Important to ensure storage layout matches between proxy and implementation contracts

7. **Prior to EIP-1559, how do you calculate the dollar cost of an Ethereum transaction?**
   - Pre-EIP-1559: `((GAS_USED * GAS_PRICE / 10^9) * CURRENT_ETHER_PRICE)`
   - Post-EIP-1559: `(((BASEFEE + PRIORITY_FEE) * GAS_USED) / 10^9) * CURRENT_ETHER_PRICE`
   - Pre-EIP-1559 miners received 100% of gas costs
   - Post-EIP-1559 miners only receive priority fee, base fee is burned
   - Base fee adjusts automatically based on network congestion

8. **What are the challenges of creating a random number on the blockchain?**
   - Blockchain is deterministic - same input always produces same output
   - All data on the blockchain is public and can be read
   - Block data (timestamps, numbers) can be predicted by miners
   - On-chain random number generation is vulnerable to manipulation
   - Best practice is to use trusted oracles (like Chainlink) for randomness

9. **What is the difference between a Dutch Auction and an English Auction?**
   - Dutch Auction: price starts high and decreases over time until first bid
   - English Auction: price starts low and increases as bidders compete
   - Dutch Auction ends with first valid bid meeting current price
   - English Auction ends after time expires with highest bidder winning
   - English Auctions need protection against DOS attacks in refund mechanism

10. **What is the difference between `transfer` and `transferFrom` in ERC20?**
    - `transfer`: moves tokens from msg.sender to destination address
    - `transferFrom`: moves tokens from a specified source to destination address
    - `transferFrom` requires prior approval via allowance mechanism
    - `transfer` doesn't need approval as sender is msg.sender
    - `transferFrom` is commonly used by contracts to transfer user tokens

11. **Which is better to use for an address allowlist: a mapping or an array? Why?**
    - Mapping is better due to gas efficiency
    - Mapping provides O(1) lookup time for checking addresses
    - Arrays require iteration through elements to check membership
    - Array gas costs increase with size of allowlist
    - Arrays are more complex to maintain (add/remove operations)

12. **Why shouldn't tx.origin be used for authentication?**
    - `tx.origin` is vulnerable to phishing attacks
    - Attacker can create a malicious contract that forwards calls
    - `tx.origin` represents the original transaction sender, not immediate caller
    - Using `tx.origin` allows attackers to impersonate legitimate users
    - Best practice is to use `msg.sender` for authentication instead

13. **What hash function does Ethereum primarily use?**
    - Ethereum primarily uses Keccak-256 hash function
    - Used for generating addresses, block hashes, and Merkle trees
    - Used in smart contracts for hashing data and event signatures
    - Used for transaction receipts and signature verification
    - Other functions like ECDSA, SHA-256, and RIP-EMD160 are used for specific purposes

14. **How much is 1 gwei of Ether?**
    - 1 Gwei is one billionth (10^-9) of an Ether
    - 1 Wei = 10^-18 Ether
    - 1 Gwei = 10^-9 Ether
    - 1 Szabo = 10^-6 Ether
    - 1 Finney = 10^-3 Ether

15. **What is the difference between `assert` and `require`?**
    - `require` reverts the transaction and refunds remaining gas to the caller
    - `assert` (pre-0.8.0) reverts the transaction without refunding gas
    - After Solidity 0.8.0, both `assert` and `require` refund gas
    - `require` allows optional error messages, `assert` does not
    - `require` is used for input validation, `assert` for invariant checking