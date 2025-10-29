💰 SimpleBank



A minimal on-chain banking smart contract for secure ETH deposits and withdrawals, deployed on the Celo Sepolia Testnet.

🌐 Transaction Details
Network: Celo Sepolia Testnet
Transaction Hash: 0xf27913156919b1989ee430ea8966eff59bd45a33acb9890412b6d7f90114576f
Status: ✅ Success
Block Number: 8,453,668
Timestamp: Oct 29, 2025, 17:04:16 PM (UTC+05:30)
Deployed By: 0xC6a4DF3e3c65ab6930FF99ade096302429e8168C
📜 Smart Contract Code
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
        // minimal low-level transfer (IERC20 interface not included to keep it simple)
        (bool success, ) = token.call(abi.encodeWithSignature("transfer(address,uint256)", owner, amount));
        require(success, "Token transfer failed");
    }
}

🧠 How It Works

Deposit ETH:

Users call deposit() and send ETH.
Funds are securely stored under their wallet address.

Withdraw ETH:

Users can withdraw any portion of their deposited funds.
The contract includes a reentrancy guard to prevent attacks.

Balance Tracking:

balanceOf(address) shows individual balances.
totalDeposits tracks total ETH held by the contract.

Safety Features:

Uses checks-effects-interactions pattern for withdrawal safety.
Owner can recover mistakenly sent ERC20 tokens (not ETH).
🚀 Deployment Info
Compiler Version: 0.8.20
Network: Celo Sepolia Testnet
Tool Used: Remix IDE + MetaMask
Explorer: Celo Blockscout
Status: ✅ Successfully Deployed
📷 Screenshot



Example of successful deployment:
<img width="1470" height="956" alt="Screenshot 2025-10-29 at 17 05 01" src="https://github.com/user-attachments/assets/f587c775-4738-45dd-a66a-c8880a40c3b3" />


🔮 Future Enhancements
Add interest accrual system for long-term deposits.
Implement multi-token support (ERC20).
Add frontend dApp for easy user interaction.
👨‍💻 Author



Prietish Pathak
🎓 AI & ML Student | Narula Institute of Technology
⚙️ Passionate about Blockchain, AI, and Digital Innovation
📫 Contact: prietish12@gmail.com
