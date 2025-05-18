# solidity-assignments-anjitha-0123
solidity-assignments-anjitha-0123 created by GitHub Classroom

## Verified Contract Link

https://sepolia.etherscan.io/verifyContract-solc?a=0x3F3aB14b6E4Df43F1Fc0680b25F849cAb71a9281&c=v0.8.30%2bcommit.73712a01&lictype=3

## Smart Contract
```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.30;

contract studentManagement{
     enum Dept {CS,EC,ME}

     struct Student{
        string name;
        string sem;
        Dept dep;
        uint cgpa;
        uint rollno;
        address studentAddress;
    }

    address public staff;

    mapping (uint=>Student) public students;

    uint[] private  studentRollNumber;

    mapping (address=>uint )private addressToRoll;

    constructor() {
        staff=msg.sender;
    }

    modifier onlyStaff(){
        require(msg.sender==staff,"Unautherised Access");
        _;
    }

    modifier onlyStudent(uint _rollno){
        require(students[_rollno].studentAddress==msg.sender,"Unautherised Access");
        _;
    }
    function setStudent(
        string memory _name,
        string memory _sem,
        Dept _dep,
        uint _cgpa,
        uint _rollno,
        address _studentAddress
    ) public onlyStaff {
        bool isNew = students[_rollno].rollno == 0;

        students[_rollno] = Student(_name, _sem, _dep, _cgpa, _rollno, _studentAddress);
        addressToRoll[_studentAddress] = _rollno;

        if (isNew) {
            studentRollNumber.push(_rollno);
        }
    }
    function editMyName(string memory _newName) public {
        uint rollno = addressToRoll[msg.sender];
        require(rollno != 0, "You are not registered as a student.");
        students[rollno].name = _newName;
    }
    function getStudentDetails(uint _rollno) public view onlyStaff returns (
        string memory name,
        string memory sem,
        Dept dep,
        uint cgpa,
        uint rollno,
        address studentAddress
    ) {
        Student memory s = students[_rollno];
        require(s.rollno != 0, "Student not found.");
        return (s.name, s.sem, s.dep, s.cgpa, s.rollno, s.studentAddress);
    }

   
}
    
```
