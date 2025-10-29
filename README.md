🏦 SimpleBank – A Minimal On-Chain Bank (Solidity)
Overview

SimpleBank is a Solidity-based smart contract that enables users to deposit and withdraw ETH securely on the blockchain. It uses the checks-effects-interactions pattern and a non-reentrancy guard to ensure transaction safety.

Deployed and tested on the Celo Sepolia Testnet.

🔗 Deployment Details

Network: Celo Sepolia Testnet

Transaction Hash: 0xa79bf83f6b61a781fa2e6a2501bd5d506e8c075adf6f441689aa49e4ed67c45b

Status: ✅ Successful

Timestamp: Oct 29, 2025

📜 Smart Contract Code
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

/// @title SimpleBank - a minimal on-chain bank for deposits & withdrawals
contract SimpleBank {
    mapping(address => uint256) private balances;
    uint256 public totalDeposits;
    address public owner;

    uint256 private unlocked = 1;
    modifier nonReentrant() {
        require(unlocked == 1, "Reentrant call");
        unlocked = 0;
        _;
        unlocked = 1;
    }

    event Deposit(address indexed account, uint256 amount);
    event Withdrawal(address indexed account, uint256 amount);

    constructor() {
        owner = msg.sender;
    }

    function deposit() external payable {
        require(msg.value > 0, "Send ETH to deposit");
        balances[msg.sender] += msg.value;
        totalDeposits += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    function withdraw(uint256 amount) external nonReentrant {
        require(amount > 0, "Withdraw > 0");
        uint256 bal = balances[msg.sender];
        require(bal >= amount, "Insufficient balance");

        balances[msg.sender] = bal - amount;
        totalDeposits -= amount;

        (bool sent, ) = payable(msg.sender).call{value: amount}("");
        require(sent, "Transfer failed");

        emit Withdrawal(msg.sender, amount);
    }

    function balanceOf(address account) external view returns (uint256) {
        return balances[account];
    }

    receive() external payable {
        balances[msg.sender] += msg.value;
        totalDeposits += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    function rescueERC20(address token, uint256 amount) external {
        require(msg.sender == owner, "Only owner");
        (bool success, ) = token.call(abi.encodeWithSignature("transfer(address,uint256)", owner, amount));
        require(success, "Token transfer failed");
    }
}

⚙️ Features

💰 Deposit & Withdraw ETH

🔐 Reentrancy protection

📈 Track total deposits

🧾 Transparent event logs for all actions

👑 Owner-only ERC20 token recovery

🧠 Tech Stack

Language: Solidity ^0.8.20

IDE: Remix Ethereum

Network: Celo Sepolia Testnet

Explorer: BlockScout for Celo Sepolia

🚀 How to Run

Open Remix IDE
.

Paste the contract code above.

Compile with Solidity compiler version 0.8.20.

Deploy using Injected Web3 (MetaMask connected to Celo Sepolia).

Test deposit() and withdraw() functions with ETH.

Verify transactions on Celo BlockScout
.
