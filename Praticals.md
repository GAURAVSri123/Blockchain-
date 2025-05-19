# Praticals

# Question -1 
Create a voting system with multiple candidates. Each address can vote only once.


## Contract 
 ```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract VotingSystem {
    mapping(address => bool) public hasVoted;
    mapping(string => uint) public votes;
    string[] public candidates;

    constructor(string[] memory _candidates) {
        candidates = _candidates;
    }

    function vote(string memory _candidate) public {
        require(!hasVoted[msg.sender], "Already voted!");
        bool valid = false;
        for (uint i = 0; i < candidates.length; i++) {
            if (keccak256(bytes(candidates[i])) == keccak256(bytes(_candidate))) {
                valid = true;
                break;
            }
        }
        require(valid, "Invalid candidate");
        votes[_candidate]++;
        hasVoted[msg.sender] = true;
    }
}

```

## deployment 
![Screenshot 2025-05-19 135807](https://github.com/user-attachments/assets/fb174bc2-c637-48fe-a385-5baf9896aae4)

![Screenshot 2025-05-19 135313](https://github.com/user-attachments/assets/e504a1be-7983-4e2b-995c-b533d969f3ea)



# Question 2
Write a contract that manages a list of student records (name, roll number). Allow adding and retrieving student data.


## Contract
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract StudentRecords {
    struct Student {
        string name;
        uint rollNo;
    }

    Student[] public students;

    function addStudent(string memory _name, uint _rollNo) public {
        students.push(Student(_name, _rollNo));
    }

    function getStudent(uint index) public view returns (string memory, uint) {
        require(index < students.length, "Invalid index");
        Student memory s = students[index];
        return (s.name, s.rollNo);
    }
}
```

## Deployment
![Screenshot 2025-05-19 140237](https://github.com/user-attachments/assets/25b00285-5f16-4f73-8c38-d4f69df174de)
![Screenshot 2025-05-19 140333](https://github.com/user-attachments/assets/9e26ddee-9152-46d5-a229-9cc5bead6ded)



# Question 3
Develop a contract that only allows the deployer (owner) to call a specific function (use modifiers).


## Contract
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OwnerOnly {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner");
        _;
    }

    string public secret;

    function setSecret(string memory _secret) public onlyOwner {
        secret = _secret;
    }
}

```

## deployment 
![Screenshot 2025-05-19 140757](https://github.com/user-attachments/assets/82840f29-9b45-4abb-98d8-7c00789cebd2)
![Screenshot 2025-05-19 140835](https://github.com/user-attachments/assets/04f4299b-2722-458f-9ea7-983daa214028)



# Question - 4
Write a contract where people can donate Ether and the top 3 donors are tracked.


## Contracts
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Donations {
    struct Donor {
        address donor;
        uint amount;
    }

    Donor[] public topDonors;

    function donate() public payable {
        require(msg.value > 0, "Send some ether");

        Donor memory newDonor = Donor(msg.sender, msg.value);
        topDonors.push(newDonor);

        // Sort donors by amount (descending)
        for (uint i = 0; i < topDonors.length; i++) {
            for (uint j = i + 1; j < topDonors.length; j++) {
                if (topDonors[j].amount > topDonors[i].amount) {
                    Donor memory temp = topDonors[i];
                    topDonors[i] = topDonors[j];
                    topDonors[j] = temp;
                }
            }
        }

        // Keep only top 3 donors
        if (topDonors.length > 3) {
            topDonors.pop();
        }
    }

    function getTopDonors() public view returns (Donor[] memory) {
        return topDonors;
    }
}

```

## deployment
![Screenshot 2025-05-19 141405](https://github.com/user-attachments/assets/df7540b8-c3f9-4d68-b65f-71d5a8259236)
![Screenshot 2025-05-19 141455](https://github.com/user-attachments/assets/7e5574c4-6600-418d-b405-938323c0ced1)



# Question - 5
Implement a simple auction system where users can place bids, and the highest bidder wins.


## Contracts
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Auction {
    address public highestBidder;
    uint public highestBid;
    address public owner;
    bool public ended;

    constructor() {
        owner = msg.sender;
    }

    function bid() public payable {
        require(!ended, "Auction ended");
        require(msg.value > highestBid, "Bid too low");

        // Refund the previous highest bidder
        if (highestBid != 0) {
            payable(highestBidder).transfer(highestBid);
        }

        highestBidder = msg.sender;
        highestBid = msg.value;
    }

    function endAuction() public {
        require(msg.sender == owner, "Only owner can end the auction");
        require(!ended, "Auction already ended");

        ended = true;
        payable(owner).transfer(highestBid);
    }
}

```

## deployment
![Screenshot 2025-05-19 141822](https://github.com/user-attachments/assets/3f1b7c67-9f4e-40c3-a367-501dbbf29455)
![Screenshot 2025-05-19 141851](https://github.com/user-attachments/assets/5fd1a7a2-5ff8-410b-8ed4-b4e53fcab7a6)



# Question - 6
Create a contract that splits incoming Ether between 3 fixed addresses.


## Contracts
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract EtherSplitter {
    address payable public addr1;
    address payable public addr2;
    address payable public addr3;

    constructor(address payable _addr1, address payable _addr2, address payable _addr3) {
        addr1 = _addr1;
        addr2 = _addr2;
        addr3 = _addr3;
    }

    receive() external payable {
        uint share = msg.value / 3;
        addr1.transfer(share);
        addr2.transfer(share);
        addr3.transfer(msg.value - 2 * share); // to handle any remainder
    }
}

```

## deployment
![Screenshot 2025-05-19 142428](https://github.com/user-attachments/assets/4e104b21-6a8c-47b5-b217-8f0adcdd6954)
![Screenshot 2025-05-19 142520](https://github.com/user-attachments/assets/e7d4bf94-fab5-45b6-89d8-932e4e345cb7)

















