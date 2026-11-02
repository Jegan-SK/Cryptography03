# HILL CIPHER

## EX. NO: 3 

## AIM:
 
## To write a C program to implement the hill cipher substitution techniques.

## DESCRIPTION:

Each letter is represented by a number modulo 26. Often the simple scheme A = 0, B
= 1... Z = 25, is used, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters is  multiplied by an invertible n × n matrix, against modulus 26. To
decrypt the message, each block is multiplied by the inverse of the m trix used for
 
encryption. The matrix used
 
for encryption is the cipher key, and it sho
 
ld be chosen
 
randomly from the set of invertible n × n matrices (modulo 26).


## ALGORITHM:

STEP-1: Read the plain text and key from the user. 
STEP-2: Split the plain text into groups of length three. 
STEP-3: Arrange the keyword in a 3*3 matrix.
STEP-4: Multiply the two matrices to obtain the cipher text of length three.
STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM 

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Function to calculate GCD
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// Function to calculate determinant of 3x3 matrix
int determinant(int key[3][3]) {
    return key[0][0] * (key[1][1] * key[2][2] - key[1][2] * key[2][1])
         - key[0][1] * (key[1][0] * key[2][2] - key[1][2] * key[2][0])
         + key[0][2] * (key[1][0] * key[2][1] - key[1][1] * key[2][0]);
}

// Hill Cipher Encryption
void hillEncrypt(char text[], int key[3][3]) {
    int len = strlen(text);

    // Pad plaintext with X until length is divisible by 3
    while (len % 3 != 0) {
        text[len++] = 'X';
    }
    text[len] = '\0';

    printf("\nCiphertext: ");

    // Process plaintext in blocks of 3
    for (int i = 0; i < len; i += 3) {
        int block[3];

        // Convert characters to numbers
        for (int j = 0; j < 3; j++) {
            block[j] = text[i + j] - 'A';
        }

        // Matrix multiplication
        for (int row = 0; row < 3; row++) {
            int sum = 0;

            for (int col = 0; col < 3; col++) {
                sum += key[row][col] * block[col];
            }

            printf("%c", (sum % 26) + 'A');
        }
    }

    printf("\n");
}

int main() {
    char plaintext[510];
    char keyText[100];
    int key[3][3];

    printf("Enter Plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = '\0';

    printf("Enter 9-letter Encryption Key: ");
    fgets(keyText, sizeof(keyText), stdin);
    keyText[strcspn(keyText, "\n")] = '\0';

    // Validate key length
    if (strlen(keyText) != 9) {
        printf("Error: Key must contain exactly 9 letters.\n");
        return 1;
    }

    // Convert key into uppercase and validate
    for (int i = 0; i < 9; i++) {
        if (!isalpha((unsigned char)keyText[i])) {
            printf("Error: Key must contain alphabets only.\n");
            return 1;
        }

        keyText[i] = toupper((unsigned char)keyText[i]);
    }

    // Generate 3x3 key matrix
    int k = 0;

    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            key[i][j] = keyText[k++] - 'A';
        }
    }

    // Validate plaintext and convert to uppercase
    int n = 0;

    for (int i = 0; plaintext[i] != '\0'; i++) {
        if (isalpha((unsigned char)plaintext[i])) {
            plaintext[n++] = toupper((unsigned char)plaintext[i]);
        }
        else if (plaintext[i] != ' ') {
            printf("Error: Plaintext must contain only letters and spaces.\n");
            return 1;
        }
    }

    plaintext[n] = '\0';

    // Check whether key matrix is invertible modulo 26
    int det = determinant(key);

    if (gcd((det % 26 + 26) % 26, 26) != 1) {
        printf("Error: Invalid key matrix. Choose an invertible matrix.\n");
        return 1;
    }

    printf("\nOriginal Plaintext: %s\n", plaintext);

    // Display key matrix
    printf("\nKey Matrix:\n");

    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%3d", key[i][j]);
        }
        printf("\n");
    }

    // Encrypt
    hillEncrypt(plaintext, key);

    return 0;
}
```

## OUTPUT

<img width="1085" height="408" alt="image" src="https://github.com/user-attachments/assets/cdd08108-4399-4b07-9a08-cdfcee9bbc9f" />


## RESULT

Thus the implementation of Hill Cipher Encryption and decryption using C is ran and executed successfully.

