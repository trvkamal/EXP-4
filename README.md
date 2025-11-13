# Experiment 4: DeFi Lending and Borrowing Protocol
### Name : kamalesh v
### Reg No :212222240042
## Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

## Algorithm:
1.Users deposit ETH into the smart contract to provide liquidity for the lending system.

2.Each depositor’s contribution is tracked, and interest is calculated based on their share and the overall borrowing activity.

3.Borrowers request ETH by providing collateral worth at least 150% of the borrowed amount to ensure overcollateralization.

4.The system monitors how much of the total pool is borrowed and adjusts interest rates dynamically based on the utilization rate.

5.The contract continuously checks whether a borrower's collateral remains above the required ratio to avoid default risk.

6.If the collateral value drops below the threshold, liquidators can repay the debt and claim the borrower's collateral at a discounted rate.
## Program:
```py
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
## Ouput:

![Output 1](https://github.com/user-attachments/assets/a05f1ab4-b55b-4d05-9e18-a5567d4d4c55)

![Output 2](https://github.com/user-attachments/assets/7cc12720-2269-4a4a-839a-48d5218d1ab0)

![Output 3](https://github.com/user-attachments/assets/3e951852-35d1-4759-86f3-0d6796a38e2d)

![Output 4](https://github.com/user-attachments/assets/3b477e07-a1c3-49ba-8da5-761a62f131c8)

## High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.

Introduces risk management: overcollateralization and liquidation.

Directly related to DeFi protocols like Aave and Compound.

## Result:
Thus, the decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral is executed succesfully.

