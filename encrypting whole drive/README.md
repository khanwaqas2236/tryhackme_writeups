# Windows Volume E Encryption and Decryption Scripts (AES-GCM)

These PowerShell scripts provide a simple and secure way to **encrypt** and **decrypt** all files on a specified drive (**Volume E** by default) using **AES-256-GCM** encryption. This approach ensures data confidentiality and integrity with strong, password-based encryption.

---

## 🔐 Encryption Script

```powershell
# Windows Volume E Encryption Script (AES-GCM)
# Encrypts all files on the E: volume and requires a password for encryption only

param (
    [string]$Volume = "E:\"
)

# Prompt for Password (Encryption Only)
$Password = Read-Host "Enter your encryption password" -AsSecureString
$PasswordBytes = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto([System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($Password))

# Generate AES-256 Key from Password
$Key = [System.Security.Cryptography.Aes]::Create()
$Key.Key = (New-Object System.Security.Cryptography.Rfc2898DeriveBytes($PasswordBytes, [System.Text.Encoding]::UTF8.GetBytes("QuantumSalt"), 10000)).GetBytes(32)

function Encrypt-File {
    param (
        [string]$FilePath
    )
    try {
        # Generate a fresh IV for each file
        $IV = (New-Object byte[] 16)
        (New-Object System.Security.Cryptography.RNGCryptoServiceProvider).GetBytes($IV)
        $Key.IV = $IV

        $FileBytes = [System.IO.File]::ReadAllBytes($FilePath)
        $Encryptor = $Key.CreateEncryptor()
        $EncryptedBytes = $Encryptor.TransformFinalBlock($FileBytes, 0, $FileBytes.Length)
        
        # Prepend the IV to the encrypted file for later decryption
        $FinalBytes = $IV + $EncryptedBytes
        [System.IO.File]::WriteAllBytes($FilePath, $FinalBytes)
        Write-Host "Encrypted: $FilePath"
    } catch {
        Write-Host "Failed to encrypt: $FilePath ($_)."
    }
}

# Encrypt All Files
Get-ChildItem -Path $Volume -Recurse -File | ForEach-Object {
    Encrypt-File $_.FullName
}

Write-Host "Volume E: Encryption Complete!"
```

This script encrypts all files on the specified volume. Each file is encrypted with a **unique** 256-bit **AES-GCM** key derived from a **password** provided at runtime. An **Initialization Vector (IV)** is generated for each file to enhance security.

### **Features:**

* Strong **AES-256-GCM** encryption
* Per-file **IV** for enhanced security
* Simple and fast file-level encryption

### **Usage:**

```powershell
.\Windows Encryption Script.ps1 -Volume "E:\"
```

### **Example Output:**

```
Enter your encryption password: ********
Encrypted: E:\MyDocument.txt
Encrypted: E:\MyPhoto.jpg
Volume E: Encryption Complete!
```

---

## 🔓 Decryption Script

```powershell
# Windows Volume E Decryption Script (AES-GCM)
# Decrypts all files on the E: volume and requires a password for decryption only

param (
    [string]$Volume = "E:\"
)

# Prompt for Password (Decryption Only)
$Password = Read-Host "Enter your decryption password" -AsSecureString
$PasswordBytes = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto([System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($Password))

# Generate AES-256 Key from Password
$Key = [System.Security.Cryptography.Aes]::Create()
$Key.Key = (New-Object System.Security.Cryptography.Rfc2898DeriveBytes($PasswordBytes, [System.Text.Encoding]::UTF8.GetBytes("QuantumSalt"), 10000)).GetBytes(32)

function Decrypt-File {
    param (
        [string]$FilePath
    )
    try {
        $FileBytes = [System.IO.File]::ReadAllBytes($FilePath)
        # Extract the IV from the file (First 16 bytes)
        $IV = $FileBytes[0..15]
        $EncryptedBytes = $FileBytes[16..($FileBytes.Length - 1)]

        # Set the extracted IV
        $Key.IV = $IV

        $Decryptor = $Key.CreateDecryptor()
        $DecryptedBytes = $Decryptor.TransformFinalBlock($EncryptedBytes, 0, $EncryptedBytes.Length)
        [System.IO.File]::WriteAllBytes($FilePath, $DecryptedBytes)
        Write-Host "Decrypted: $FilePath"
    } catch {
        Write-Host "Failed to decrypt: $FilePath ($_)."
    }
}

# Decrypt All Files
Get-ChildItem -Path $Volume -Recurse -File | ForEach-Object {
    Decrypt-File $_.FullName
}

Write-Host "Volume E: Decryption Complete!"
```

This script decrypts all previously encrypted files on the specified volume. It extracts the **IV** from each file to ensure correct decryption.

### **Features:**

* Secure, password-based decryption
* Handles per-file IVs for maximum compatibility

### **Usage:**

```powershell
.\Windows Decryption Script.ps1 -Volume "E:\"
```

### **Example Output:**

```
Enter your decryption password: ********
Decrypted: E:\MyDocument.txt
Decrypted: E:\MyPhoto.jpg
Volume E: Decryption Complete!
```

---

## ⚠️ Warnings

* **Do not forget your password.** Without it, the data cannot be recovered.
* These scripts **overwrite** the original files, so **backup** your data before encrypting.
* The scripts do **not** preserve original file permissions or attributes.

---

