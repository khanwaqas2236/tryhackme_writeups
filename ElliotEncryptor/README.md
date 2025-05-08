# Elliot-Style File Encryption Scripts for Kali Linux

Welcome to my GitHub repository for two powerful, Elliot-style file encryption approaches. These scripts provide high-security file encryption with a unique twist inspired by Mr. Robot. Choose the approach that fits your style:

## **Approach 1: Manual Decryption (Elliot-Style)**

In this approach, the encryption process is handled by a script, but decryption is done manually using commands. This mirrors the way Elliot might handle sensitive files, emphasizing stealth and manual control.

### **Encryption Script**

```bash

#!/bin/bash

# Welcome message to set the mood
clear
echo -e "\e[31mElliot's Encryptor - Encrypt like a Ghost in the Wires\e[0m"
sleep 1
echo "Initializing secure file encryption..."
sleep 1

# Prompt for file to encrypt
read -p "Enter the path to the file you want to encrypt: " FILE_PATH
if [ ! -f "$FILE_PATH" ]; then
    echo -e "\e[31mError: File not found!\e[0m"
    exit 1
fi

# Prompt for encryption key
read -s -p "Enter your encryption key (it will not be displayed): " ENC_KEY
echo

# Prompt for image to hide the key
read -p "Enter the path to the image where you want to store the encryption key: " IMAGE_PATH
if [ ! -f "$IMAGE_PATH" ]; then
    echo -e "\e[31mError: Image file not found!\e[0m"
    exit 1
fi

# Encrypt the file
ENCRYPTED_FILE="${FILE_PATH}.enc"
openssl enc -aes-256-cbc -salt -in "$FILE_PATH" -out "$ENCRYPTED_FILE" -k "$ENC_KEY" -pbkdf2

# Store the encryption key in the image
echo "$ENC_KEY" | steghide embed -cf "$IMAGE_PATH" -ef - -p ""

# Securely delete the original file
shred -u "$FILE_PATH"

# Clean up and confirmation
unset ENC_KEY
clear
echo -e "\e[32mFile encrypted successfully and key stored securely!\e[0m"
echo -e "Encrypted file: \e[34m$ENCRYPTED_FILE\e[0m"
echo -e "Key stored in: \e[34m$IMAGE_PATH\e[0m"
exit 0



```

### **Decryption Process** (Manual Commands)

Once your file is encrypted, use the following commands to decrypt it:

**1. Extract the key from the image:**

```bash
steghide extract -sf your_image.jpg -p "" -xf extracted_key.txt
```

**2. Decrypt the file:**

```bash
ENC_KEY=$(cat extracted_key.txt)
openssl enc -d -aes-256-cbc -salt -in your_encrypted_file.enc -out your_decrypted_file -k "$ENC_KEY" -pbkdf2
```

### **Advantages:**

* Greater control over decryption.
* Manual key extraction adds an extra layer of security.

### **Limitations:**

* **Vulnerable to Chosen Plaintext Attacks (CPA)** – Without a unique salt and IV for each file, attackers can use known plaintext to deduce the key.
* **Replay Attacks** – Using the same key repeatedly without additional entropy can lead to vulnerabilities.
* **No IV Handling** – Lacks the ability to handle Initialization Vectors, making it less secure against certain cryptographic attacks.

---

## **Approach 2: Full Encryption and Decryption Scripts**

This approach provides a more streamlined experience, with separate scripts for encryption and decryption, including metadata handling for added security.

### **Why This Approach is Stronger:**

* **Metadata Security:** It stores the salt and IV separately in a metadata file, ensuring that even if the encrypted file is intercepted, it cannot be decrypted without the metadata.
* **Improved Key Management:** By generating a random IV for each file, it prevents attackers from using known-plaintext attacks.
* **Efficient Decryption:** Automated decryption reduces human error, which is a major security risk.

### **Understanding Salt and IV:**

* **Salt:** A random value added before encryption to ensure the same plaintext produces different ciphertexts each time. It defends against rainbow table, dictionary, and precomputation attacks by forcing attackers to brute-force each hash individually, significantly increasing the time and resources required.
* **IV (Initialization Vector):** A unique, random value used for each encryption operation to prevent patterns in the ciphertext, ensuring semantic security. This is crucial for block ciphers like AES, as it prevents identical plaintext blocks from producing identical ciphertexts, which could reveal patterns to an attacker.
* **Protection Against Advanced Attacks:** Using salt and IV together protects against:

  * Known-Plaintext Attacks (KPA)
  * Chosen-Plaintext Attacks (CPA)
  * Chosen-Ciphertext Attacks (CCA)
  * Birthday Attacks
  * Replay Attacks

