# Ex-2 PLAYFAIR CIPHER

<br>

## DATE:

<br>

## AIM:

<br>

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

<br>

## ALGORITHM:

<br>

Step 1: Design of PlayFair Cipher algorithnm

<br>

Step 2: Implementation using C or pyhton code

<br>

Step 3: Testing algorithm with different key values.

<br>

## PROGRAM:

<br>

```
#include <iostream>
#include <cstring>
#include <cctype>

#define SIZE 30

using namespace std;

// Function to convert the string to lowercase
void toLowerCase(char str[]) {
    for (int i = 0; str[i]; i++) {
        str[i] = tolower(str[i]);
    }
}

// Function to remove all spaces in a string
int removeSpaces(char* str) {
    int count = 0;
    for (int i = 0; str[i]; i++) {
        if (str[i] != ' ') {
            str[count++] = str[i];
        }
    }
    str[count] = '\0'; // Null-terminate the string
    return count;
}

// Function to generate the 5x5 key square
void generateKeyTable(char key[], char keyTable[5][5]) {
    bool charSet[26] = {false}; // To track characters used in the key
    int index = 0;

    // Insert key characters into keyTable
    for (int i = 0; key[i]; i++) {
        if (key[i] == 'j') key[i] = 'i'; // Treat 'j' as 'i'
        if (!charSet[key[i] - 'a']) { // Check if character is already used
            charSet[key[i] - 'a'] = true;
            keyTable[index / 5][index % 5] = key[i];
            index++;
        }
    }

    // Fill the rest of the table with remaining characters
    for (char ch = 'a'; ch <= 'z'; ch++) {
        if (ch == 'j') continue; // Skip 'j'
        if (!charSet[ch - 'a']) {
            keyTable[index / 5][index % 5] = ch;
            index++;
        }
    }
}

// Function to search for the characters of a digraph in the key square and return their position
void search(char keyTable[5][5], char a, char b, int arr[]) {
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (keyTable[i][j] == a) {
                arr[0] = i; // Row of first character
                arr[1] = j; // Column of first character
            }
            if (keyTable[i][j] == b) {
                arr[2] = i; // Row of second character
                arr[3] = j; // Column of second character
            }
        }
    }
}

// Function to prepare the plaintext for encryption
int prepare(char str[]) {
    int len = strlen(str);
    if (len % 2 != 0) { // If the length is odd, append 'x'
        str[len++] = 'x';
    }
    str[len] = '\0'; // Null-terminate the string
    return len;
}

// Function for performing the encryption
void encrypt(char str[], char keyTable[5][5]) {
    int len = strlen(str);
    for (int i = 0; i < len; i += 2) {
        int arr[4];
        search(keyTable, str[i], str[i + 1], arr);
        if (arr[0] == arr[2]) { // Same row
            str[i] = keyTable[arr[0]][(arr[1] + 1) % 5];
            str[i + 1] = keyTable[arr[2]][(arr[3] + 1) % 5];
        } else if (arr[1] == arr[3]) { // Same column
            str[i] = keyTable[(arr[0] + 1) % 5][arr[1]];
            str[i + 1] = keyTable[(arr[2] + 1) % 5][arr[3]];
        } else { // Rectangle swap
            str[i] = keyTable[arr[0]][arr[3]];
            str[i + 1] = keyTable[arr[2]][arr[1]];
        }
    }
}

// Function to encrypt using Playfair Cipher
void encryptByPlayfairCipher(char str[], char key[]) {
    char keyTable[5][5];
    toLowerCase(key);
    int ks = removeSpaces(key); // Remove spaces and get length
    generateKeyTable(key, keyTable);
    int ps = removeSpaces(str); // Prepare plaintext
    ps = prepare(str); // Make length even
    encrypt(str, keyTable); // Encrypt
}

int main() {
    char str[SIZE], key[SIZE];

    // Key to be encrypted
    strcpy(key, "Monarchy");
    cout << "Key text: " << key << endl;

    // Plaintext to be encrypted
    strcpy(str, "instruments");
    cout << "Plain text: " << str << endl;

    // Encrypt using Playfair Cipher
    encryptByPlayfairCipher(str, key);
    cout << "Cipher text: " << str << endl;

    return 0;
}
```

<br>

## OUTPUT:

<br>

![image](https://github.com/user-attachments/assets/0a99c975-4b53-4503-aeed-60bd913cf600)

<br>

## RESULT:

<br>

The program is executed successfully
