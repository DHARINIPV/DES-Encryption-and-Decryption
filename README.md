# Ex 7 DES-Encryption-and-Decryption
## Aim:
Implementation of Pseudorandom Number Generation Using Standard library (DES)
## Program:
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <time.h>

// Function to calculate GCD
long gcd(long a, long b) {
    while (b != 0) {
        long t = b;
        b = a % b;
        a = t;
    }
    return a;
}

// Function to perform modular exponentiation
long mod_exp(long base, long exp, long mod) {
    long result = 1;
    base = base % mod;
    while (exp > 0) {
        if (exp % 2 == 1) {
            result = (result * base) % mod;
        }
        exp = exp >> 1;
        base = (base * base) % mod;
    }
    return result;
}

// Function to generate RSA keys
void generate_keys(long *e, long *d, long *n) {
    long p = 61; // Example prime number
    long q = 53; // Example prime number
    *n = p * q;
    long phi = (p - 1) * (q - 1);

    *e = 3;
    while (gcd(*e, phi) != 1) {
        (*e)++;
    }

    *d = 1;
    while ((*d * (*e)) % phi != 1) {
        (*d)++;
    }
}

// Function to encrypt a message
long encrypt(long message, long e, long n) {
    return mod_exp(message, e, n);
}

// Function to decrypt a message
long decrypt(long encrypted, long d, long n) {
    return mod_exp(encrypted, d, n);
}

// Function to generate round keys for DES
void generate_round_keys(uint64_t key, uint64_t round_keys[16]) {
    for (int i = 0; i < 16; ++i) {
        round_keys[i] = key ^ (i * 0x0101010101010101ULL);
    }
}

// DES encryption function
uint64_t des_encrypt(uint64_t plaintext, uint64_t round_keys[16]) {
    uint64_t ciphertext = plaintext;
    for (int i = 0; i < 16; ++i) {
        ciphertext = ciphertext ^ round_keys[i];
    }
    return ciphertext;
}

// DES decryption function
uint64_t des_decrypt(uint64_t ciphertext, uint64_t round_keys[16]) {
    uint64_t plaintext = ciphertext;
    for (int i = 15; i >= 0; --i) {
        plaintext = plaintext ^ round_keys[i];
    }
    return plaintext;
}

// Generate a pseudorandom key using standard library
uint64_t generate_random_key() {
    uint64_t key = 0;
    for (int i = 0; i < 8; ++i) {
        key = (key << 8) | (rand() % 256);
    }
    return key;
}

int main() {
    srand(time(NULL)); // Seed the random number generator

    long e, d, n;
    generate_keys(&e, &d, &n);
    
    printf("Public Key: (%ld, %ld)\n", e, n);
    printf("Private Key: (%ld, %ld)\n", d, n);
    
    long message = 65; // Example message to encrypt
    long encrypted = encrypt(message, e, n);
    printf("Encrypted Message: %ld\n", encrypted);
    
    long decrypted = decrypt(encrypted, d, n);
    printf("Decrypted Message: %ld\n", decrypted);

    uint64_t random_key = generate_random_key();
    uint64_t round_keys[16];
    generate_round_keys(random_key, round_keys);

    uint64_t des_plaintext = 0x0123456789ABCDEF; // Example plaintext for DES

    uint64_t des_encrypted = des_encrypt(des_plaintext, round_keys);
    printf("DES Encrypted: %016llX\n", des_encrypted);

    uint64_t des_decrypted = des_decrypt(des_encrypted, round_keys);
    printf("DES Decrypted: %016llX\n", des_decrypted);

    return 0;
}
```
## Output:
![image](https://github.com/user-attachments/assets/2acd51e1-5e62-4861-9558-e0d4a62c11a0)

## Result:
The program is executed successfully.
