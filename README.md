# Homomorphic Encryption Libraries Compared

In this project, we are comparing three homomorphic encryption libraries for Python. The goal is to learn, how accessible current homomorphic encryption libraries are to software engineers without formal education in cryptography.

The three libraries are:

* IBM HElayers[^1]
* Pyfhel[^2]
* SEAL-Python[^3]

Test code is provided in the sourcecode, including a Docker file containing all three libraries, so you can get started quickly. The docker file is about 5.6GB in uncompressed form. You can pull it via the command: ```docker pull ailerios/projwork_comp_he_libraries```

The link to this repository is: https://github.com/Ailerios/hecomp-hka-projwork

## Frameworks Overview

### IBM HElayers
HElayers is a library that enables developers to use fully homomorphic encryption with Machine Learning, without requiring specialized cryptographic knowledge. In addition to a low-level API for manipulating ciphertexts directly, it offers features for streamlined usage of machine learning with homomorphic encryption. It is delivered via a docker image that already contains demos of features like Credit Card Fraud Detection, Privacy Database Search, Text Classification, Heart Disease Prediction in different ways, such as with a Neural Network, Logistic or Linear Regression. It is free for non-commercial purposes, for commercial purposes you will need to obtain a paid license. Images are provided for IBM's cloud s390x and x86 architectures, for Python "helayers-pylab" and C++ "helayers-lab"[^4]. HElayers does not have a designated power operation, but it relinearizes automatically and applies optimizations. There are also raw operations available.

#### Advantages
- Specialized tools for applying Homomorphic Encryption to Machine Learning
- Applies some optimizations, quoting the class reference: _"may perform some additional light-weight tasks allowing for a smooth sequence of operations"_ [^5]
#### Disadvantages
- Slightly longer execution time per operation than SEAL-Python
- Requires a paid license for commercial purposes
- Lack of documentation: There are examples, but only an auto-generated class reference.

### Pyfhel
Pyfhel is a library that uses Microsoft SEAL in the background and provides easy access to homomorphic encryption functionality. It uses a syntax similar to normal arithmetic such as \*, +, -, >>, \*\*. It supports Integer FHE with BFV and Fixed-point FHE with CKKS. Thus it is an easy library to get started quickly, however as the numbers below show, it is generally slower than HElayers and SEAL. The docker file contains only one example how to use Pyfhel with Integer FHE via BFV, however there are more extensive tutorials available online[^6].

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
8192 slots have been used for Pyfhel and SEAL-Python.

For HElayers, only 1024 slots have been used, since I was not able to find a value for the plaintext prime modulus in a way that achieves 8192 slots. Instead, the Cyclotomic polynomial had to be increased from 128 to 2048. Secondly, the plaintext prime modulus had to be increased from 127 to 4079617. Without, overflow happened and resulted in negative results with the multiplication of two positive integers. Also, I had to take out the Subtraction operation, since negative values always resulted in a negative overflow, indicating that the slots only store unsigned integers. One way to solve this would be to manually interpret the results as signed integers, however time ran out in the end.

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

|                       | HElayers  | Pyfhel    | SEAL-Python     |
|-----------------------|-----------|-----------|-----------------|
| Addition              | 0.574405  | 0.118562  | 0.143598        |
| Subtraction           | 0.590532  | 0.151160  | 0.136508        |
| Multiplication        | 143.159352| 20.858954 | 24.720832       |

##### Single Ciphertext:

|                       | HElayers  | Pyfhel    | SEAL-Python     |
|-----------------------|-----------|-----------|-----------------|
| Squaring              | 111.080257| 16.266907 | 15.564545       |
| Negation              | 0.462940  | 16.037625 | 18.062968       |
| Left Rotation         | 225.597397| nya       | nya             |
| Right Rotation        | 149.627825| nya       | nya             |

##### Ciphertext-Plaintext:

|                       | HElayers  | Pyfhel    | SEAL-Python     |
|-----------------------|-----------|-----------|-----------------|
| Addition              | 27.632771 | 0.366492  | 0.327240        |
| Subtraction           | 38.966958 | 0.379061  | 0.351506        |
| Multiplication        | 24.003798 | 0.219237  | 3.280312        |

#### Test 2
In Test 2, all slots are filled with the maximum integer 100.

##### Ciphertext-Ciphertext:

|                       | HElayers  | Pyfhel    | SEAL-Python     |
|-----------------------|-----------|-----------|-----------------|
| Addition              | 1.141045  | 0.119359  | 0.131236        |
| Subtraction           | 1.146679  | 0.122014  | 0.146674        |
| Multiplication        | 258.167802| 19.078499 | 5.046285        |

##### Single Ciphertext:

|                       | HElayers  | Pyfhel    | SEAL-Python     |
|-----------------------|-----------|-----------|-----------------|
| Squaring              | 196.036979| 10.843201 | 4.119792        |
| Negation              | 0.931942  | 15.794727 | 0.131858        |
| Left Rotation         | 436.420230| 6.025279  | nya             |
| Right Rotation        | 285.941758| 4.975399  | nya             |

##### Ciphertext-Plaintext:

|                       | HElayers  | Pyfhel    | SEAL-Python     |
|-----------------------|-----------|-----------|-----------------|
| Addition              | 48.082252 | 0.399861  | 0.147074        |
| Subtraction           | 71.116051 | 0.357513  | 0.150686        |
| Multiplication        | 44.642202 | 0.209374  | 0.217033        |

### Data Conclusion

Overall tendence of the data seems to suggest, that SEAL-Python performs the best of the three libraries. Although Pyfhel is the easiest to use, it comes with a heavy increase in computation time per operation. HElayers is in the midfield.

## Conclusion

Although there are some guides and examples out there, I cannot recommend Homomorphic Encryption to computer scientists without at least getting a foundation in the mathematical theories behind. The parameter tuning has been done by trial-and-error, but they critically determine the performance of the calculations. In hindsight, I am looking forward to dive into the world of Homomorphic Encryptions again, however next time, with some more theoretical education.


[^1]: https://github.com/IBM/helayers
[^2]: https://github.com/ibarrond/Pyfhel
[^3]: https://github.com/Huelse/SEAL-Python
[^4]: https://developer.ibm.com/blogs/secure-ai-workloads-using-fully-homomorphic-encrypted-data/
[^5]: https://ibm.github.io/fhe-toolkit-linux/html/ml-helib/classhelayers_1_1_c_tile.html#a58f4ca21f819eaf447eb8698d1c446ec
[^6]: https://pyfhel.readthedocs.io/en/latest/_autoexamples/index.html
