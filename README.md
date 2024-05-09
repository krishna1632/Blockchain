# Definition of Blockchain:

Blockchain is a distributed immutable ledger which is completely transparent. The main purpose of Blockchain is deccentralization. In January 2009 Satoshi Nakamoto created the first cryptocurrency Bitcoin.

# Metamask:

MetaMask is a software cryptocurrency wallet used to interact with the Ethereum blockchain.

![image](https://github.com/krishna1632/Blockchain/assets/160998925/08cc9bb2-d8b5-4db4-b824-385a00950cd0)

# How to Make Smart Contract?


// SPDX-License-Identifier: MIT
pragma solidity >=0.5.0 <0.9.0;

contract EventContract{
    struct Event{
        address organizer;
        string name;
        uint date;
        uint price;
        uint ticketCount;
        uint ticketRemain;
    }


    mapping(uint=>Event) public events;
    mapping(address=>mapping(uint=>uint)) public tickets;
    uint public nextId;

    function createEvent(string memory name,uint date,uint price,uint ticketCount) external {

        require(date>block.timestamp,"You can organise event for future date");
        require(ticketCount>0,"You can organise event only if you buy more than 0 ticket");


        events[nextId]=Event(msg.sender,name,date,price,ticketCount,ticketCount);
        nextId++;
        

    }
    
    
    function buyTicket(uint id,uint quantity) external payable {

        require(events[id].date!=0,"Event does no exist");
        require(events[id].date>block.timestamp,"Event already occured");
        Event storage _event = events[id];
        require(msg.value==(_event.price*quantity),"Ethere is not enough");
        require(_event.ticketRemain>=quantity,"Not enough Tickets");
        _event.ticketRemain -=quantity;
        tickets[msg.sender][id]+=quantity;

    }
    function transferTicket(uint id,uint quantity,address to)external {
        
        require(events[id].date!=0,"Event does no exist");
        require(events[id].date>block.timestamp,"Event already occured");
        require(tickets[msg.sender][id]>=quantity,"You do not have enough Tickets");
        tickets[msg.sender][id]-=quantity;
        tickets[to][id]+=quantity;
    }
}
