# Hash Function Comparative Analysis Report
Generated on: 2025-04-18 00:31:39

## System Information
- Platform: linux
- Python Version: 3.10.12 (main, Feb  4 2025, 14:57:36) [GCC 11.4.0]
- CPU Count: 12

## Algorithm Technical Details

### SHA-256 (FIPS 180-4)
**Family**: SHA-2
**Structure**: Merkle-Damgård construction with Davies-Meyer compression
**Designer**: National Security Agency (NSA)
**Standardization**: NIST FIPS 180-4 (August 2015)

#### Specifications
- **Digest Size**: 256 bits (32 bytes)
- **Block Size**: 512 bits (64 bytes)
- **Word Size**: 32 bits
- **Rounds**: 64 rounds
- **Operations**: Bitwise AND, OR, XOR, NOT, modular addition, rotations, shifts

#### Security Properties
- **Collision Resistance**: 128 bits (classical), 85 bits (quantum with Grover's)
- **Preimage Resistance**: 256 bits (classical), 128 bits (quantum with Grover's)
- **Known Attacks**: None that break full rounds; best attacks are on reduced rounds
- **Length Extension**: Vulnerable without proper implementation measures

**Applications**: TLS, SSH, digital signatures, blockchain, file integrity

### SHA-512 (FIPS 180-4)
**Family**: SHA-2
**Structure**: Merkle-Damgård construction with Davies-Meyer compression
**Designer**: National Security Agency (NSA)
**Standardization**: NIST FIPS 180-4 (August 2015)

#### Specifications
- **Digest Size**: 512 bits (64 bytes)
- **Block Size**: 1024 bits (128 bytes)
- **Word Size**: 64 bits
- **Rounds**: 80 rounds
- **Operations**: Bitwise AND, OR, XOR, NOT, modular addition, rotations, shifts

#### Security Properties
- **Collision Resistance**: 256 bits (classical), 128 bits (quantum with Grover's)
- **Preimage Resistance**: 512 bits (classical), 256 bits (quantum with Grover's)
- **Known Attacks**: None that break full rounds; best attacks are on reduced rounds
- **Length Extension**: Vulnerable without proper implementation measures

**Applications**: High-security applications, PKI, HMAC, password hashing

### Quantum-Safe (SHA-512 + BLAKE3)
**Family**: Composite hash (SHA-2 + BLAKE3)
**Structure**: Sequential composition with concatenation
**Designer**: QUANTM Project (combining NIST and BLAKE3 team designs)
**Standardization**: Custom design based on FIPS 180-4 and BLAKE3 specification

#### Specifications
- **Digest Size**: 256 bits (32 bytes) from BLAKE3 output
- **First Stage**: SHA-512 with 512-bit output
- **Second Stage**: BLAKE3 with input: SHA-512(message) || message
- **Internal State**: BLAKE3 uses 8 x 4 x 32-bit state matrix
- **Operations**: ARX (Addition, Rotation, XOR) operations from ChaCha

#### Security Properties
- **Collision Resistance**: Minimum of both algorithms - 256 bits classical
- **Preimage Resistance**: Enhanced through composition - stronger than individual functions
- **Quantum Resistance**: At least 128 bits against Grover's algorithm
- **Composition Advantage**: Defense in depth; attacker must break both algorithms
- **Side Channel Protection**: BLAKE3 designed with constant-time implementation

#### BLAKE3 Algorithm Details
- **Designer**: Jack O'Connor, Jean-Philippe Aumasson, Samuel Neves, Zooko Wilcox-O'Hearn
- **Year**: 2020
- **Features**: Parallelizable, SIMD-optimized, built-in keying and tree hashing
- **Speed**: Typically 4-8x faster than SHA-2 on modern CPUs

**Applications**: Post-quantum cryptographic systems, zero-knowledge proofs, blockchain

## Performance Analysis
Average execution time and throughput:

```
+---------------------------------+---------------------+-----------------+-----------------------+-------------------+------------------------+--------------------+--------------------------+----------------------+
| Hash Function                   |   64 bytes Time (s) |   64 bytes MB/s |   1024 bytes Time (s) |   1024 bytes MB/s |   16384 bytes Time (s) |   16384 bytes MB/s |   1048576 bytes Time (s) |   1048576 bytes MB/s |
+=================================+=====================+=================+=======================+===================+========================+====================+==========================+======================+
| SHA-256 (FIPS 180-4)            |          5.282e-07  |          115.56 |            9.832e-07  |            993.24 |            8.7114e-06  |            1793.63 |              0.000522298 |              1914.61 |
+---------------------------------+---------------------+-----------------+-----------------------+-------------------+------------------------+--------------------+--------------------------+----------------------+
| SHA-512 (FIPS 180-4)            |          6.614e-07  |           92.28 |            1.7357e-06 |            562.64 |            1.77757e-05 |             879.01 |              0.00111386  |               897.78 |
+---------------------------------+---------------------+-----------------+-----------------------+-------------------+------------------------+--------------------+--------------------------+----------------------+
| Quantum-Safe (SHA-512 + BLAKE3) |          1.6299e-06 |           37.45 |            3.7669e-06 |            259.25 |            2.32886e-05 |             670.93 |              0.00155445  |               643.31 |
+---------------------------------+---------------------+-----------------+-----------------------+-------------------+------------------------+--------------------+--------------------------+----------------------+
```

## Entropy Analysis
Score closer to 1.0 indicates better randomness distribution:

```
+---------------------------------+-----------------+---------------+-----------+----------+--------------------+
| Hash Function                   |   Entropy Score |   Chi-Squared | Zeros %   | Ones %   | Distribution       |
+=================================+=================+===============+===========+==========+====================+
| SHA-256 (FIPS 180-4)            |               1 |        0.0023 | 50.24%    | 49.76%   | OPTIMAL (p > 0.05) |
+---------------------------------+-----------------+---------------+-----------+----------+--------------------+
| SHA-512 (FIPS 180-4)            |               1 |        0.0006 | 50.13%    | 49.87%   | OPTIMAL (p > 0.05) |
+---------------------------------+-----------------+---------------+-----------+----------+--------------------+
| Quantum-Safe (SHA-512 + BLAKE3) |               1 |        0.0001 | 50.06%    | 49.94%   | OPTIMAL (p > 0.05) |
+---------------------------------+-----------------+---------------+-----------+----------+--------------------+
```

## Avalanche Effect
Ideal score is 50% bit changes (lower ideal_score is better):

```
+---------------------------------+--------------+----------------+-----------+---------------+
| Hash Function                   | Input Size   | Bit Change %   |   Std Dev |   Ideal Score |
+=================================+==============+================+===========+===============+
| SHA-256 (FIPS 180-4)            | 64 bytes     | 49.90%         |      2.95 |          0.1  |
+---------------------------------+--------------+----------------+-----------+---------------+
| SHA-256 (FIPS 180-4)            | 1024 bytes   | 49.94%         |      3.18 |          0.06 |
+---------------------------------+--------------+----------------+-----------+---------------+
| SHA-512 (FIPS 180-4)            | 64 bytes     | 50.20%         |      1.96 |          0.2  |
+---------------------------------+--------------+----------------+-----------+---------------+
| SHA-512 (FIPS 180-4)            | 1024 bytes   | 50.03%         |      2.25 |          0.03 |
+---------------------------------+--------------+----------------+-----------+---------------+
| Quantum-Safe (SHA-512 + BLAKE3) | 64 bytes     | 49.94%         |      2.84 |          0.06 |
+---------------------------------+--------------+----------------+-----------+---------------+
| Quantum-Safe (SHA-512 + BLAKE3) | 1024 bytes   | 49.84%         |      3.31 |          0.16 |
+---------------------------------+--------------+----------------+-----------+---------------+
```

## Conclusion
Insufficient data to generate overall scores.