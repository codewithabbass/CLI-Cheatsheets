<div align="center">

# 🪟 Windows & PowerShell Commands

</div>

[← Back to Home](../README.md)

---

## Table of Contents

- [File System Navigation](#-file-system-navigation)
- [File & Directory Operations](#-file--directory-operations)
- [File Content & Search](#-file-content--search)
- [Process Management](#-process-management)
- [Network Commands](#-network-commands)
- [System Information](#-system-information)
- [Environment Variables](#-environment-variables)
- [User & Permissions](#-user--permissions)
- [Services](#-services)
- [PowerShell Pipelines & Objects](#-powershell-pipelines--objects)
- [PowerShell Modules](#-powershell-modules)
- [Scripting Essentials](#-scripting-essentials)
- [Windows Package Manager (winget)](#-windows-package-manager-winget)

---

## 📁 File System Navigation

| Task | PowerShell | CMD |
|------|-----------|-----|
| Current directory | `Get-Location` / `pwd` | `cd` / `echo %cd%` |
| Change directory | `Set-Location C:\Users` / `cd C:\Users` | `cd C:\Users` |
| Go up one level | `cd ..` | `cd ..` |
| Go to home | `cd ~` | `cd %USERPROFILE%` |
| List files | `Get-ChildItem` / `ls` / `dir` | `dir` |
| List hidden files | `Get-ChildItem -Force` | `dir /a` |
| List recursively | `Get-ChildItem -Recurse` | `dir /s` |

```powershell
Get-ChildItem                            # List current directory
Get-ChildItem -Path C:\Users             # List specific path
Get-ChildItem -Filter *.txt              # Filter by extension
Get-ChildItem -Recurse -Filter *.log     # Recursive filter
Get-ChildItem | Sort-Object LastWriteTime -Descending  # Sort by date
```

---

## 📂 File & Directory Operations

### Create

```powershell
New-Item -ItemType File -Name "file.txt"        # Create file
New-Item -ItemType Directory -Name "MyFolder"   # Create directory
mkdir MyFolder                                  # (shortcut)
"Hello World" | Out-File file.txt               # Create file with content
```

---

### Copy, move, delete

```powershell
Copy-Item file.txt C:\backup\                   # Copy file
Copy-Item C:\src -Destination C:\dst -Recurse   # Copy directory

Move-Item file.txt C:\archive\                  # Move file
Move-Item old-name.txt new-name.txt             # Rename file

Remove-Item file.txt                            # Delete file
Remove-Item C:\MyFolder -Recurse -Force         # Delete directory
```

> ⚠️ `Remove-Item -Recurse -Force` is irreversible. Double-check the path.

---

### Check existence

```powershell
Test-Path C:\Users\file.txt              # Returns True/False
Test-Path C:\MyFolder -PathType Container  # Check if directory
if (Test-Path .\config.json) { "exists" }
```

---

## 📖 File Content & Search

```powershell
Get-Content file.txt                     # Read file (like cat)
Get-Content file.txt -Tail 20            # Last 20 lines (like tail)
Get-Content file.txt -Head 10            # First 10 lines (like head)
Get-Content file.txt -Wait               # Follow new content (like tail -f)

Set-Content file.txt "New content"       # Overwrite file
Add-Content file.txt "Append this"       # Append to file

# Search within files
Select-String -Pattern "error" -Path *.log           # Grep equivalent
Select-String -Pattern "error" -Path *.log -Recurse  # Recursive
Select-String -Pattern "^\d+" -Path file.txt          # Regex match
Get-ChildItem -Recurse | Select-String "TODO"         # Search all files
```

---

## ⚙️ Process Management

```powershell
Get-Process                              # List all processes (like ps)
Get-Process -Name chrome                 # Filter by name
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
Stop-Process -Name notepad               # Kill by name
Stop-Process -Id 1234                    # Kill by PID
Stop-Process -Name chrome -Force         # Force kill
Start-Process notepad                    # Start process
Start-Process cmd -Verb RunAs            # Run as Administrator

# Tasklist / Taskkill (CMD equivalents)
tasklist                                 # List processes (CMD)
taskkill /IM notepad.exe /F              # Kill by image name
taskkill /PID 1234 /F                    # Kill by PID
```

---

## 🌐 Network Commands

```powershell
# Connectivity
ping google.com                          # Ping host
Test-Connection google.com               # PowerShell ping
Test-Connection google.com -Count 3      # Limited pings
Test-NetConnection google.com -Port 443  # Test TCP port

# DNS
Resolve-DnsName google.com               # DNS lookup
nslookup google.com                      # DNS (CMD style)
ipconfig /flushdns                       # Flush DNS cache

# IP and interfaces
Get-NetIPAddress                         # All IP addresses
Get-NetAdapter                           # Network adapters
ipconfig                                 # IP config (CMD)
ipconfig /all                            # Detailed IP config
netstat -an                              # All connections
netstat -b                               # Connections with executables

# Routes and firewall
Get-NetRoute                             # Routing table
route print                              # Route table (CMD)
tracert google.com                       # Traceroute (CMD)
Test-NetConnection -TraceRoute google.com  # PowerShell traceroute

# Firewall
Get-NetFirewallRule -Enabled True        # Active firewall rules
New-NetFirewallRule -DisplayName "Allow HTTP" -Direction Inbound -Port 80 -Protocol TCP -Action Allow
```

---

## 💻 System Information

```powershell
Get-ComputerInfo                         # Full system summary
$env:COMPUTERNAME                        # Computer name
$env:USERNAME                            # Current user
$env:OS                                  # OS name

# Hardware
Get-CimInstance Win32_OperatingSystem    # OS details
Get-CimInstance Win32_Processor          # CPU info
Get-CimInstance Win32_PhysicalMemory     # RAM info
Get-CimInstance Win32_LogicalDisk        # Disk info

# Disk usage
Get-PSDrive                              # Drive space overview
Get-PSDrive C | Select-Object Used,Free  # C: drive space

# Uptime
(Get-Date) - (gcim Win32_OperatingSystem).LastBootUpTime

# Windows version
winver                                   # GUI version dialog
[System.Environment]::OSVersion         # Programmatic version

# Event logs
Get-EventLog -LogName System -Newest 20  # Recent system events
Get-EventLog -LogName Application -EntryType Error
```

---

## 🔐 Environment Variables

```powershell
# Read
$env:PATH                                # Read PATH
$env:USERPROFILE                         # User profile path
$env:APPDATA                             # AppData path
[System.Environment]::GetEnvironmentVariable("PATH", "Machine")

# Set (current session only)
$env:MY_VAR = "value"

# Set permanently (user level)
[System.Environment]::SetEnvironmentVariable("MY_VAR", "value", "User")

# Set permanently (system level — requires admin)
[System.Environment]::SetEnvironmentVariable("MY_VAR", "value", "Machine")

# Remove
Remove-Item Env:MY_VAR                   # Session only
[System.Environment]::SetEnvironmentVariable("MY_VAR", $null, "User")

# CMD equivalents
set MY_VAR=value                         # Session only
setx MY_VAR "value"                      # Persistent (user)
setx MY_VAR "value" /M                   # Persistent (machine, needs admin)
```

---

## 👤 User & Permissions

```powershell
whoami                                   # Current user
whoami /groups                           # Group memberships
net user                                 # List local users
net user username                        # User details
net user newuser Password1 /add          # Create local user
net localgroup administrators username /add  # Add to admins

# Check if running as admin
([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)

# File permissions (ACL)
Get-Acl C:\MyFolder                      # View permissions
icacls C:\MyFolder                       # View/modify (CMD)
icacls C:\MyFolder /grant user:F         # Grant full control
```

---

## 🔧 Services

```powershell
Get-Service                              # List all services
Get-Service -Name wuauserv              # Windows Update service
Get-Service | Where-Object Status -EQ Running

Start-Service -Name Spooler              # Start service
Stop-Service -Name Spooler               # Stop service
Restart-Service -Name Spooler            # Restart service

Set-Service -Name Spooler -StartupType Automatic   # Set to auto-start
Set-Service -Name Spooler -StartupType Disabled    # Disable service

# CMD equivalents
sc query                                 # List services
net start Spooler                        # Start service
net stop Spooler                         # Stop service
sc config Spooler start= auto            # Set startup type
```

---

## 🔀 PowerShell Pipelines & Objects

```powershell
# Filter with Where-Object
Get-Process | Where-Object { $_.CPU -gt 10 }
Get-ChildItem | Where-Object { $_.Length -gt 1MB }

# Select properties
Get-Process | Select-Object Name, CPU, Id
Get-ChildItem | Select-Object Name, Length, LastWriteTime

# Sort
Get-Process | Sort-Object CPU -Descending
Get-ChildItem | Sort-Object Length

# Group
Get-Process | Group-Object -Property Company

# Measure
Get-ChildItem | Measure-Object Length -Sum -Average

# Format output
Get-Process | Format-Table                     # Table view
Get-Process | Format-List                      # List view
Get-Process | Out-GridView                     # GUI table (interactive)
Get-Process | Export-Csv processes.csv -NoTypeInformation
Get-Process | ConvertTo-Json | Out-File procs.json

# Count
(Get-Process).Count
Get-Process | Measure-Object | Select-Object Count
```

---

## 📦 PowerShell Modules

```powershell
Get-Module                               # Loaded modules
Get-Module -ListAvailable                # All installed modules
Install-Module -Name Az                  # Install from PSGallery
Install-Module -Name Az -Scope CurrentUser
Update-Module -Name Az                   # Update module
Remove-Module -Name Az                   # Unload module
Uninstall-Module -Name Az                # Remove module
Import-Module Az                         # Load module manually
Find-Module "Azure*"                     # Search PSGallery
```

---

## 📝 Scripting Essentials

### Variables and types

```powershell
$name = "Alice"                         # String
$age = 30                               # Integer
$pi = 3.14                              # Double
$active = $true                         # Boolean
$items = @("a", "b", "c")              # Array
$map = @{ key = "value"; n = 42 }      # Hashtable
```

---

### Control flow

```powershell
if ($age -gt 18) { "adult" } else { "minor" }

foreach ($item in $items) { Write-Host $item }

for ($i = 0; $i -lt 10; $i++) { $i }

while ($true) { break }

switch ($name) {
  "Alice" { "Hello Alice" }
  "Bob"   { "Hello Bob" }
  default { "Hello stranger" }
}
```

---

### Useful functions

```powershell
Write-Host "Message"                    # Print to console
Write-Output "Value"                    # Output to pipeline
Write-Error "Something failed"          # Write error
Write-Warning "Be careful"              # Write warning
Read-Host "Enter value"                 # Prompt for input

# String operations
"hello world".ToUpper()
"  trim me  ".Trim()
"hello" -replace "l", "L"
"a,b,c" -split ","
@("a", "b") -join "-"

# Math
[Math]::Round(3.567, 2)
[Math]::Abs(-5)
```

---

### Execution policy

```powershell
Get-ExecutionPolicy                      # Current policy
Get-ExecutionPolicy -List                # All scopes
Set-ExecutionPolicy RemoteSigned         # Allow local scripts (system)
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser  # Per-user
Set-ExecutionPolicy Bypass -Scope Process  # Bypass for current session only
```

> ⚠️ Only change execution policy if you understand the security implications.

---

## 📦 Windows Package Manager (winget)

> `winget` is the official Windows Package Manager CLI, built into Windows 11 and available for Windows 10 via the App Installer.

### Search & install

```powershell
winget search nodejs                     # Search for a package
winget show Microsoft.VisualStudioCode   # Show package details
winget install Microsoft.VisualStudioCode          # Install a package
winget install --id Git.Git -e           # Install exact match by ID
winget install --id Git.Git -e --silent  # Silent install (no UI)
```

### Upgrade & uninstall

```powershell
winget upgrade                           # List all upgradable packages
winget upgrade --all                     # Upgrade everything
winget upgrade --id Microsoft.PowerShell # Upgrade specific package
winget uninstall --id Git.Git            # Uninstall a package
```

### List installed packages

```powershell
winget list                              # All installed packages managed by winget
winget list --id Git.Git                 # Check if a specific package is installed
```

### Export & import (reproducible setups)

```powershell
# Export installed packages to a file (great for dotfiles)
winget export -o winget-packages.json

# Install all packages from a file on a new machine
winget import -i winget-packages.json --ignore-unavailable
```

### Common package IDs

| Package | winget ID |
|---------|----------|
| Git | `Git.Git` |
| Node.js LTS | `OpenJS.NodeJS.LTS` |
| VS Code | `Microsoft.VisualStudioCode` |
| Windows Terminal | `Microsoft.WindowsTerminal` |
| PowerShell 7 | `Microsoft.PowerShell` |
| Python | `Python.Python.3.12` |
| 7-Zip | `7zip.7zip` |
| Postman | `Postman.Postman` |
| Docker Desktop | `Docker.DockerDesktop` |

> Use `winget search <name>` to find the exact ID for any package.

--- | [Back to Home](../README.md)
