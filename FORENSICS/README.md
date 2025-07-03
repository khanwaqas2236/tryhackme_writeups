## Chrome History Backup & Retention Script

## Automatically backs up Chrome browsing history (even if deleted) and retains only the last 7 days of data.

## 📝 Description

This script continuously monitors Chrome's History file (SQLite database) and creates timestamped backups. It automatically:

✅ Backs up history every 60 seconds

✅ Keeps only the last 7 days of backups (deletes older files)

✅ Runs silently in the background (via Scheduled Task)

## Useful for:

Forensics (recovering deleted Chrome history)

Parental monitoring (track browsing even if cleared)

Personal backup (accidental history deletion)

⚙️ Setup Instructions

1️⃣ Download the Script

Choose either the Batch or PowerShell version:


## 📜 Batch Version (ChromeHistoryBackup.bat)


```

@echo off
setlocal enabledelayedexpansion

# Configure paths
set "chrome_history=%LOCALAPPDATA%\Google\Chrome\User Data\Default\History"
set "backup_folder=C:\ChromeHistoryBackup"
set "days_to_keep=7"

# Create backup folder if missing
if not exist "%backup_folder%" mkdir "%backup_folder%"

# Main loop
:loop
  # Generate timestamp (YYYYMMDD_HHMMSS)
  for /f "tokens=2 delims==" %%G in ('wmic os get localdatetime /value') do set "timestamp=%%G"
  set "timestamp=!timestamp:~0,8!_!timestamp:~8,6!"

  # Copy current history file
  robocopy "%LOCALAPPDATA%\Google\Chrome\User Data\Default" "%backup_folder%" "History" /R:1 /W:1 /NP
  ren "%backup_folder%\History" "History_!timestamp!.db"

  # Delete backups older than 7 days
  forfiles /p "%backup_folder%" /m "History_*.db" /d -%days_to_keep% /c "cmd /c del @path"

  # Wait 60 seconds
  timeout /t 60 >nul
goto loop

```


## 🔹 PowerShell Version (ChromeHistoryBackup.ps1) (Recommended)

```

$chrome_history = "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\History"
$backup_folder = "C:\ChromeHistoryBackup"
$days_to_keep = 7

# Create backup folder if missing
if (-not (Test-Path $backup_folder)) { New-Item -ItemType Directory -Path $backup_folder }

while ($true) {
    $timestamp = Get-Date -Format "yyyyMMdd_HHmmss"
    $backup_path = "$backup_folder\History_$timestamp.db"
    
    # Copy current history
    Copy-Item $chrome_history -Destination $backup_path
    
    # Delete old backups
    Get-ChildItem "$backup_folder\History_*.db" | 
        Where-Object { $_.LastWriteTime -lt (Get-Date).AddDays(-$days_to_keep) } | 
        Remove-Item -Force
    
    Start-Sleep -Seconds 60
}

```


2️⃣ Run the Script in Background (Hidden Scheduled Task)

🛠️ Method 1: Using Task Scheduler (Best for Auto-Start)

## Open Task Scheduler

Press Win + R, type taskschd.msc, hit Enter.

Create a New Task

Right-click Task Scheduler Library → Create Task.

Configure General Settings

Name: Chrome History Backup

Check: "Run whether user is logged on or not"

Check: "Run with highest privileges"

Set Trigger

"At startup" (to run automatically on system boot).

Set Action

## For Batch:


Program: cmd.exe

```

Arguments: /c "C:\path\to\ChromeHistoryBackup.bat"

```

## For PowerShell:



Program: powershell.exe

```

Arguments: -WindowStyle Hidden -File "C:\path\to\ChromeHistoryBackup.ps1"

```


## Accessing Backups

Backups are saved in:

                                 C:\ChromeHistoryBackup\History_YYYYMMDD_HHMMSS.db



 ## Notes

 use DB Browser for sql-lite to view chrome history

 first save the history file as .bat otherwise you want see it

 then in db browser click on open database and select the history file
