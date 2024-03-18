---
title: 'Fernet Encryption and Password Management in Python'
date: 2024-03-18T03:17:04+01:00
disableshare: false
category:
cover:
  image:
  alt:
  caption:
  relative: false
draft: false
---

In this project, I tried to build basic password manager with Fernet Encryption.

Find project on GitHub, by clicking [here](https://github.com/heyitsazar/basic_password_manager).

## Understanding Fernet Encryption

[Fernet](https://cryptography.io/en/latest/fernet/) is a symmetric encryption algorithm provided by the `cryptography` library in Python. It ensures that a message encrypted with it cannot be manipulated or read by unauthorized users. Fernet encryption requires a key, which is used to both encrypt and decrypt the data. This key should be kept secure, as anyone with access to it can decrypt the encrypted data.

Fernet encryption operates in a symmetric key system (*I will post a post about symmetric/asymmetric key systems*), meaning the same key is used for both encryption and decryption. This simplicity makes it easy to implement while providing strong security for sensitive information.

## How the Code Works

Now, let's dive into a Python script.

### Generating and Loading the Master Key

```python
from cryptography.fernet import Fernet

def generate_key():
    key = Fernet.generate_key()
    key_file = open("key.key","wb")
    key_file.write(key)
    key_file.close()
    return key

def load_key():
    key_file = open("key.key","rb")
    key = key_file.read()
    key_file.close()
    return key
```

These functions handle the generation and loading of the master key required for encryption and decryption. 
- The `generate_key()` function generates a new key using Fernet's `generate_key()` method and stores it in a file named `key.key`.
- The `load_key()` function loads the key stored in the `key.key` file.

### Initializing the Password Manager

```python
print("Welcome to password manager!")
generate_pwd_prompt = input("If this is your first run, we need to generate a master key. Do you want to generate it? (y/n): ").lower() == "y"
main_pwd = ""

if generate_pwd_prompt:
    key = generate_key()
    if key:
        main_pwd = key
    else:
        print("Error: Unable to generate master key.")
        exit()  
else:
    key = load_key()
    if key:
        main_pwd = key
    else:
        print("Error: Unable to load master key.")
        exit()  
```

This section initializes the password manager by prompting the user to either generate a new master key or load an existing one. 
- The user's response is stored in `generate_pwd_prompt`.
- If the user chooses to generate a new key, `generate_key()` function is called and the key is stored in `main_pwd`.
- If the user chooses to load an existing key, `load_key()` function is called to load the key.

### Initializing Fernet with the Master Key

```python
fer = Fernet(key)
```

The Fernet object `fer` is initialized with the master key obtained in the previous step. This object is used for encryption and decryption operations.

### Adding and Viewing Passwords

```python
def add():
    vault_file = open("vault.txt","a")
    account_site = input("Enter site name (Facebook, Google): ")
    account_name = input("Enter account name (Username/E-mail): ")
    account_password = input("Enter password: ")
    vault_file.write(account_site + "|" + fer.encrypt(account_name.encode()).decode() + "|" + fer.encrypt(account_password.encode()).decode())
    vault_file.close()
    print("Password added!" + "\n")

def view():
    vault_file = open("vault.txt","r")
    for line in vault_file.readlines():
        data = line.rstrip()
        site_name, user, passw = data.split("|")
        print("Site name: ", site_name, "| Username: ",  fer.decrypt(user.encode()).decode(), "| Password: ", fer.decrypt(passw.encode()).decode())
    print("")
```

These functions allow users to add and view passwords.
- The `add()` function prompts the user to enter details for a new password (site name, account name, and password). The provided information is encrypted using Fernet encryption and stored in the `vault.txt` file.
- The `view()` function reads the contents of the `vault.txt` file, decrypts the stored passwords using the Fernet object `fer`, and prints the decrypted information (site name, username, and password).

### Main Loop for Password Manager Operations

```python
while True:
    print("This is your main password vault (Find it on key.key file) (DO NOT LOSE IT!!!): " + main_pwd.decode())
    operation =  input("Select the operation (add, view): ").lower()

    if operation == "add":
        add()
    elif operation == "view":
        view()
    else:
        print("Wrong operation name")
```

This loop continuously prompts the user to select an operation (add or view) for the password manager. Based on the user's input, it calls the corresponding function (`add()` or `view()`). If an invalid operation is entered, an error message is displayed.

This Python script provides a basic implementation of a password manager using Fernet encryption, allowing users to securely store and retrieve passwords for different accounts.

### Note
This project serves as an educational resource to understand basic password management techniques using encryption in Python. It is not intended for use in production environments and should not be used to manage sensitive data.