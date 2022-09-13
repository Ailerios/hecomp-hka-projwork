# Homomorphic Encryption Frameworks Compared

In this project we are comparing the following homomorphic encryption frameworks:
* IBM HElayers[^1]
* Pyfhel[^2]
* Microsoft SEAL for Python[^3]

Test code is provided in the sourcecode, including a Docker file containing all three frameworks, so you can get started quickly.

## Frameworks Overview

### IBM HElayers

### Pyfhel

### Microsoft SEAL

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

### Execution time
Average execution time of 1000 operations with random integers between 1 and 100, except squaring, of which 500 operations have been executed. Time is specified in milliseconds and rounded to 6 decimals.

#### Ciphertext-Ciphertext:

|                       | HElayers  | Pyfhel    | Microsoft SEAL  |
|-----------------------|-----------|-----------|-----------------|
| Addition              | 0.162148  | 0.159746  | 0.140146        |
| Subtraction           | 0.181511  | 0.1558    | 0.155562        |
| Multiplication        | 9.631774  | 24.495132 | 4.981326        |

#### Single Ciphertext:

|                       | HElayers  | Pyfhel    | Microsoft SEAL  |
|-----------------------|-----------|-----------|-----------------|
| Squaring              | 12.601946 | 14.476970 | 3.847139        |
| Negation              | 0.135402  | 16.452288 | 0.133110        |

#### Ciphertext-Plaintext:

|                       | HElayers  | Pyfhel    | Microsoft SEAL  |
|-----------------------|-----------|-----------|-----------------|
| Addition              | 0.099909  | 0.303370  | 0.127959        |
| Subtraction           | 0.101400  | 0.313871  | 0.133034        |
| Multiplication        | 3.941279  | 0.205930  | 3.652518        |


### Deviation


[^1]: https://github.com/IBM/helayers
[^2]: https://github.com/ibarrond/Pyfhel
[^3]: https://github.com/Huelse/SEAL-Python
