pragma solidity ^0.4.11;

contract CloudCrowdFund {
    int public historyTotal;
    int public currentTotal;
    int public minBet = 0.01;
    int public unitBet = 0.01;
    int public maxBet = 0.03;
    uint public betAmount = 3;
    int public lotteryValue = unitBet * betAmount;
    uint public progress = 0;
    int public contractBenifits = 0.02;
    address public owner;

    enum State { open, close}
    enum BetState { active, inactive}

    State public status;
    BetState public betStatus;
   
    mapping(address => uint) public betRecords;
    mapping(bytes32 => address) public votingPool;

    function CloudCrowdFund() {
        owner = msg.sender;
    }

    function Purchase() payable {
        uint lotteryCount = msg.value / unitBet;
        // todo: check msg.value is valid
        

        //generate n ticket for msg.sender
        for (uint i = 0 ; i< lotteryCount; i++){
            bytes32 hash = sha3(123, msg.sender);
            votingPool[hash] = msg.sender;
        }

        historyTotal += msg.value;
        currentTotal += msg.value;

        if (currentTotal == lotteryValue ){
            Lottery();
        }
    }

    function Lottery() {
        betStatus = 'inactive';
         
        uint random_number = uint(block.blockhash(block.number-1))%10 + 1;

        var winerAddress = votingPool[random_number];

        // send 98% total 
        winerAddress.transfer(currentTotal * ( 1 - contractBenifits));

        // reset;
        currentTotal = 0;
        betStatus = 'active';
        
    }

}

