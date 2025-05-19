## Pratical questions 
1.Create a voting system with multiple candidates. Each address can vote only once.

2.Write a contract that manages a list of student records (name, roll number). Allow adding and retrieving student data.

3.Develop a contract that only allows the deployer (owner) to call a specific function (use modifiers).

4.Write a contract where people can donate Ether and the top 3 donors are tracked.

5.Implement a simple auction system where users can place bids, and the highest bidder wins.

6.Create a contract that splits incoming Ether between 3 fixed addresses.

 ## pratical 1
  codes -- 
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
