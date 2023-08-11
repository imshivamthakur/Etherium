# Etherium
pragma solidity ^ 0.6.1;

contract Patient_SC{
address payable owner;
bytes32 key;
bytes32 hashid;
bytes32 hash;
bool public paused;

mapping (bytes32=>bytes32) hashmapping;

    constructor() public { //to set the deployer of the contract as owner 
        owner=msg.sender;
    }

    function setpublickey(bytes32 _key) external{

       require (msg.sender==owner,"You are not the owner");
        key=_key;
    }

    function getpublickey() external view returns(bytes32){ // Service provider is able to access data identifiers 
        return(key);
    }

    function set_hash(bytes32 _hashid, bytes32 _hash) external {  // owner can set and update data identifier
        require (msg.sender==owner,"You are not the owner");
          hashmapping[_hashid]=_hash;
    }

    function get_hash(bytes32 _hashid) external view returns(bytes32){ // Service provider is able to access data identifiers 
         require(paused==false,"contract is paused by owner");
        return(hashmapping[_hashid]);
    }


    function pause_smartcontract(bool _paused) public{ //owner is able to pause and unpause the contract 
        require(msg.sender==owner,"you are not the owner");
        paused=_paused;
    }

}


contract Provider_SC{
address payable owner;
bytes32 key;
bytes32 hashid;
bytes32 hash;
mapping (bytes32=>bytes32) hashmapping;
address addressreg;
 bool paused;
constructor() public { //to set the deployer of the contract as owner 
        owner=msg.sender;
    }

function setpublickey(bytes32 _key) external{

       require (msg.sender==owner,"You are not the owner");
        key=_key;

}

    function getpublickey() external view returns(bytes32){ // Service provider is able to access data identifiers 
        return(key);
    }

    function set_hash(bytes32 _hashid, bytes32 _hash) external{  // owner can set and update data identifier
        require (msg.sender==owner,"You are not the owner");
          hashmapping[_hashid]=_hash;
    }

    function get_hash(bytes32 _hashid) external view returns(bytes32){ // Service provider is able to access data identifiers 
         require(paused==false,"contract is paused by owner");
        return(hashmapping[_hashid]);
    }

    function pause_smartcontract(bool _paused) public{ //owner is able to pause and unpause the contract 
        require(msg.sender==owner,"you are not the owner");
        paused=_paused;
    }

}
contract registry{

    address payable owner;
    mapping (address=>bytes32) hidmapping;
    mapping (address=>address) addressmapping;
    mapping (address=>bytes32) regulatormapping;

    function register_regulator(bytes32 publickey) external {  // owner can set and update data identifier
          //require( isSealer( tx.origin ), "tx.origin is not a known sealer and can not call this function." );

         regulatormapping[msg.sender]=publickey;
    }

     function verify__regulator(address _healthid) external view returns(bytes32){ // Service provider is able to access data identifiers 

        return(regulatormapping[_healthid]);
    }

    function register_attestation(address _healthid,bytes32 _key) external {  // owner can set and update data identifier
          require (regulatormapping[msg.sender].length!=0,"You are not the owner");
          hidmapping[_healthid]=_key;
          addressmapping[_healthid]=msg.sender;
    }

     function verify_registration(address _healthid) external view returns(bytes32,address){ // Service provider is able to access data identifiers 

        return(hidmapping[_healthid],addressmapping[_healthid]);
    }
      function revoke_registration(address _healthid) external{ // Service provider is able to access data identifiers 

    require (addressmapping[_healthid]==msg.sender,"You are not the validator");
    hidmapping[_healthid]=0;

    }


}
