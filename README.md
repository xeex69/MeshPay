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
