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

![Transaction Screenshot] <img width="1470" height="956" alt="Screenshot 2025-10-29 at 17 05 01" src="https://github.com/user-attachments/assets/31448960-3aae-43b3-8d6c-373d60ef9bb0" />


> _Note: This transaction was performed on CELO Sepolia testnet for demonstration purposes only._

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
