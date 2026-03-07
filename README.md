🔒 ASCON-IoT: Secure Sensor Communication

An implementation of secure, authenticated end-to-end communication between IoT nodes using the ASCON-128 (AEAD) lightweight cryptographic cipher. Designed for resource-constrained hardware like the ESP32.

==============================================================================
🚀 Overview

This project demonstrates a secure pipeline for transmitting sensitive sensor data across a Wi-Fi network. By utilizing Authenticated Encryption with Associated Data (AEAD), we ensure that data is not only kept private but also protected against tampering and injection attacks.

==============================================================================
🛠️ The Secure Pipeline
1. Data Acquisition & Packetization

Node A (The Producer) reads environmental data via I2C/SPI. The data is packed into a compact binary structure to minimize transmission overhead.

Plaintext Structure:
| Field | Type | Size | Description |
| :--- | :--- | :--- | :--- |
| SensorID | uint8_t | 1 Byte | Unique Identifier for the node |
| Temperature| float | 4 Bytes | IEEE 754 Floating point value |
| Timestamp | uint32_t| 4 Bytes | Unix epoch or sequence counter |
2. ASCON-128 Encryption (AEAD)

Before leaving the device, the plaintext is processed through the ASCON algorithm.

    Encryption: Transforms plaintext into ciphertext using a 128-bit Secret Key and a unique Nonce.

    Authentication: Generates a 128-bit Tag. This tag acts as a "digital seal." If even one bit of the ciphertext is changed during transit, the tag verification will fail on Node B.

3. Transmission (Wi-Fi/UDP)

The encrypted payload is transmitted over the network.

    Security Note: Even if an attacker performs a "Man-in-the-Middle" (MitM) attack or sniffs the Wi-Fi traffic, they only see the Nonce and the encrypted Ciphertext. Without the Secret Key, the data is computationally impossible to revert to plaintext.

4. Decryption & Integrity Verification

Node B (The Receiver) receives the packet and attempts to decrypt it.

    Success: If the Tag matches, the data is unpacked and forwarded (e.g., to a Serial Dashboard or Database).

    Failure: If the Tag does not match, the packet is dropped immediately. This prevents "Replay Attacks" or corrupted data from affecting the system.
==============================================================================
📊 Performance Evaluation

One of the primary goals of this project is to measure the overhead of lightweight cryptography on embedded systems.
