MeshPay is a Spring Boot project that simulates offline UPI-style payments
using a mesh network.

The goal was to explore:
- Secure payment transmission
- End-to-end encryption
- Duplicate transaction prevention
- Distributed message propagation

The system encrypts payment instructions using RSA and AES,
routes packets through simulated devices,
and settles transactions when a bridge node regains internet connectivity.


ARCHITECTURE DIAGRAM 
┌──────────────────────────┐
│      Sender Phone        │
│   Alice sends ₹500       │
└────────────┬─────────────┘
             │
             │ Encrypt (RSA + AES)
             ▼
┌──────────────────────────┐
│      Mesh Packet         │
│  Encrypted Transaction   │
└────────────┬─────────────┘
             │
             ▼
┌─────────┐   ┌─────────┐   ┌─────────┐
│ Phone A │──▶│ Phone B │──▶│ Phone C │
└─────────┘   └─────────┘   └─────────┘
       Gossip-Based Propagation
             │
             ▼
┌──────────────────────────┐
│       Bridge Node        │
│ Regains Internet Access  │
└────────────┬─────────────┘
             │ HTTPS Upload
             ▼
┌──────────────────────────┐
│   Spring Boot Backend    │
├──────────────────────────┤
│ SHA-256 Hash Check       │
│ Idempotency Validation   │
│ AES/RSA Decryption       │
│ Replay Protection        │
│ Transaction Settlement   │
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│     Ledger Database      │
│  Debit / Credit Records  │
└──────────────────────────┘


PROJECT WORKFLOW
Create Payment
      │
      ▼
Encrypt Transaction
      │
      ▼
Inject Into Mesh
      │
      ▼
Gossip Propagation
      │
      ▼
Bridge Upload
      │
      ▼
Duplicate Check
      │
      ▼
Decrypt Packet
      │
      ▼
Validate Freshness
      │
      ▼
Settle Transaction
      │
      ▼
Update Ledger


KEY FEATURES 

Secure End-to-End Encryption
RSA-2048 key exchange
AES-256-GCM payload encryption
Intermediate devices cannot view transaction data


Offline Payment Routing
Mesh network simulation
Device-to-device packet forwarding
TTL-based propagation


Exactly-Once Settlement
SHA-256 packet hashing
Idempotency cache
Duplicate packet rejection


Replay Attack Protection
Timestamp validation
Nonce generation
Expired packet rejection


Concurrent Processing Safety
Transactional settlement
Optimistic locking
Multi-thread duplicate handling


Interactive Dashboard
Send payments
Run gossip rounds
Upload bridge packets
Monitor balances
View transaction history


Technology            Stack

Layer	                Technology
Backend	              Java 17
Framework	            Spring Boot
ORM	                  Spring Data JPA
Database	            H2 Database
Build Tool	          Maven
Security	            RSA-OAEP, AES-GCM
Frontend	            Thymeleaf
Testing	              JUnit


Security Design

Payment Instruction
        │
        ▼
Generate AES Key
        │
        ▼
Encrypt Payload (AES-GCM)
        │
        ▼
Encrypt AES Key (RSA)
        │
        ▼
Create Mesh Packet
        │
        ▼
Forward Through Network
        │
        ▼
Backend Decrypts


Challenges Solved

Challenge	               Solution
No Internet	             Mesh-based forwarding
Untrusted Devices	       End-to-end encryption
Duplicate Delivery	     SHA-256 idempotency
Concurrent Uploads	     Atomic claim mechanism
Packet Tampering	       AES-GCM authentication
Replay Attacks	         Nonce + timestamp validation
