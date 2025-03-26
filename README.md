## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
## NAME : KISHORE K
## REG N0 : 212223040101
 

## AIM:
 
To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

char keyTable[SIZE][SIZE];
int exists[26] = {0};

void toUpperCase(char *str) {
    for (int i = 0; str[i]; i++) {
        str[i] = toupper(str[i]);
    }
}

void removeSpaces(char *str) {
    int count = 0;
    for (int i = 0; str[i]; i++) {
        if (str[i] != ' ') {
            str[count++] = str[i];
        }
    }
    str[count] = '\0';
}

void generateKeyTable(char *key) {
    memset(exists, 0, sizeof(exists));
    int k = 0;
    toUpperCase(key);
    removeSpaces(key);
    
    for (int i = 0; key[i]; i++) {
        if (key[i] == 'J') key[i] = 'I';
        if (!exists[key[i] - 'A']) {
            keyTable[k / SIZE][k % SIZE] = key[i];
            exists[key[i] - 'A'] = 1;
            k++;
        }
    }
    
    for (char ch = 'A'; ch <= 'Z'; ch++) {
        if (ch == 'J') continue;
        if (!exists[ch - 'A']) {
            keyTable[k / SIZE][k % SIZE] = ch;
            exists[ch - 'A'] = 1;
            k++;
        }
    }
}

void findPosition(char ch, int *row, int *col) {
    if (ch == 'J') ch = 'I';
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (keyTable[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void encrypt(char *text, char *cipher) {
    toUpperCase(text);
    removeSpaces(text);
    
    int len = strlen(text);
    for (int i = 0; i < len; i += 2) {
        if (i + 1 == len || text[i] == text[i + 1]) {
            for (int j = len; j > i + 1; j--) {
                text[j] = text[j - 1];
            }
            text[i + 1] = 'X';
            len++;
        }
    }
    
    text[len] = '\0';
    int row1, col1, row2, col2;
    for (int i = 0; i < len; i += 2) {
        findPosition(text[i], &row1, &col1);
        findPosition(text[i + 1], &row2, &col2);
        
        if (row1 == row2) {
            cipher[i] = keyTable[row1][(col1 + 1) % SIZE];
            cipher[i + 1] = keyTable[row2][(col2 + 1) % SIZE];
        } else if (col1 == col2) {
            cipher[i] = keyTable[(row1 + 1) % SIZE][col1];
            cipher[i + 1] = keyTable[(row2 + 1) % SIZE][col2];
        } else {
            cipher[i] = keyTable[row1][col2];
            cipher[i + 1] = keyTable[row2][col1];
        }
    }
    cipher[len] = '\0';
}

void printKeyTable() {
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf("%c ", keyTable[i][j]);
        }
        printf("\n");
    }
}

int main() {
    char key[100], text[100], cipher[100];
    
    printf("Enter key: ");
    gets(key);
    generateKeyTable(key);
    
    printf("\nPlayfair Cipher Key Table:\n");
    printKeyTable();
    
    printf("\nEnter plaintext: ");
    gets(text);
    
    encrypt(text, cipher);
    
    printf("\nCiphertext: %s\n", cipher);
    return 0;
}
```



Output:
