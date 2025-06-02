# Experiment 4: DeFi Lending and Borrowing Protocol
## DATE : 21.04.2025
## Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

## Algorithm:
### step 1:
Deploy contract and set the owner, interest rate, and liquidation threshold.

### step 2:
User deposits funds by calling deposit() and sending Ether to the contract.

### step 3:
Check collateral and ensure it meets the required threshold for borrowing.

### step 4:
User borrows funds by calling borrow(amount), transferring borrowed funds to the user.

### step 5:
Record deposits in the deposits mapping and borrowed amounts in the borrowed mapping.

### step 6:
Accumulate interest (future step, not implemented in current contract).

### step 7:
Check liquidation if the user’s collateral is below 150% of the borrowed amount.

### step 8:
Liquidate borrower by calling liquidate() and transferring seized collateral to the caller.

## Program:
```

NAME : KAMALESH SV
REG NO : 212222240041

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {
    address public owner;
    uint256 public interestRate = 5; // 5% interest per cycle
    uint256 public liquidationThreshold = 150; // 150% collateralization
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Deposited(address indexed user, uint256 amount);
    event Borrowed(address indexed user, uint256 amount, uint256 collateral);
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 collateralSeized);

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Deposit must be greater than zero");
        deposits[msg.sender] += msg.value;
        emit Deposited(msg.sender, msg.value);
    }

    function borrow(uint256 amount) public payable {
        require(msg.value >= (amount * liquidationThreshold) / 100, "Nota enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
    }
    function reduceCollateral(address user, uint256 amount) public {
    require(msg.sender == owner, "Only owner can reduce");
    require(collateral[user] >= amount, "Not enough collateral to reduce");
    collateral[user] -= amount;
    }

    function liquidate(address borrower) public {
        require(collateral[borrower] < (borrowed[borrower] * liquidationThreshold) / 100, "Not eligible for liquidation");
        uint256 debt = borrowed[borrower];
        uint256 seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;
        payable(msg.sender).transfer(seizedCollateral);
        emit Liquidated(borrower, debt, seizedCollateral);
    }
}

```
# Output:
![Screenshot 2025-04-28 131034](https://github.com/user-attachments/assets/2d9a5fb3-1d84-4285-8484-caa8d1248c07)

![Screenshot 2025-04-28 133609](https://github.com/user-attachments/assets/9ddab251-e2b6-47f3-8948-22b56978a814)

![Screenshot 2025-04-28 133943](https://github.com/user-attachments/assets/6294788f-e4b7-4d18-8154-bf6aa80aa874)

![Screenshot 2025-04-28 134019](https://github.com/user-attachments/assets/801bab03-cbde-46de-bb95-d1eff9a44cbc)

![Screenshot 2025-04-28 134030](https://github.com/user-attachments/assets/856a64e9-077a-4ef8-a75f-383d84defc18)

![Screenshot 2025-04-28 134129](https://github.com/user-attachments/assets/e7b99147-77e2-4cfd-9637-d1ab5c3b0384)

# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# RESULT : 
Thus the DeFi Lending and Borrowing Protocol is successfully implemented.
