# AES Encryption and Decryption with UART

## FPGA-Based AES-128 Encryption/Decryption with UART Communication

This project implements **AES-128 encryption and decryption on an FPGA**, with **UART communication** used to transfer 128-bit plaintext, ciphertext, and key data between a PC and the FPGA.

The project integrates an AES hardware core with UART transmit and receive logic, allowing a user to send encryption data from a PC, process it on the FPGA, and receive the result back through UART.

---

## Key Features

* AES-128 encryption
* AES-128 decryption
* 128-bit plaintext
* 128-bit encryption key
* 128-bit ciphertext
* UART-based data transfer
* FPGA implementation
* Verilog RTL design
* Simulation and verification using a testbench
* PC ↔ FPGA communication

---

## System Architecture

```text
              PC / Terminal
                   |
                   | UART
                   v
             +-----------+
             |  UART RX  |
             +-----+-----+
                   |
                   v
          +------------------+
          |    AES-128 Core  |
          |                  |
          | Encryption       |
          | Decryption       |
          +--------+---------+
                   |
                   v
             +-----------+
             |  UART TX  |
             +-----+-----+
                   |
                   | UART
                   v
              PC / Terminal
```

---

# AES-128 Overview

AES (Advanced Encryption Standard) is a symmetric block cipher standardized by NIST.

AES-128 uses:

```text
Block Size  : 128 bits
Key Size    : 128 bits
Rounds      : 10
```

The same secret key is used for both encryption and decryption.

```text
Plaintext + Key
      |
      v
   AES-128
      |
      v
 Ciphertext
```

For decryption:

```text
Ciphertext + Key
      |
      v
   AES-128
      |
      v
 Plaintext
```

---

# AES Encryption

AES-128 operates on a 128-bit data block and performs multiple transformation rounds.

The main AES operations are:

1. SubBytes
2. ShiftRows
3. MixColumns
4. AddRoundKey

The final round does not include MixColumns.

```text
128-bit Plaintext
       |
       v
 AddRoundKey
       |
       v
  AES Round 1
       |
       v
  AES Round 2
       |
       .
       .
       .
       |
       v
 AES Round 10
       |
       v
128-bit Ciphertext
```

---

# AES Decryption

The inverse transformations are used during decryption:

* InvSubBytes
* InvShiftRows
* InvMixColumns
* AddRoundKey

The decryption process reconstructs the original plaintext from the ciphertext.

```text
Ciphertext
     |
     v
AES Decryption
     |
     v
Plaintext
```

---

# UART Communication

UART is used as the interface between the PC and FPGA.

The UART interface provides:

```text
PC
 |
 | Serial Data
 v
UART RX
 |
 v
AES Core
 |
 v
UART TX
 |
 | Serial Data
 v
PC
```

Since AES operates on **128-bit blocks**, the UART interface handles the transfer of the required 128-bit information between the serial interface and the AES datapath.

---

# Data Flow

### Encryption

```text
PC
 |
 | Plaintext + Key
 v
UART RX
 |
 v
AES Encryption
 |
 v
Ciphertext
 |
 v
UART TX
 |
 v
PC
```

### Decryption

```text
PC
 |
 | Ciphertext + Key
 v
UART RX
 |
 v
AES Decryption
 |
 v
Plaintext
 |
 v
UART TX
 |
 v
PC
```

---

# Hardware Architecture

The overall design consists of three major sections:

```text
+-------------------+
|   UART Interface  |
|                   |
| RX          TX    |
+----+---------+----+
     |         ^
     v         |
+-------------------+
|     AES Core      |
|                   |
| Encryption        |
| Decryption        |
| Key Expansion     |
+-------------------+
```

The UART logic handles communication, while the AES core performs the cryptographic computation.

---

# AES Key Expansion

AES-128 generates a set of round keys from the original 128-bit key.

The original key is expanded into the required round keys used during the 10 AES rounds.

```text
128-bit Key
     |
     v
Key Expansion
     |
     +--> Round Key 0
     +--> Round Key 1
     +--> Round Key 2
     |
     .
     .
     +--> Round Key 10
```

---

# Verification

The design is verified using simulation by applying known plaintext/key combinations and checking the resulting ciphertext.

A typical verification flow is:

```text
Testbench
    |
    v
Plaintext + Key
    |
    v
AES Encryption
    |
    v
Compare Ciphertext
    |
    v
PASS / FAIL
```

Decryption is then verified by applying the generated ciphertext:

```text
Ciphertext + Key
       |
       v
AES Decryption
       |
       v
Compare with Original Plaintext
       |
       v
PASS / FAIL
```

---

# Example AES Verification

A standard AES-128 test vector can be used to verify the implementation.

```text
Key:
000102030405060708090A0B0C0D0E0F

Plaintext:
00112233445566778899AABBCCDDEEFF
```

Expected ciphertext:

```text
69C4E0D86A7B0430D8CDB78070B4C55A
```

The testbench can compare the AES output against this known reference value.

---

# Project Structure

```text
AES-with-UART/
│
├── AES RTL files
├── UART TX/RX modules
├── Top-level integration
├── Testbench
├── Simulation files
├── FPGA constraints
└── README.md
```

The repository contains the AES and UART RTL required to integrate the cryptographic datapath with the serial communication interface.

---

# Tools Used

* **Verilog HDL**
* **Xilinx Vivado**
* **FPGA Development Board**
* **UART Serial Communication**
* **Simulation/Testbench**

---

# What This Project Demonstrates

This project provides practical experience with:

* AES cryptographic hardware
* RTL design
* Hardware/software communication
* UART protocol
* 128-bit datapaths
* Key expansion
* Encryption and decryption
* FPGA integration
* RTL simulation
* Hardware verification

---

# Possible Applications

AES hardware acceleration can be used in:

* Secure IoT devices
* Embedded systems
* Secure communication
* Hardware security modules
* Network security
* SoC security accelerators
* FPGA-based cryptographic systems

---

# Future Improvements

Possible extensions include:

* Higher-speed UART communication
* AXI4-Lite interface for SoC integration
* AXI-Stream interface
* FIFO-based UART buffering
* AES-256 support
* Hardware performance optimization
* Pipelined AES architecture
* Integration with a RISC-V processor
* Secure IoT gateway integration

---

# Author

**Gunta Bhumesh**

Electronics & Communication Engineering

**Interests:**
VLSI | RTL Design | FPGA | SystemVerilog | UVM | SoC Design | Hardware Security

## Repository

[GitHub Repository](https://github.com/guntabhumesh/AES-with-UART)
