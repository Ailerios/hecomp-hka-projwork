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

|                       | HElayers               | Pyfhel                 | Microsoft SEAL          |
|-----------------------|------------------------|------------------------|-------------------------|
| Addition              | 0.00016214776039123535 | 0.0001597461700439453  | 0.00014014625549316407  |
| Subtraction           | 0.00018151068687438966 | 0.00015580034255981446 | 0.000155562162399292    |
| Multiplication        | 0.009631774425506591   | 0.024495131731033324   | 0.004981326103210449    |

#### Single Ciphertext:

|                       | HElayers               | Pyfhel                 | Microsoft SEAL          |
|-----------------------|------------------------|------------------------|-------------------------|
| Squaring              | 0.012601945877075195   | 0.014476970195770263   | 0.0038471388816833495   |
| Negation              | 0.00013540172576904298 | 0.016452288389205934   | 0.00013311004638671874  |

#### Ciphertext-Plaintext:

|                       | HElayers               | Pyfhel                 | Microsoft SEAL          |
|-----------------------|------------------------|------------------------|-------------------------|
| Addition              | 0.00009990882873535156 | 0.00030337023735046385 | 0.00012795901298522948  |
| Subtraction           | 0.00010140013694763184 | 0.0003138706684112549  | 0.00013303422927856445  |
| Multiplication        | 0.003941279411315918   | 0.0002059304714202881  | 0.003652517557144165    |


### Deviation


[^1]: https://github.com/IBM/helayers
[^2]: https://github.com/ibarrond/Pyfhel
[^3]: https://github.com/Huelse/SEAL-Python
