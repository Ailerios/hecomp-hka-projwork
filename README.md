# Homomorphic Encryption Frameworks Compared

In this project we are comparing the following homomorphic encryption frameworks:
* IBM HElayers[^1]
* Pyfhel[^2]
* Microsoft SEAL for Python[^3]

Test code is provided in the sourcecode, including a Docker file containing all three frameworks, so you can get started quickly.

## Frameworks Overview

### IBM HElayers
(Middle layer, offers auto-features)

### Pyfhel
(Pyfhel is the easiest framework to get into)

### Microsoft SEAL
(Most deep framework of the three)

### Available operations (table)

## Measured Data

### Testing System

Hardware:
- Device: Acer Aspire 5 A515 54G 50F2
- CPU: Intel Core i5-10210U @ 1.60GHz; 4 cores, 8 threads
- RAM: 16 GB
- Motherboard: V1.16 / Doc_WC (CML)

Software:
- OS: Ubuntu 20.04.5 LTS
- Runtime: Jupyter Notebook inside Docker container
- Language: Python

### Configuration
8192 slots have been used for all three frameworks.

#### HElayers

For HElayers, fractional_part_precision had to be decreased to 39, and integer_part_precision increased to 21 in order to support the calculation of 100\*100.

Here are the used values:

- num_slots = 8192
- multiplication_depth = 2
- fractional_part_precision = 39
- integer_part_precision = 21
- security_level = 128
- Context: DefaultContext



### Execution time
Average execution time of 1000 operations, except squaring, of which 500 operations have been executed. Time is specified in milliseconds and rounded to 6 decimals.

#### Ciphertext-Ciphertext:

|                       | HElayers  | Pyfhel    | Microsoft SEAL  |
|-----------------------|-----------|-----------|-----------------|
| Addition              | 0.155078  | 0.150896  | 0.143598        |
| Subtraction           | 0.165524  | 0.151160  | 0.167902        |
| Multiplication        | 9.509052  | 20.858954 | 6.683598        |

#### Single Ciphertext:

|                       | HElayers  | Pyfhel    | Microsoft SEAL  |
|-----------------------|-----------|-----------|-----------------|
| Squaring              | 11.775293 | 16.266907 | 4.447789        |
| Negation              | 0.146339  | 16.037625 | 0.148114        |

#### Ciphertext-Plaintext:

|                       | HElayers  | Pyfhel    | Microsoft SEAL  |
|-----------------------|-----------|-----------|-----------------|
| Addition              | 0.113416  | 0.366492  | 0.164739        |
| Subtraction           | 0.110463  | 0.379061  | 0.173108        |
| Multiplication        | 4.859381  | 0.219237  | 3.945497        |


### Deviation


[^1]: https://github.com/IBM/helayers
[^2]: https://github.com/ibarrond/Pyfhel
[^3]: https://github.com/Huelse/SEAL-Python
