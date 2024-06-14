pragma solidity ^0.8.0;

   contract SupplyChain {
       enum State { Manufactured, ForSale, Sold, Shipped, Received }

       struct Product {
           uint id;
           string name;
           address owner;
           State state;
       }

       mapping(uint => Product) public products;
       uint public productCount;

       event ProductCreated(uint id, string name, address owner, State state);
       event StateChanged(uint id, State state);

       function createProduct(string memory _name) public {
           productCount++;
           products[productCount] = Product(productCount, _name, msg.sender, State.Manufactured);
           emit ProductCreated(productCount, _name, msg.sender, State.Manufactured);
       }

       function changeState(uint _id, State _state) public {
           require(products[_id].owner == msg.sender, "Only the owner can change the state");
           products[_id].state = _state;
           emit StateChanged(_id, _state);
       }

       function transferOwnership(uint _id, address _newOwner) public {
           require(products[_id].owner == msg.sender, "Only the owner can transfer ownership");
           products[_id].owner = _newOwner;
       }
   }
