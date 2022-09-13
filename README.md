# hecomp-hka-projwork
Comparison of HE frameworks

We are comparing the following homomorphic encryption frameworks:
* IBM HElayers[^1]
* Pyfhel[^2]
* Microsoft SEAL for Python[^3]

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
Execution time is specified in seconds.

#### Ciphertext-Ciphertext:

|                       | HElayers               | Pyfhel | Microsoft SEAL |
|-----------------------|------------------------|--------|----------------|
| Addition              | 0.00016214776039123535 |        |                |
| Subtraction           | 0.00018151068687438966 |        |                |
| Multiplication        | 0.009631774425506591   |        |                |

#### Single Ciphertext:

|                       | HElayers               | Pyfhel | Microsoft SEAL |
|-----------------------|------------------------|--------|----------------|
| Squaring              | 0.012601945877075195   |        |                |
| Negation              | 0.00013540172576904298 |        |                |

#### Ciphertext-Plaintext:

|                       | HElayers               | Pyfhel | Microsoft SEAL |
|-----------------------|------------------------|--------|----------------|
| Addition              | 0.00009990882873535156 |        |                |
| Subtraction           | 0.00010140013694763184 |        |                |
| Multiplication        | 0.003941279411315918   |        |                |


### Deviation


[^1]: https://github.com/IBM/helayers
[^2]: https://github.com/ibarrond/Pyfhel
[^3]: https://github.com/Huelse/SEAL-Python
