// SPDX-License-Identifier: MIT
pragma solidity ^0.8.8;

error NotOwner();

contract FundMe {
    mapping(address => uint256) public addressToAmountFunded; 
    address[] public funders; 
    int minimumamountinusd;

    address public i_owner; 

    function fund() public payable {  
        require(msg.value >= 1, "You need to spend more ETH!");
        addressToAmountFunded[msg.sender] += msg.value;
            funders.push(msg.sender);
    }

  constructor() {
        i_owner = msg.sender;
    }
    
    modifier onlyOwner() {
        if (msg.sender != i_owner) revert NotOwner();
        _;
    }

    function withdraw() public onlyOwner {  
        for (
            uint256 funderIndex = 0;
            funderIndex < funders.length;
            funderIndex++
        ) {
            address funder = funders[funderIndex];
            addressToAmountFunded[funder] = 0;
        }
        funders = new address[](0);
        (bool callSuccess, ) = payable(msg.sender).call{
            value: address(this).balance
        }("");
        require(callSuccess, "Call failed");
    }
}
