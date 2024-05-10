# Definition of Blockchain:

Blockchain is a distributed immutable ledger which is completely transparent. The main purpose of Blockchain is deccentralization. In January 2009 Satoshi Nakamoto created the first cryptocurrency Bitcoin.

# Metamask:

MetaMask is a software cryptocurrency wallet used to interact with the Ethereum blockchain.

![image](https://github.com/krishna1632/Blockchain/assets/160998925/08cc9bb2-d8b5-4db4-b824-385a00950cd0)

# Smart Contract:

Smart contracts are apps on a blockchain that make each side of a transaction complete its part. For example, a smart contract could initiate a fund transfer with a third party when the transferor signed an agreement.

# How to make smart cotract in blockchain?

Step 1: To make smart contract, first we have to open Remix IDE.

Step 2: In the contracts folder we make a file with .sol extension (eg.: example.sol) which is your smart contract.

We made a smart contract EventContract.sol.


# Code:

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


1. Pragma Directive: This specifies the version of Solidity the contract is compatible with. It allows the contract to be compiled with compiler versions greater than or equal to 0.5.0 and less than 0.9.0.

![image](https://github.com/krishna1632/Blockchain/assets/160998925/393eb75c-be45-4067-8f06-add7e9f00e1e)


2. EventContract: This is the name of the contract being defined.
3. Struct Event: This is a custom data structure defined within the contract. It represents an event and contains several properties such as the organizer's address, event name, date, price, ticket count, and remaining tickets.

![image](https://github.com/krishna1632/Blockchain/assets/160998925/e12a0cc2-ea91-4a94-924f-c49550a32e95)

4. Mapping: There are two mappings declared:
events: It maps an integer ID to an Event struct, allowing events to be looked up by their ID.
tickets: It maps a tuple of (address, uint) to a uint, where the address represents a participant and the uint represents the event ID. This mapping keeps track of how many tickets each participant has for each event.

![image](https://github.com/krishna1632/Blockchain/assets/160998925/b66ddf75-3239-4c38-99b4-7908e9a7451b)

5. nextId: This is a public unsigned integer variable that keeps track of the next available ID for events.


![image](https://github.com/krishna1632/Blockchain/assets/160998925/5b04269d-064b-4eaa-b94a-c84d3c09b431)






