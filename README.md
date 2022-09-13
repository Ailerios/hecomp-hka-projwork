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
Execution time is specified in milliseconds.

#### Ciphertext-Ciphertext:

|                       | HElayers              | Pyfhel              | Microsoft SEAL      |
|-----------------------|-----------------------|---------------------|---------------------|
| Addition              | 0.16214776039123535   | 0.1597461700439453  | 0.14014625549316407 |
| Subtraction           | 0.18151068687438966   | 0.15580034255981446 | 0.155562162399292   |
| Multiplication        | 9.631774425506591     | 24.495131731033324  | 4.981326103210449   |

#### Single Ciphertext:

|                       | HElayers              | Pyfhel              | Microsoft SEAL      |
|-----------------------|-----------------------|---------------------|---------------------|
| Squaring              | 12.601945877075195    | 14.476970195770263  | 3.8471388816833495  |
| Negation              | 0.13540172576904298   | 16.452288389205934  | 0.13311004638671874 |

#### Ciphertext-Plaintext:

|                       | HElayers              | Pyfhel              | Microsoft SEAL      |
|-----------------------|-----------------------|---------------------|---------------------|
| Addition              | 0.09990882873535156   | 0.30337023735046385 | 0.12795901298522948 |
| Subtraction           | 0.10140013694763184   | 0.3138706684112549  | 0.13303422927856445 |
| Multiplication        | 3.941279411315918     | 0.2059304714202881  | 3.652517557144165   |


### Deviation


[^1]: https://github.com/IBM/helayers
[^2]: https://github.com/ibarrond/Pyfhel
[^3]: https://github.com/Huelse/SEAL-Python
