# Degen Game

This Solidity program is a simple "Degen" program that demonstrates  the  functionality of various functions in the Solidity programming language.

## Description

This program is a smart contract written in Solidity, a programming language used for developing smart contracts on the Ethereum blockchain. The contract has a various function . This program can be used as a stepping stone for more complex projects in the future.

## Getting Started

### Executing program

// Requirements
// Your task is to create a ERC20 token and deploy it on the Avalanche network for Degen Gaming. The smart contract should have the following functionality:

// 1. Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
// 2. Transferring tokens: Players should be able to transfer their tokens to others.
// 3. Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
// 4. Checking token balance: Players should be able to check their token balance at any time.
// 5. Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";

contract DegenToken is ERC20, Ownable, ERC20Burnable {

    struct Loyalty {
        uint256 lastUpdated;
        uint256 loyaltyPoints;
    }

    mapping(address => Loyalty) private _loyaltyPoints;
    uint256 public constant LOYALTY_RATE = 1; // 1 token per day per token held

    event TokensMinted(address indexed to, uint256 amount);
    event TokensTransferred(address indexed from, address indexed to, uint256 amount);
    event TokensRedeemed(address indexed from, string item, uint256 amount);
    event TokensBurned(address indexed from, uint256 amount);
    event LoyaltyPointsUpdated(address indexed user, uint256 loyaltyPoints);

    // Mapping to store item names and their respective token costs
    mapping(string => uint256) public itemCosts;

    constructor() ERC20("Degen", "DGN") Ownable(msg.sender) {}

    function mint(address to, uint256 amount) external onlyOwner {
        _mint(to, amount);
        emit TokensMinted(to, amount);
    }

    function transferTokens(address recipient, uint256 amount) external {
        _updateLoyaltyPoints(msg.sender);
        _updateLoyaltyPoints(recipient);
        _transfer(_msgSender(), recipient, amount);
        emit TokensTransferred(msg.sender, recipient, amount);
    }

    function redeemTokens(string memory item) external {
        uint256 itemCost = itemCosts[item];
        require(itemCost > 0, "Item does not exist.");
        require(balanceOf(msg.sender) >= itemCost, "Insufficient balance to redeem tokens for this item.");
        _burn(msg.sender, itemCost);
        emit TokensRedeemed(msg.sender, item, itemCost);
    }

    function checkBalance() external view returns (uint256) {
        return balanceOf(msg.sender);
    }

    function burnTokens(uint256 amount) external {
        require(balanceOf(msg.sender) >= amount, "Insufficient balance to burn tokens.");
        _burn(msg.sender, amount);
        emit TokensBurned(msg.sender, amount);
    }

    function checkLoyaltyPoints() external view returns (uint256) {
        return _loyaltyPoints[msg.sender].loyaltyPoints;
    }

    function claimLoyaltyTokens() external {
        _updateLoyaltyPoints(msg.sender);
        uint256 loyaltyPoints = _loyaltyPoints[msg.sender].loyaltyPoints;
        require(loyaltyPoints > 0, "No loyalty points to claim.");
        _loyaltyPoints[msg.sender].loyaltyPoints = 0;
        _mint(msg.sender, loyaltyPoints);
        emit LoyaltyPointsUpdated(msg.sender, 0);
    }

    function _updateLoyaltyPoints(address user) internal {
        Loyalty storage loyalty = _loyaltyPoints[user];
        uint256 daysHeld = (block.timestamp - loyalty.lastUpdated) / 1 days;
        uint256 newPoints = daysHeld * balanceOf(user) * LOYALTY_RATE;
        loyalty.loyaltyPoints += newPoints;
        loyalty.lastUpdated = block.timestamp;
        emit LoyaltyPointsUpdated(user, loyalty.loyaltyPoints);
    }

    // Function to set item costs, only owner can set this
    function setItemCost(string memory item, uint256 cost) external onlyOwner {
        itemCosts[item] = cost;
    }
}


To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar. Make sure the "Compiler" option is set to "0.8.20" (or another compatible version), and then click on the "Compile DegenGame.sol" button.

Once the code is compiled, you can deploy the contract by clicking on the "Deploy & Run Transactions" tab in the left-hand sidebar. Select the "DegenToken" contract from the dropdown menu, and then click on the "Deploy" button.

Once the contract is deployed, you can interact with it by calling all the functions. Click on the "DegenToken" contract in the left-hand sidebar, and then click on all the functions one by one. Finally, click on the "transact" button to execute the functions.

## Authors
Ajmeet kour @Ajmeetkour


## License

This project is licensed under the MIT License.
