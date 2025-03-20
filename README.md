## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

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
import re
SIZE = 5
def generate_key_matrix(key):
    key = key.upper().replace('J', 'I')
    seen = set()
    matrix = []
    for char in key + "ABCDEFGHIKLMNOPQRSTUVWXYZ":
        if char not in seen and char.isalpha():
            seen.add(char)
            matrix.append(char)
    return [matrix[i * SIZE:(i + 1) * SIZE] for i in range(SIZE)]
def find_position(matrix, char):
    for row in range(SIZE):
        for col in range(SIZE):
            if matrix[row][col] == char:
                return row, col
    return None, None
def process_digraphs(text):
    text = re.sub(r'[^A-Z]', '', text.upper().replace('J', 'I'))
    digraphs = []
    i = 0
    while i < len(text):
        a = text[i]
        b = text[i + 1] if i + 1 < len(text) else 'X'
        
        if a == b:
            digraphs.append(a + 'X')
            i += 1
        else:
            digraphs.append(a + b)
            i += 2
    
    return digraphs
def encrypt_decrypt(text, matrix, encrypt=True):
    processed_text = process_digraphs(text)
    result = []
    
    for a, b in processed_text:
        row1, col1 = find_position(matrix, a)
        row2, col2 = find_position(matrix, b)
        
        if row1 == row2:  # Same row
            col1 = (col1 + 1) % SIZE if encrypt else (col1 - 1) % SIZE
            col2 = (col2 + 1) % SIZE if encrypt else (col2 - 1) % SIZE
        elif col1 == col2:  # Same column
            row1 = (row1 + 1) % SIZE if encrypt else (row1 - 1) % SIZE
            row2 = (row2 + 1) % SIZE if encrypt else (row2 - 1) % SIZE
        else:  # Rectangle swap
            col1, col2 = col2, col1
        
        result.append(matrix[row1][col1] + matrix[row2][col2])
    
    return ''.join(result)

def playfair_cipher():
    key = input("Enter the key: ").strip()
    text = input("Enter text to encrypt: ").strip()
    
    matrix = generate_key_matrix(key)
    
    print("\nKey Matrix:")
    for row in matrix:
        print(' '.join(row))
    
    encrypted_text = encrypt_decrypt(text, matrix, encrypt=True)
    print(f"\nEncrypted Text: {encrypted_text}")
    
    decrypted_text = encrypt_decrypt(encrypted_text, matrix, encrypt=False)
    print(f"Decrypted Text: {decrypted_text}")

if __name__ == "__main__":
    playfair_cipher()

```


Output:

![Screenshot 2025-03-20 041108](https://github.com/user-attachments/assets/b012a0b2-4343-4e46-8c84-d8acc9af02d1)

Result:

Thus the implemetation of Playfair cyfer has been executed successfully..
