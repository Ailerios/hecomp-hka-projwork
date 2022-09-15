# Homomorphic Encryption Libraries Compared

In this project we are comparing the following three homomorphic encryption libraries for Python:
* IBM HElayers[^1]
* Pyfhel[^2]
* SEAL-Python[^3]

Test code is provided in the sourcecode, including a Docker file containing all three libraries, so you can get started quickly.

## Frameworks Overview

### IBM HElayers
HElayers is a library that enables developers to use fully homomorphic encryption with Machine Learning, without requiring specialized cryptographic knowledge. In addition to a low-level API for manipulating ciphertexts directly, it offers features for streamlined usage of machine learning with homomorphic encryption. It is delivered via a docker image that already contains demos of features like Credit Card Fraud Detection, Privacy Database Search, Text Classification, Heart Disease Prediction in different ways, such as with a Neural Network, Logistic or Linear Regression. It is free for non-commercial purposes, for commercial purposes you will need to obtain a paid license. Images are provided for IBM's cloud s390x and x86 architectures, for Python "helayers-pylab" and C++ "helayers-lab"[^4]. HElayers does not have a designated power operation, but it relinearizes automatically and applies optimizations. There are also raw operations available.

In short:

#### Advantages
- Specialized tools for applying Homomorphic Encryption to Machine Learning
- Applies some optimizations
#### Disadvantages
- Slightly longer execution time per operation than SEAL-Python
- Requires a paid license for commercial purposes

### Pyfhel
Pyfhel is a library that uses Microsoft SEAL in the background and provides easy access to homomorphic encryption functionality. It uses a syntax similar to normal arithmetic such as \*, +, -, >>, \*\*. It supports Integer FHE with BFV and Fixed-point FHE with CKKS. Thus it is an easy library to get started quickly, however as the numbers below show, it is generally slower than HElayers and SEAL. The docker file contains only one example how to use Pyfhel with Integer FHE via BFV, however there are more extensive tutorials available online[^5].

#### Advantages
- Easy to start
- Tutorials available online

#### Disadvantages
- Very slow execution times compared to HElayers and SEAL-Python

### SEAL-Python
SEAL-Python is a lightweight python binding for the Microsoft SEAL framework. The image provides examples for basic BGV arithmetics, matrix operations and serialization. There is little documentation available, so you might need to dig through some sourcecode to get all of its features, however it's not complicated - the wrapper only has 687 lines. SEAL-Python does not have a designated power operation.

#### Advantages
- Fastest execution times compared to Pyfhel and HElayers
- Offers easy access to some of Microsoft SEAL's functionality including matrix operations
- Extremely lightweight

#### Disadvantages
- Limited functionality compared to Pyfhel and HElayers

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

For HElayers, fractional_part_precision had to be decreased from 40 (standard) to 39, and integer_part_precision increased from 20 (standard) to 21 in order to support the calculation of 100\*100. Without, overflow happened and resulted in negative results with the multiplication of two positive integers.

Similar changes had to be conducted with Pyfhel and Microsoft SEAL for the same reason: The number of bits for the plaintext modulus has been increased from 20 (standard) to 22. In Pyfhel the attribute is called t_bits, in Microsoft SEAL it is passed as a parameter (refer to "CPerfSeal.ipynb" for details).

Here are the configuration details for all three frameworks:

#### HElayers
- scheme: CKKS
- num_slots = 8192
- multiplication_depth = 2
- fractional_part_precision = 39
- integer_part_precision = 21
- security_level = 128

#### Pyfhel
- scheme: BFV
- n (number of slots): 8192
- t (plaintext modulus): 65537
- t_bits (number of bits in t): 22
- sec (equivalent length of AES key in bits): 128

#### Microsoft SEAL:
- scheme: BFV
- poly_modulus_degree: 8192
- number of bits for plaintext modulus: 22
- row_size: slot_count / 2

### Tests
500 cycles with 2 operations per cycle with the same encrypted object. Exception is squaring, where only 1 operation per cycle is performed. New cycle means new encryption of fresh data. More operations per cycle resulted in overflows and negative results.

A deviation was only measurable with HElayers, Pyfhel and SEAL-Python had a deviation of 0.0 when the right parameters were applied.

Average execution time is specified in milliseconds and rounded to 6 decimals.

#### Test 1
Test 1 is performed with a random integer between 1 and 100 for each slot. In each cycle all slots are filled with new random integers.

##### Ciphertext-Ciphertext:

|                       | HElayers  | Pyfhel    | SEAL-Python     | Deviation (HElayers)  |
|-----------------------|-----------|-----------|-----------------|-----------------------|
| Addition              | 0.155078  | 0.150896  | 0.143598        | 0.000032              |
| Subtraction           | 0.165524  | 0.151160  | 0.167902        | 0.000032              |
| Multiplication        | 9.509052  | 20.858954 | 6.683598        | 0.100574              |

##### Single Ciphertext:

|                       | HElayers  | Pyfhel    | SEAL-Python     | Deviation (HElayers)  |
|-----------------------|-----------|-----------|-----------------|-----------------------|
| Squaring              | 11.775293 | 16.266907 | 4.447789        | 0.002911              |
| Negation              | 0.146339  | 16.037625 | 0.148114        | 0.000014              |

##### Ciphertext-Plaintext:

|                       | HElayers  | Pyfhel    | SEAL-Python     | Deviation (HElayers)  |
|-----------------------|-----------|-----------|-----------------|-----------------------|
| Addition              | 0.113416  | 0.366492  | 0.164739        | 0.000014              |
| Subtraction           | 0.110463  | 0.379061  | 0.173108        | 0.000014              |
| Multiplication        | 4.859381  | 0.219237  | 3.945497        | 0.048829              |

#### Test 2
In Test 2, all slots are filled with the maximum integer 100.

##### Ciphertext-Ciphertext:

|                       | HElayers  | Pyfhel    | SEAL-Python     | Deviation (HElayers)  |
|-----------------------|-----------|-----------|-----------------|-----------------------|
| Addition              | 0.143021  | 0.119526  | 0.131236        | 0.000032
| Subtraction           | 0.164373  | 0.117806  | 0.146674        | 0.000032
| Multiplication        | 8.271257  | 19.068434 | 5.046285        | 0.354025

##### Single Ciphertext:

|                       | HElayers  | Pyfhel    | SEAL-Python     | Deviation (HElayers)  |
|-----------------------|-----------|-----------|-----------------|-----------------------|
| Squaring              | 10.835346 | 10.936287 | 4.119792        | 0.005780              |
| Negation              | 0.134604  | 17.419216 | 0.131858        | 0.000014              |

##### Ciphertext-Plaintext:

|                       | HElayers  | Pyfhel    | SEAL-Python     | Deviation (HElayers)  |
|-----------------------|-----------|-----------|-----------------|-----------------------|
| Addition              | 0.113639  | 0.385319  | 0.147074        | 0.000014              |
| Subtraction           | 0.109920  | 0.339111  | 0.150686        | 0.000014              |
| Multiplication        | 4.356075  | 0.203711  | 0.217033        | 0.144504              |


[^1]: https://github.com/IBM/helayers
[^2]: https://github.com/ibarrond/Pyfhel
[^3]: https://github.com/Huelse/SEAL-Python
[^4]: https://developer.ibm.com/blogs/secure-ai-workloads-using-fully-homomorphic-encrypted-data/
[^5]: https://pyfhel.readthedocs.io/en/latest/_autoexamples/index.html
