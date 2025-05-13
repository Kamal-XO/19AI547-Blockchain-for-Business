# Experiment 8: Post-Quantum Blockchain Wallet with Lattice-Based Cryptography
# DATE : 30.04.2025
# Aim:
To create a quantum-resistant wallet using lattice-based cryptography instead of traditional ECDSA, ensuring that future quantum computers cannot break private keys.

# Algorithm:
### step 1:Identify Quantum Risk
Understand that traditional ECDSA signatures in Ethereum wallets are vulnerable to quantum attacks.

### step 2:Choose Lattice-Based Crypto
Select a quantum-resistant cryptographic method (e.g., NTRU, Kyber) for secure key generation.

### step 3:Precompute Keys Off-Chain
Generate lattice-based public-private key pairs off-chain and hash the public key for storage.

### step 4:User Registration
Users register on the blockchain by submitting the hash of their quantum-safe public key.

### step 5:Store User Info Securely
Save the hashed public key and registration status in the smart contract for each user.

### step 6:Prepare Transaction
When sending funds, the user creates a message with transaction data and signs it off-chain using their private lattice-based key.

### step 7:Verify Signature Hash
The smart contract calculates a hash of the transaction data and compares it to the provided signature (simulated as a hash).

### step 8:Process Transaction
If the hashes match and users are registered with enough balance, transfer the funds and emit a confirmation event.


# Program:

Name : RIYA P L

Reg no : 212223240141

(Solidity does not natively support lattice cryptography yet, but we simulate it using custom hash-based authentication.)
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PostQuantumWallet {
    struct User {
        bytes32 publicKeyHash;
        bool registered;
    }

    mapping(address => User) private users; // Made private to hide from Remix UI
    mapping(address => uint256) public balances;

    event UserRegistered(address user, bytes32 publicKeyHash);
    event TransactionVerified(address from, address to, uint256 amount);

    // Constructor
    constructor() {}

    // Generate a quantum-safe signature (simulated using keccak256)
    function generateSignature(address _sender, address _recipient, uint256 _amount) public pure returns (bytes32) {
        return keccak256(abi.encodePacked(_sender, _recipient, _amount));
    }

    // Generate a simulated lattice-based public key hash
    function generatePublicKeyHash(string memory _publicKey) public pure returns (bytes32) {
        return keccak256(abi.encodePacked(_publicKey));
    }

    // Register a user with a public key hash
    function registerUser(bytes32 _publicKeyHash) public {
        require(!users[msg.sender].registered, "User already registered");
        users[msg.sender] = User(_publicKeyHash, true);
        emit UserRegistered(msg.sender, _publicKeyHash);
    }

    // Send funds using quantum-safe simulated signature
    function sendFunds(address _to, uint256 _amount, bytes32 _signature) public {
        require(users[msg.sender].registered, "Sender not registered");
        require(users[_to].registered, "Recipient not registered");
        require(balances[msg.sender] >= _amount, "Insufficient funds");

        bytes32 calculatedSignature = generateSignature(msg.sender, _to, _amount);
        require(calculatedSignature == _signature, "Invalid quantum-safe signature");

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;
        emit TransactionVerified(msg.sender, _to, _amount);
    }

    // Deposit funds to the wallet
    function depositFunds() public payable {
        balances[msg.sender] += msg.value;}
}
```

# Expected Output:
Users register using a post-quantum secure public key.

![Screenshot 2025-05-07 081839](https://github.com/user-attachments/assets/f761a246-67ab-493a-8b04-f9d58817df03)


Transactions require a quantum-resistant signature for authentication.

![Screenshot 2025-05-07 081901](https://github.com/user-attachments/assets/e0cf038d-1fa0-4acd-a10b-a2a23b73c61c)


If a traditional quantum-vulnerable hash is used, the transaction fails.

![Screenshot 2025-05-07 081913](https://github.com/user-attachments/assets/84e262be-f582-4a9a-ac35-5bb2e48e67e9)


# RESULT : 
To create a quantum-resistant wallet using lattice-based cryptography instead of traditional ECDSA, ensuring that future quantum computers cannot break private keys is completed sucessfully.


