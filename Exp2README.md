# Experiment 2: Blockchain-Based Crowdfunding (Kickstarter Alternative)
## DATE : 16.04.2025
## Aim:
To create a decentralized crowdfunding platform where donors contribute funds only if the campaign goal is met.

## Algorithm:
### STEP 1: 
A project owner starts a campaign with a funding goal and deadline.

### STEP 2:
Contributors can send ETH to the campaign.

### STEP 3:
If the goal is met before the deadline, funds are released to the project owner.

### STEP 4:
If the goal is not met, contributors can withdraw their funds.

## Program:
```
REG NO : 212223240141
NAME : RIYA P L

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Crowdfunding {
    struct Campaign {
        address creator;
        uint256 goal;
        uint256 deadline;
        uint256 amountRaised;
        bool goalMet;
        mapping(address => uint256) contributions;
    }

    Campaign public campaign;

    constructor(uint256 _goal, uint256 _duration) {
        campaign.creator = msg.sender;
        campaign.goal = _goal;
        campaign.deadline = block.timestamp + _duration;
    }

    function contribute() public payable {
        require(block.timestamp < campaign.deadline, "Campaign ended");
        campaign.amountRaised += msg.value;
        campaign.contributions[msg.sender] += msg.value;
    }

    function withdrawFunds() public {
        require(msg.sender == campaign.creator, "Only creator can withdraw");
        require(campaign.amountRaised >= campaign.goal, "Goal not met");
        payable(msg.sender).transfer(campaign.amountRaised);
        campaign.goalMet = true;
    }

    function refund() public {
        require(block.timestamp > campaign.deadline, "Campaign still active");
        require(campaign.amountRaised < campaign.goal, "Goal was met");
        uint256 amount = campaign.contributions[msg.sender];
        campaign.contributions[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }
}
```
# Output:
![Screenshot 2025-04-16 094918](https://github.com/user-attachments/assets/61115f2e-8cd1-479b-abc6-3d4700973cf4)
![Screenshot 2025-04-16 094950](https://github.com/user-attachments/assets/ee2f01a5-6451-4f19-92c3-045f68bf049b)
![Screenshot 2025-04-16 094939](https://github.com/user-attachments/assets/fb246563-2749-4d95-8055-ecc87d034f83)

# RESULT: 
Thus the Blockchain-Based Crowdfunding is successfully implemented.
