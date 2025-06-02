# Experiment 3: Supply Chain Transparency for Luxury Goods
## DATE : 21.04.2025
## Aim:
To develop a smart contract that tracks the supply chain of luxury goods, ensuring authenticity.
## Algorithm:
### Step 1 :
Manufacturer registers a product with unique productId and name.

### Step 2:
Product details (name, owner, verified status) are recorded on-chain.

### Step 3:
At each checkpoint, the current owner can transfer ownership to the next party in the chain.

### Step 4:
Ownership transfer is allowed only if the sender is the current owner.

### Step 5:
Each transfer is logged via an event (OwnershipTransferred).

### Step 6:
Anyone (e.g., buyer) can call verifyProduct(productId) to: View the name, current owner, and verification status.

### Step 7:
If the product exists and is verified, it's authentic.

### Step 8:
The chain ensures traceability and trust.

# Program:
```
NAME : KAMALESH SV
REG NO : 212222240041

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LuxurySupplyChain {
    struct Product {
        string name;
        address currentOwner;
        bool verified;
    }

    mapping(uint256 => Product) public products;

    event ProductRegistered(uint256 productId, string name);
    event OwnershipTransferred(uint256 productId, address newOwner);

    function registerProduct(uint256 productId, string memory name) public {
        require(products[productId].currentOwner == address(0), "Product already registered");
        products[productId] = Product(name, msg.sender, true);
        emit ProductRegistered(productId, name);
    }

    function transferOwnership(uint256 productId, address newOwner) public {
        require(products[productId].currentOwner == msg.sender, "Not the owner");
        products[productId].currentOwner = newOwner;
        emit OwnershipTransferred(productId, newOwner);
    }

    function verifyProduct(uint256 productId) public view returns (string memory, address, bool) {
        Product memory p = products[productId];
        return (p.name, p.currentOwner, p.verified);
    }
}
```
# Output:
![Screenshot 2025-04-21 141607](https://github.com/user-attachments/assets/013d06e4-8178-4c31-a2fb-c14f23e72350)

![Screenshot 2025-04-21 141624](https://github.com/user-attachments/assets/5a0e6245-fb63-489e-ab56-33fd48efb53d)

![Screenshot 2025-04-21 141656](https://github.com/user-attachments/assets/86f6d3c2-67a8-4027-b306-62f785b81b94)

![Screenshot 2025-04-21 141716](https://github.com/user-attachments/assets/7af7239a-c098-4f0c-a3b5-e593c747c271)

# High-Level Overview:
Helps prevent counterfeit luxury goods.


Teaches real-world supply chain use cases.

# RESULT : 
Thus the Supply Chain Transparency for Luxury Goods is successfully implemented.
