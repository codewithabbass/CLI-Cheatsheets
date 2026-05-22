<div align="center">

# 🐧 Linux Commands

</div>

[← Back to Home](../README.md)

---

## Table of Contents

- [File & Directory Commands](#-file--directory-commands)
- [Permissions](#-permissions)
- [File Viewing & Editing](#-file-viewing--editing)
- [Searching](#-searching)
- [Process Management](#-process-management)
- [Networking](#-networking)
- [Disk & System Information](#-disk--system-information)
- [Package Management](#-package-management)
- [Archive & Compression](#-archive--compression)
- [User Management](#-user-management)

---

## 📁 File & Directory Commands

### Navigate directories

```bash
pwd                  # Print working directory
cd /path/to/dir      # Change directory (absolute)
cd ..                # Go up one level
cd ~                 # Go to home directory
cd -                 # Go to previous directory
```

---

### List files

```bash
ls                   # List files
ls -l                # Long format (permissions, size, date)
ls -a                # Include hidden files
ls -lh               # Human-readable sizes
ls -lt               # Sort by modification time (newest first)
ls -R                # Recursive listing
```

---

### Create files and directories

```bash
touch file.txt             # Create empty file (or update timestamp)
mkdir my-folder            # Create a directory
mkdir -p a/b/c             # Create nested directories
```

---

### Copy, move, rename, delete

```bash
cp source.txt dest.txt          # Copy file
cp -r /src /dest                # Copy directory recursively
mv old.txt new.txt              # Rename or move file
mv /old/path /new/path          # Move directory
rm file.txt                     # Delete file
rm -r my-folder                 # Delete directory and contents
rm -rf my-folder                # Force-delete (no prompt)
```

> ⚠️ `rm -rf` is irreversible. Always double-check before running.

---

### Create symbolic links

```bash
ln -s /path/to/original /path/to/link
```

---

## 🔐 Permissions

### View permissions

```bash
ls -l
```

Output format: `-rwxr-xr-x 1 user group size date filename`

| Character | Meaning |
|-----------|---------|
| `r` | Read |
| `w` | Write |
| `x` | Execute |
| `-` | No permission |

---

### Change permissions (`chmod`)

```bash
chmod 755 file.sh        # rwxr-xr-x
chmod 644 file.txt       # rw-r--r--
chmod +x script.sh       # Add execute for all
chmod -w file.txt        # Remove write for all
chmod u+x file.sh        # Add execute for owner
```

**Common permission numbers:**

| Octal | Symbolic | Description |
|-------|----------|-------------|
| `777` | `rwxrwxrwx` | Everyone full access |
| `755` | `rwxr-xr-x` | Owner full, others read+execute |
| `644` | `rw-r--r--` | Owner read+write, others read |
| `600` | `rw-------` | Owner only |

---

### Change ownership (`chown`)

```bash
chown user file.txt               # Change owner
chown user:group file.txt         # Change owner and group
chown -R user:group /path         # Recursive
sudo chown root:root /etc/file    # Change to root
```

---

## 👁️ File Viewing & Editing

### View file contents

```bash
cat file.txt            # Print entire file
less file.txt           # Paginated viewer (q to quit)
more file.txt           # Basic pager
head file.txt           # First 10 lines
head -n 20 file.txt     # First 20 lines
tail file.txt           # Last 10 lines
tail -n 20 file.txt     # Last 20 lines
tail -f app.log         # Follow live updates
```

---

### Edit files

```bash
nano file.txt           # Simple editor (Ctrl+O save, Ctrl+X exit)
vim file.txt            # Vi improved editor
```

**Vim quick reference:**

| Command | Action |
|---------|--------|
| `i` | Enter insert mode |
| `Esc` | Exit insert mode |
| `:w` | Save |
| `:q` | Quit |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |
| `dd` | Delete current line |
| `yy` | Copy (yank) line |
| `p` | Paste |

---

## 🔎 Searching

### Find files (`find`)

```bash
find /path -name "file.txt"              # Find by name
find . -name "*.log"                     # Find by pattern
find . -type f -name "*.md"              # Files only
find . -type d                           # Directories only
find . -mtime -7                         # Modified in last 7 days
find . -size +100M                       # Files larger than 100MB
find . -name "*.tmp" -delete             # Find and delete
```

---

### Search inside files (`grep`)

```bash
grep "pattern" file.txt                  # Search in file
grep -r "pattern" /path                  # Recursive search
grep -i "pattern" file.txt               # Case-insensitive
grep -n "pattern" file.txt               # Show line numbers
grep -v "pattern" file.txt               # Invert match (exclude)
grep -l "pattern" /path/*.txt            # Only show filenames
grep -c "pattern" file.txt               # Count matches
grep -E "regex" file.txt                 # Extended regex
```

---

### Locate a file

```bash
locate filename                          # Fast search (uses DB)
sudo updatedb                            # Update locate database
which python3                            # Find executable path
whereis python3                          # Find binary, source, man pages
```

---

## ⚙️ Process Management

### View processes

```bash
ps aux                    # All processes (all users)
ps aux | grep nginx        # Filter by name
top                        # Live process monitor
htop                       # Enhanced live monitor (if installed)
```

---

### Manage processes

```bash
kill <PID>                # Send SIGTERM (graceful stop)
kill -9 <PID>             # Force kill (SIGKILL)
pkill nginx               # Kill by process name
killall node              # Kill all processes named 'node'
```

---

### Background & foreground jobs

```bash
command &                 # Run command in background
jobs                      # List background jobs
fg %1                     # Bring job 1 to foreground
bg %1                     # Resume job 1 in background
Ctrl+Z                    # Suspend current process
Ctrl+C                    # Terminate current process
```

---

### Run processes persistently (`nohup`, `screen`)

```bash
nohup ./script.sh &               # Keep running after logout
screen -S session-name            # Start named screen session
screen -ls                        # List screen sessions
screen -r session-name            # Reattach to session
Ctrl+A then D                     # Detach from screen
```

---

## 🌐 Networking

### Check connectivity

```bash
ping google.com                   # Test connectivity
ping -c 4 google.com              # Limit to 4 packets
traceroute google.com             # Trace route to host
mtr google.com                    # Live traceroute
```

---

### Inspect network configuration

```bash
ip addr                           # Show IP addresses
ip addr show eth0                 # Specific interface
ip route                          # Show routing table
hostname -I                       # Show local IPs
```

---

### DNS lookup

```bash
nslookup google.com               # DNS lookup
dig google.com                    # Detailed DNS query
dig +short google.com             # Short answer only
host google.com                   # Simple DNS lookup
```

---

### Active connections

```bash
ss -tulnp                         # Listening TCP/UDP ports
netstat -tulnp                    # (legacy, same info)
ss -s                             # Connection summary
lsof -i :80                       # What's using port 80
```

---

### Download files

```bash
wget https://example.com/file.zip
wget -O output.zip https://example.com/file.zip

curl -O https://example.com/file.zip
curl -L -o output.zip https://example.com/file.zip
curl -I https://example.com             # Show headers only
```

---

### SSH

```bash
ssh user@host                            # Connect
ssh user@host -p 2222                    # Custom port
ssh -i ~/.ssh/key.pem user@host          # With private key
scp file.txt user@host:/remote/path      # Copy file to remote
scp user@host:/remote/file.txt ./        # Copy file from remote
scp -r ./folder user@host:/remote/       # Copy directory
```

---

## 💽 Disk & System Information

### Disk usage

```bash
df -h                             # Disk space (human-readable)
du -sh /path                      # Directory size
du -sh *                          # Size of each item in current dir
du -ah --max-depth=1 /var         # Shallow breakdown
```

---

### System info

```bash
uname -a                          # Full system info
uname -r                          # Kernel version
hostname                          # System hostname
uptime                            # How long running, load average
whoami                            # Current user
id                                # User ID, group info
date                              # Current date/time
timedatectl                       # Timezone and time settings
```

---

### Memory

```bash
free -h                           # RAM and swap usage
cat /proc/meminfo                 # Detailed memory info
```

---

### CPU

```bash
lscpu                             # CPU architecture details
nproc                             # Number of processing units
cat /proc/cpuinfo                 # Raw CPU info
```

---

## 📦 Package Management

### Debian / Ubuntu (apt)

```bash
sudo apt update                         # Refresh package lists
sudo apt upgrade                        # Upgrade installed packages
sudo apt install <package>              # Install package
sudo apt remove <package>               # Remove package
sudo apt purge <package>                # Remove + config files
sudo apt autoremove                     # Remove orphan packages
apt search <keyword>                    # Search packages
apt show <package>                      # Package info
```

---

### RHEL / CentOS / Fedora (dnf / yum)

```bash
sudo dnf update                         # Update all packages
sudo dnf install <package>              # Install
sudo dnf remove <package>               # Remove
dnf search <keyword>                    # Search
dnf info <package>                      # Package details
```

---

### Arch Linux (pacman)

```bash
sudo pacman -Syu                        # Update system
sudo pacman -S <package>                # Install
sudo pacman -R <package>                # Remove
sudo pacman -Ss <keyword>               # Search
```

---

## 🗜️ Archive & Compression

### tar

```bash
# Create archive
tar -czf archive.tar.gz /path           # .tar.gz (gzip)
tar -cjf archive.tar.bz2 /path         # .tar.bz2 (bzip2)
tar -caf archive.tar.xz /path          # .tar.xz (xz)

# Extract archive
tar -xzf archive.tar.gz
tar -xzf archive.tar.gz -C /dest       # Extract to specific dir

# List contents
tar -tzf archive.tar.gz
```

---

### zip / unzip

```bash
zip -r archive.zip /path               # Create zip
unzip archive.zip                      # Extract zip
unzip archive.zip -d /dest             # Extract to directory
unzip -l archive.zip                   # List contents
```

---

## 👤 User Management

```bash
whoami                                  # Current user
id                                      # UID, GID info
sudo command                            # Run as root
su - username                           # Switch user
adduser username                        # Add new user
usermod -aG groupname username          # Add user to group
passwd username                         # Change password
userdel -r username                     # Delete user and home dir
groups username                         # List user's groups
```

---

[← Back to Home](../README.md) | [🌿 Git →](git.md)
