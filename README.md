# 💰 Decentralized Smart Bank for Withdrawals and Deposits

This project demonstrates a **Simple Decentralized Bank** built using **Solidity** and deployed on the **Celo Sepolia Testnet**.  
Users can securely **deposit** and **withdraw** funds directly on the blockchain, ensuring **transparency**, **trustlessness**, and **immutability**.  

---

## 🧾 Overview

This smart contract allows users to:
- Deposit ETH (CELO) into their own account on-chain.  
- Withdraw funds securely using a **non-reentrant** mechanism.  
- Check their account balance.  
- Automatically handle direct ETH transfers to the contract.  

It also includes a **reentrancy guard** and owner-only **token recovery** function to prevent security vulnerabilities.

---

## 🚀 Features

- 🔒 **Secure Transactions** — Protected with a simple reentrancy guard.  
- 💵 **Deposit & Withdraw Easily** — Simple interface for ETH transfers.  
- 📊 **Track Balances** — Total deposits and individual balances stored transparently.  
- ⚙️ **Owner Control** — Owner can rescue accidentally sent ERC20 tokens.  
- 🌐 **Deployed on CELO Sepolia Testnet**

---

## 🔗 Transaction Details

**Deployed Transaction Link:**  
👉 [View on Celo Sepolia BlockScout](https://celo-sepolia.blockscout.com/tx/0xa79bf83f6b61a781fa2e6a2501bd5d506e8c075adf6f441689aa49e4ed67c45b)

---

## 🖼️ Transaction Proof (Screenshot)

Below is the screenshot showing the **successful deployment transaction** on the Celo Sepolia testnet explorer:

![Transaction Screenshot](https://sour-coral-6btsavzzxm.edgeone.app/) 

> _Note: This transaction was performed on CELO Sepolia testnet for demonstration purposes only._

---

## 🧠 Smart Contract Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

/// @title SimpleBank - a minimal on-chain bank for deposits & withdrawals
/// @author -
contract SimpleBank {
    // --- state ---
    mapping(address => uint256) private balances;
    uint256 public totalDeposits;      // total ETH held by contract (tracked)
    address public owner;

    // simple reentrancy guard
    uint256 private unlocked = 1;
    modifier nonReentrant() {
        require(unlocked == 1, "Reentrant call");
        unlocked = 0;
        _;
        unlocked = 1;
    }

    // --- events ---
    event Deposit(address indexed account, uint256 amount);
    event Withdrawal(address indexed account, uint256 amount);

    // --- constructor ---
    constructor() {
        owner = msg.sender;
    }

    // --- public/external functions ---

    /// @notice deposit ETH to your account
    /// @dev payable so user sends ETH with the call
    function deposit() external payable {
        require(msg.value > 0, "Send ETH to deposit");
        balances[msg.sender] += msg.value;
        totalDeposits += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    /// @notice withdraw `amount` wei from caller's balance
    /// @dev Uses nonReentrant guard and the checks-effects-interactions pattern
    function withdraw(uint256 amount) external nonReentrant {
        require(amount > 0, "Withdraw > 0");
        uint256 bal = balances[msg.sender];
        require(bal >= amount, "Insufficient balance");

        // effects
        balances[msg.sender] = bal - amount;
        totalDeposits -= amount;

        // interaction
        (bool sent, ) = payable(msg.sender).call{value: amount}("");
        require(sent, "Transfer failed");

        emit Withdrawal(msg.sender, amount);
    }

    /// @notice get balance of an account
    function balanceOf(address account) external view returns (uint256) {
        return balances[account];
    }

    /// @notice fallback to accept plain ETH transfers
    receive() external payable {
        balances[msg.sender] += msg.value;
        totalDeposits += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    /// @notice helper to let the owner recover accidentally sent ERC20 tokens (not ETH)
    /// @dev placeholder — owner only
    function rescueERC20(address token, uint256 amount) external {
        require(msg.sender == owner, "Only owner");
        (bool success, ) = token.call(abi.encodeWithSignature("transfer(address,uint256)", owner, amount));
        require(success, "Token transfer failed");
    }
}


---

🧩 Deployment Details

Network: CELO Sepolia Testnet

Tool Used: Remix IDE

Compiler Version: 0.8.20

Environment: Injected Web3 (Metamask connected to CELO Sepolia)



---

📘 How to Run Locally

1. Open Remix IDE


2. Create a new file named SimpleBank.sol


3. Paste the above Solidity code


4. Compile using Solidity 0.8.20


5. Connect Metamask to Celo Sepolia Testnet


6. Deploy the contract


7. Interact using the deposit(), withdraw(), and balanceOf() functions




---

🧑‍💻 Author

Project: Decentralized Smart Bank
Developer: Prietish Pathak
Contact Details: prietish12@gmail.com
Network: Celo Sepolia
Category: Web3 / Blockchain / Smart Contract


---

🏁 Summary

This project showcases the foundation of a decentralized banking system where users are their own custodians.
By combining Solidity with CELO’s blockchain, this minimal prototype ensures secure, transparent, and permissionless fund management — a step toward a fully decentralized financial future.


---

Network: Celo Sepolia Testnet

Explorer: BlockScout for Celo Sepolia

🚀 How to Run

Open Remix IDE

Paste the contract code above.

Compile with Solidity compiler version 0.8.20.

Deploy using Injected Web3 (MetaMask connected to Celo Sepolia).

Test deposit() and withdraw() functions with ETH.

Verify transactions on Celo BlockScout