### **Encryption Script:**

```bash

#!/bin/bash

# Welcome message to set the mood
clear
echo -e "\e[31mElliot's Encryptor - Encrypt like a Ghost in the Wires\e[0m"
sleep 1
echo "Initializing secure file encryption..."
sleep 1

# Prompt for file to encrypt
read -p "Enter the path to the file you want to encrypt: " FILE_PATH
if [ ! -f "$FILE_PATH" ]; then
    echo -e "\e[31mError: File not found!\e[0m"
    exit 1
fi

# Prompt for encryption key
read -s -p "Enter your encryption key (it will not be displayed): " ENC_KEY
echo

# Generate Salt and IV for AES Encryption
SALT=$(openssl rand -hex 8)
IV=$(openssl rand -hex 16)

# Prompt for image to hide the key
read -p "Enter the path to the image where you want to store the encryption key: " IMAGE_PATH
if [ ! -f "$IMAGE_PATH" ]; then
    echo -e "\e[31mError: Image file not found!\e[0m"
    exit 1
fi

# Encrypt the file with AES-256-CBC
ENCRYPTED_FILE="${FILE_PATH}.enc"
openssl enc -aes-256-cbc -salt -in "$FILE_PATH" -out "$ENCRYPTED_FILE" -k "$ENC_KEY" -pbkdf2 -S "$SALT" -iv "$IV"

# Store the encryption key in the image
echo "$ENC_KEY" | steghide embed -cf "$IMAGE_PATH" -ef - -p ""

# Create the metadata file
METADATA_FILE="${ENCRYPTED_FILE}.metadata"
echo "Salt: $SALT" > "$METADATA_FILE"
echo "IV: $IV" >> "$METADATA_FILE"

# Securely delete the original file
shred -u "$FILE_PATH"

# Clean up and confirmation
unset ENC_KEY
clear
echo -e "\e[32mFile encrypted successfully and key stored securely!\e[0m"
echo -e "Encrypted file: \e[34m$ENCRYPTED_FILE\e[0m"
echo -e "Metadata file: \e[34m$METADATA_FILE\e[0m"
echo -e "Key stored in: \e[34m$IMAGE_PATH\e[0m"
exit 0
                                                                                                                                       



```

### **Encryption Script:**

```bash

#!/bin/bash

# Welcome message to set the mood
clear
echo -e "\e[31mElliot's Decryptor - Recover the Ghost in the Wires\e[0m"
sleep 1
echo "Initializing secure file decryption..."
sleep 1

# Prompt for encrypted file
read -p "Enter the path to the encrypted file: " ENC_FILE
if [ ! -f "$ENC_FILE" ]; then
    echo -e "\e[31mError: Encrypted file not found!\e[0m"
    exit 1
fi

# Prompt for metadata file
read -p "Enter the path to the metadata file: " METADATA_FILE
if [ ! -f "$METADATA_FILE" ]; then
    echo -e "\e[31mError: Metadata file not found!\e[0m"
    exit 1
fi

# Extract Salt and IV from the metadata file
SALT=$(grep "Salt" "$METADATA_FILE" | cut -d' ' -f2)
IV=$(grep "IV" "$METADATA_FILE" | cut -d' ' -f2)

if [ -z "$SALT" ] || [ -z "$IV" ]; then
    echo -e "\e[31mError: Salt or IV missing in metadata file!\e[0m"
    exit 1
fi

# Prompt for encryption key (password)
read -s -p "Enter your decryption key (it will not be displayed): " ENC_KEY
echo

# Decrypt the file using the salt, IV, and the encryption key
DECRYPTED_FILE="${ENC_FILE%.enc}"
openssl enc -d -aes-256-cbc -in "$ENC_FILE" -out "$DECRYPTED_FILE" -k "$ENC_KEY" -pbkdf2 -S "$SALT" -iv "$IV"

# Confirm decryption
if [ $? -eq 0 ]; then
    echo -e "\e[32mDecryption successful! Your file is decrypted and saved as: $DECRYPTED_FILE\e[0m"
else
    echo -e "\e[31mDecryption failed. Please check your key and metadata.\e[0m"
    exit 1
fi

exit 0

```
                                                                                                                                       




### **Advantages:**

* Automated decryption reduces the risk of human error.
* Metadata file provides additional security by including IV and Salt.
* Stronger defense against a wider range of attacks.

---

Feel free to clone, modify, and adapt these scripts based on your specific security needs. For questions, issues, or suggestions, drop them in the comments section!

Stay secure. Stay Elliot.
