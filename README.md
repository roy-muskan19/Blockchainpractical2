# Blockchainpractical2

pragma solidity ^0.8.0;

contract Voting {
    struct Candidate {
        string name;
        uint voteCount;
    }

    mapping(address => bool) public voters;
    Candidate[] public candidates;

    constructor(string[] memory _candidateNames) {
        for (uint i = 0; i < _candidateNames.length; i++) {
            candidates.push(Candidate(_candidateNames[i], 0));
        }
    }

    function vote(uint _candidateIndex) public {
        require(!voters[msg.sender], "You have already voted.");
        require(_candidateIndex < candidates.length, "Invalid candidate index.");

        voters[msg.sender] = true;
        candidates[_candidateIndex].voteCount++;
    }

    function getCandidates() public view returns (Candidate[] memory) {
        return candidates;
    }
}

![image](https://github.com/user-attachments/assets/6a484423-4399-4334-aefd-fcfcf6e6d185)

pragma solidity ^0.8.0;

contract StudentRecords {
    struct Student {
        string name;
        uint rollNumber;
    }

    mapping(uint => Student) public students;

    function addStudent(uint _rollNumber, string memory _name) public {
        students[_rollNumber] = Student(_name, _rollNumber);
    }

    function getStudent(uint _rollNumber) public view returns (string memory, uint) {
        Student memory student = students[_rollNumber];
        return (student.name, student.rollNumber);
    }
}

![image](https://github.com/user-attachments/assets/1df47976-4840-4c43-ae16-9a78c33e55d6)

ques 3)
pragma solidity ^0.8.0;

contract OwnerOnly {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function.");
        _;
    }

    function ownerFunction() public onlyOwner {
        // Function logic here
    }
}

![image](https://github.com/user-attachments/assets/431bf78d-26dc-4dd8-abe3-28c52c311637)

ques 4)

pragma solidity ^0.8.0;

contract TopDonors {
    mapping(address => uint) public donations;
    address[] public topDonors;

    function donate() public payable {
        donations[msg.sender] += msg.value;
        updateTopDonors();
    }

    function updateTopDonors() internal {
        // Simple implementation, might not be gas-efficient for large numbers of donors
        for (uint i = 0; i < topDonors.length; i++) {
            if (donations[msg.sender] > donations[topDonors[i]]) {
                // Shift existing donors and insert new donor
                address temp = topDonors[i];
                topDonors[i] = msg.sender;
                msg.sender = temp;
            }
        }
        if (topDonors.length < 3) {
            topDonors.push(msg.sender);
        }
    }

    function getTopDonors() public view returns (address[] memory) {
        return topDonors;
    }
}



ques 5)


pragma solidity ^0.8.0;

contract SimpleAuction {
    address public highestBidder;
    uint public highestBid;

    function bid() public payable {
        require(msg.value > highestBid, "Bid must be higher than the current highest bid.");

        highestBidder = msg.sender;
        highestBid = msg.value;
    }

    function endAuction() public {
        // Logic to end the auction and transfer funds to the highest bidder's address
        payable(highestBidder).transfer(highestBid);
    }
}



ques 6)
pragma solidity ^0.8.0;

contract EtherSplitter {
    address[3] public recipients;

    constructor(address[3] memory _recipients) {
        recipients = _recipients;
    }

    function splitEther() public payable {
        require(msg.value > 0, "Ether value must be greater than zero.");

        uint amount = msg.value / 3;
        for (uint i = 0; i < 3; i++) {
            payable(recipients[i]).transfer(amount);
        }
        // Handle remainder
        payable(recipients[0]).transfer(msg.value % 3);
    }
}

