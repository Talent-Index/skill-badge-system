# Avalanche L1 Days in Kenya! 🌟 || Adamur

# Solidity Masterclass: Building and Deploying Smart Contracts on Avalanche

## Overview
This guide will walk you through mastering Solidity concepts through a Project Based Approach. 
The project involves creating a **Skill Badge System** where badges can be issued and transferred, and deploying it to the Avalanche blockchain.

We will cover 100% of Solidity core concepts in a step-by-step manner, ensuring precise descriptions and practical applications.

---

## Prerequisites
0. **Solidity Documentation**: [Solidity_Link](https://soliditylang.org/)
1. **Basic Understanding of Programming**: Familiarity with programming concepts.
2. **Node.js Installed**: Download and install Node.js from [Node.js Official Site](https://nodejs.org).
3. **GitHub Account**: Create one if you don’t have it at [GitHub](https://github.com).
4. **Visual Studio Code**: [Link](https://code.visualstudio.com/)
5. **Remix**: [Remix](https://remix.run/)

---

## Step 1: Setting Up Your Development Environment

### 1.1 Install Node.js
Ensure you have Node.js installed by running the following command:

```bash
node -v
```

If not installed, download it from [Node.js Official Site](https://nodejs.org) and follow the installation instructions.

### 1.2 Create a Project Folder and Install Hardhat
1. Create a new folder for your project:

    ```bash
    mkdir skill-badge-system
    cd skill-badge-system
    ```

2. Initialize a new Node.js project:

    ```bash
    npm init -y
    ```

3. Install Hardhat:

    ```bash
    npm install --save-dev hardhat
    ```

4. Create a Hardhat project:

    ```bash
    npx hardhat
    ```

5. Choose “Create a basic sample project” and install the required dependencies.

### 1.3 Install Additional Dependencies
Install OpenZeppelin contracts and dotenv for managing environment variables:

```bash
npm install @openzeppelin/contracts dotenv
```

---

## Step 2: Writing the Smart Contract

We’ll implement a `Badge.sol` contract that demonstrates Solidity core concepts. Create the `Badge.sol` file inside the `contracts` folder and add the following code:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20; // Specifies the Solidity version.

/**
 * @title Skill Badge System
 * @dev Demonstrates core Solidity concepts with a practical use case.
 */
contract Badge {
    // Struct to store badge details.
    struct BadgeInfo {
        string name;         // Name of the badge.
        string description;  // Description of the badge.
        address owner;       // Current owner of the badge.
    }

    // Mapping to store all issued badges by their ID.
    mapping(uint256 => BadgeInfo) public badges;

    // Counter for the total number of badges issued.
    uint256 public badgeCount;

    // Address of the contract deployer, designated as the badge issuer.
    address public issuer;

    // Events to notify when a badge is issued or transferred.
    event BadgeIssued(uint256 badgeId, address recipient);
    event BadgeTransferred(uint256 badgeId, address newOwner);

    // Modifier to restrict access to the badge issuer.
    modifier onlyIssuer() {
        require(msg.sender == issuer, "Not authorized");
        _;
    }

    // Constructor to set the deployer as the initial badge issuer.
    constructor() {
        issuer = msg.sender;
    }

    /**
     * @dev Function to issue a new badge to a recipient.
     * @param _name Name of the badge.
     * @param _description Description of the badge.
     * @param _recipient Address of the recipient.
     */
    function issueBadge(
        string memory _name,
        string memory _description,
        address _recipient
    ) public onlyIssuer {
        badgeCount++; // Increment the badge ID counter.
        badges[badgeCount] = BadgeInfo(_name, _description, _recipient); // Store badge details.
        emit BadgeIssued(badgeCount, _recipient); // Trigger an event.
    }

    /**
     * @dev Function to transfer an existing badge to a new owner.
     * @param _badgeId ID of the badge to transfer.
     * @param _newOwner Address of the new owner.
     */
    function transferBadge(uint256 _badgeId, address _newOwner) public {
        require(msg.sender == badges[_badgeId].owner, "Not badge owner"); // Verify ownership.
        badges[_badgeId].owner = _newOwner; // Update the badge owner.
        emit BadgeTransferred(_badgeId, _newOwner); // Trigger an event.
    }
}
```

### Core Concepts Covered

1. **State Variables**: Variables like `badgeCount` and `badges` store persistent data on the blockchain.
2. **Structs**: Custom data types used to group related data, such as `BadgeInfo` for badge details.
3. **Mappings**: Efficient data structures that map keys to values, as seen with `badges`.
4. **Modifiers**: Reusable code enforcing rules like `onlyIssuer`, restricting access to certain functions.
5. **Events**: Emit logs for external systems to track contract actions.
6. **Constructor**: A special function executed once at deployment to initialize contract state.
7. **Functions**: Code blocks that define the contract's behavior, like `issueBadge` and `transferBadge`.

---

## Step 3: Deploying to Avalanche

1. Create a `.env` file and add your Avalanche key:

    ```bash
    PRIVATE_KEY=<your_private_key>
    RPC_URL=https://api.avax.network/ext/bc/C/rpc
    ```

2. Write the deployment script in `scripts/deploy.js`:

    ```javascript
    const hre = require("hardhat");

    async function main() {
        const Badge = await hre.ethers.getContractFactory("Badge");
        const badge = await Badge.deploy();

        await badge.deployed();

        console.log("Badge deployed to:", badge.address);
    }

    main()
        .then(() => process.exit(0))
        .catch((error) => {
            console.error(error);
            process.exit(1);
        });
    ```

3. Deploy the contract:

    ```bash
    npx hardhat run scripts/deploy.js --network avalanche
    ```

### Core Concepts Covered

1. **Environment Variables**: Securely manage sensitive information using a `.env` file.
2. **Deployment Script**: Automate contract deployment with JavaScript.
3. **Hardhat Framework**: Simplifies smart contract development, deployment, and testing.

---

## Step 4: Upload to GitHub

1. Initialize Git:

    ```bash
    git init
    git remote add origin https://github.com/yourusername/skill-badge-system.git
    git branch -M main
    ```

2. Push the code:

    ```bash
    git add .
    git commit -m "Initial commit"
    git push -u origin main
    ```

---
