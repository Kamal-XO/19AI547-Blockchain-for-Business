# Experiment 1: Decentralized Certificate Verification
## Aim:
  To develop a smart contract for issuing and verifying academic certificates on Ethereum, preventing forgery and ensuring authenticity.
## Algorithm:
### STEP 1:
Deploy a smart contract where universities can issue certificates.
### STEP 2:
Store a hash of certificate data on-chain.
### STEP 3:
Provide a verification function that checks certificate authenticity.
### STEP 4:
Users can verify the certificate by comparing the stored hash.
## Program:
```
DATE : 16.04.2025
REG NO : 212223240141

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract CertificateVerification {
    address public university;  // University address

    // Mapping to store issued certificate hashes
    mapping(bytes32 => bool) public certificates;

    // Event emitted when a certificate is issued
    event CertificateIssued(bytes32 certHash);

    // Constructor sets the deployer (university)
    constructor() {
        university = msg.sender;
    }

    // Function to issue certificate (only university can call)
    function issueCertificate(string memory studentName, string memory degree, uint year) public {
        require(msg.sender == university, "Only university can issue certificates");

        // Create a unique hash of the certificate details
        bytes32 certHash = keccak256(abi.encodePacked(studentName, degree, year));

        certificates[certHash] = true;
        emit CertificateIssued(certHash);
    }

    // Function to verify if a certificate exists
    function verifyCertificate(string memory studentName, string memory degree, uint year) public view returns (bool) {
        bytes32 certHash = keccak256(abi.encodePacked(studentName, degree, year));
        return certificates[certHash];
    }
}
```
# Output:
![Screenshot 2025-04-16 092244](https://github.com/user-attachments/assets/45841338-4ccb-4bee-a84a-1c43b4d05a7f)

![Screenshot 2025-04-16 092209](https://github.com/user-attachments/assets/65336e3b-1c64-4c13-a71d-9b4e155af495)

![Screenshot 2025-04-16 091853](https://github.com/user-attachments/assets/085bcdbb-af45-43d4-9929-38cd69f423ad)

![Screenshot 2025-04-16 091733](https://github.com/user-attachments/assets/eb9b7f8a-59d8-4852-a132-683fa8c23c9a)

![Screenshot 2025-04-16 091756](https://github.com/user-attachments/assets/8185739e-110a-4dff-8b66-2647a6a5537e)

# Result:
Thus the decentralized certificate verification using remix is successfully implemented.
