---
title: 'Experimenting Encryption and Decryption with Python: Understanding the Transposition Cipher'
date: 2024-03-18T00:07:32+01:00
disableshare: false
category:
cover:
  image:
  alt:
  caption:
  relative: false
draft: false
---

In the world of data security, encryption is like a fortress protecting sensitive information from prying eyes. Among the many encryption methods, there's one called the Transposition Cipher. It's a bit like rearranging the letters of a word to make it look scrambled. In this blog post, we'll take a closer look at how this encryption method works using a Python script, you can find full project [here](https://github.com/heyitsazar/ascii_encryption_basics).

## Understanding the Code

Let's break down the code step by step:

```python
# Asking for the message and encryption key from the user
original_message = input("Enter the original message: ")
key = input("Enter the encryption key: ")

# Function to check if the key is valid
def checkKey():
    # A nested function to check for duplicate digits in the key
    def checkDuplicate():
        for i in range(0, len(key)): 
            for j in range(i+1, len(key)):
                if(key[i] == key[j]):
                    return True;
    # Conditions to validate the key
    if len(key) != 8:
        print("The encryption key must be 8 characters long")
        return False
    elif checkDuplicate():
        print("The encryption key contains duplicate digits")
        return False
    elif '0' in str(key):
        print("The encryption key cannot contain the digit 0")
    else:
        for digit in key:
            if (int(digit) > 8):
                print("The encryption key contains digits greater than 8")
                return False
    return True

# Function to convert a decimal number to binary
def decimalToBinary(digit):
    return bin(digit).replace("b", "")
```

The code starts by asking the user for the message they want to encrypt and the key they want to use for encryption. Then, it defines a function called `checkKey()` to make sure the key is valid. This function checks the length of the key, looks for duplicate digits, and ensures that no digit is greater than 8. Additionally, there's a function `decimalToBinary()` which converts decimal numbers to binary format.

## What is the Transposition Cipher?

The Transposition Cipher works by shuffling the order of characters in the message based on a specific key. Let's illustrate this with an example:

Suppose we have a message "HELLO" and a key "2143". Following the Transposition Cipher method, we rearrange the characters of the message based on the key:

- 1st position in the key: H (1st character of the message)
- 2nd position in the key: E (2nd character of the message)
- 3rd position in the key: L (3rd character of the message)
- 4th position in the key: L (4th character of the message)
- 5th position in the key: O (5th character of the message)

| Key Position | Character in Message | Encrypted Character |
| ------------ | -------------------- | ------------------- |
| 1            | H                    | H                   |
| 2            | E                    | E                   |
| 3            | L                    | L                   |
| 4            | L                    | O                   |
| 5            | O                    | L                   |

Putting these characters together based on the key, we get the encrypted message "HELOL".

Stay tuned for the next part, where we'll dive deeper into the encryption process and see how the Transposition Cipher works in action.

---

**Disclaimer:** This project is for educational purposes only. It's not suitable for encrypting sensitive information, and there may be bugs in the provided script.