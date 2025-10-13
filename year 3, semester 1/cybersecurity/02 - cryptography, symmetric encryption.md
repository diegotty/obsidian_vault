---
related to: "[[01 - introduction to computer security]]"
created: 2025-03-02T17:41
updated: 2025-10-13T09:32
completed: false
---
# symmetric encryption
symmetric encryption is the universal technique for providing confidentiality for transmitted or stored data. it is also referred as *conventional encryption* or *single-key encryption*. two requirements need to be met for secure use:
- a strong encryption algorithm
- sender and receiver must have obtained copies of the secret key in a secure fashion, and must keep the key *secure*
>[!info] simplified model of symmetric encryption
![[Pasted image 20251013091327.png]]

## symmetric encryption attacks
2 types of attacks are used:
- **cryptanalitic attacks**: these attacks rely on the nature of the algorithm,  the knowledge of the general characteristics of the plaintext, or some sample plaintext-ciphertext pairs. this knowledge is then used to attempt to deduce the key used to encrypt plaintexts
- **brute-force attacks**: try all possible keys on some ciphertext, until an intelligible translation into plaintext is obtained
	- on average, half of all possible keys must be tried to achieve success
## types of ciphers
- **block ciphers**: they process the input of one block of elements at a time. they produce an output block for each input block. they are more common and can reuse keys
- **stream ciphers**: they process the input elements continuously, producing output one element at a time. they are faster and use far less code than block ciphers, and encrypt plaintext one byte at a time.
## symmetric encryption algorithms
the most known are:
- **AES** (advanced encryption standard / rijndael): 
- **DES** (data encryption standard): now considered inscure, 
  **RC4** (ARC4): now also considered insecure
  >[!info] brute forcing modern block ciphers
  ![[Pasted image 20251013092911.png]]

