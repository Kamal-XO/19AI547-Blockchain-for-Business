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

NAME : RIYA P L
REG NO : 212223240141

// SPDX-License-Identifier: MIT
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
        require(msg.value >= (amount * liquidationThreshold) / 100, "Not enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
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
![Screenshot 2025-04-21 135107](https://github.com/user-attachments/assets/e4097c87-ec55-4d3d-a9ad-022b0e816c32)

![Screenshot 2025-04-21 135120](https://github.com/user-attachments/assets/7715282a-97ab-4770-aa0d-105b694d0afd)

![Screenshot 2025-04-21 135143](https://github.com/user-attachments/assets/603b8cb3-8a51-410d-b868-d3c57f669c44)

![Screenshot 2025-04-21 135438](https://github.com/user-attachments/assets/72c8bb3f-393f-4596-9fa8-d6096f792f1a)

![Screenshot 2025-04-21 135519](https://github.com/user-attachments/assets/b48588ce-5294-47d1-b148-41c4aa0b8f1d)

# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# RESULT : 
Thus the DeFi Lending and Borrowing Protocol is successfully implemented.
