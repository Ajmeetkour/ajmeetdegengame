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

// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyTokenDegenAjmeet is ERC20, Ownable {

    struct Items {
        uint id;
        string name;
        uint price;
        bool available;
    }
    
    mapping(uint => Items) private items;
    uint private itemCount;

    mapping(address => Items[]) private redeemedItems;

    event ItemListed(uint indexed id, string name, uint price);
    event ItemRedeemed(address indexed account, uint indexed itemId, string itemName, uint itemPrice);

    constructor(address Owneraddress) 
    ERC20("AjmeetDegen","AJM") 
    Ownable(Owneraddress) {
    }

    function mint(address sendto, uint256 amount) public onlyOwner {
        _mint(sendto, amount);
    }

    function transfer(address to, uint256 amounttransfer) public override returns (bool) {
        _transfer(_msgSender(), to, amounttransfer);
        return true;
    }
     function burn(uint256 amount) public {
        require(amount > 0, "Amount must be greater than zero");
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");

        _burn(msg.sender, amount);
    }

    function redeem(uint itemId) public {
        require(itemId > 0 && itemId <= itemCount, "Invalid item ID");
        require(items[itemId].available, "Item not available");
        require(balanceOf(msg.sender) >= items[itemId].price, "Insufficient balance");

        _transfer(msg.sender, owner(), items[itemId].price);
        redeemedItems[msg.sender].push(items[itemId]);
        items[itemId].available = false;

        emit ItemRedeemed(msg.sender, itemId, items[itemId].name, items[itemId].price);
    }
     function listItem(string memory name, uint price) public onlyOwner {
        itemCount++;
        items[itemCount] = Items(itemCount, name, price, true);

        emit ItemListed(itemCount, name, price);
    }
    function _addInitialItems() internal onlyOwner {
        listItem("Sword", 100 * 10 ** 18);
        listItem("Water Shield", 150 * 10 ** 18);
        listItem("Health Potion", 50 * 10 ** 18);
        listItem("Cape", 50 * 10 ** 18);
    }

    function getItem(uint itemId) public view returns (Items memory) {
        require(itemId > 0 && itemId <= itemCount, "Invalid item ID");
        return items[itemId];
    }

    function getItemCount() public view returns (uint) {
        return itemCount;
    }

    function checkBalance(address account) public view returns (uint256) {
        return balanceOf(account);
    }


    function getRedeemedItems(address account) public view returns (Items[] memory) {
        return redeemedItems[account];
    }
}


To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar. Make sure the "Compiler" option is set to "0.8.20" (or another compatible version), and then click on the "Compile DegenGame.sol" button.

Once the code is compiled, you can deploy the contract by clicking on the "Deploy & Run Transactions" tab in the left-hand sidebar. Select the "MyTokenDegenAjmeet" contract from the dropdown menu, and then click on the "Deploy" button.

Once the contract is deployed, you can interact with it by calling all the functions. Click on the "MyTokenDegenAjmeet" contract in the left-hand sidebar, and then click on all the functions one by one. Finally, click on the "transact" button to execute the functions.

## Authors
Ajmeet kour @Ajmeetkour


## License

This project is licensed under the MIT License.
