# 🐧 Linux Command Reference Guide

> A complete, beginner-friendly documentation of essential Linux commands covering Login, Disk Usage, and Process Management.

---

## 📚 Table of Contents

1. [Login Related Commands](#1-login-related-commands)
   - [ssh](#-ssh--secure-shell)
2. [Disk Usage Commands](#2-disk-usage-commands)
   - [df](#-df--disk-free)
   - [du](#-du--disk-usage)
3. [Process Management Commands](#3-process-management-commands)
   - [ps](#-ps--process-status)
   - [top](#-top--table-of-processes)
   - [fuser](#-fuser--file-user)
   - [kill](#-kill)
   - [nohup](#-nohup--no-hang-up)
   - [free](#-free--memory-usage)
   - [vmstat](#-vmstat--virtual-memory-statistics)
4. [Quick Reference Cheat Sheet](#4-quick-reference-cheat-sheet)

---

## 1. Login Related Commands

### 🔐 `ssh` — Secure Shell

**What it does:**
`ssh` (Secure Shell) is a cryptographic network protocol used to securely log into a remote machine over an unsecured network. It encrypts all traffic to eliminate eavesdropping.

**Basic Syntax:**
```bash
ssh [OPTIONS] user@hostname_or_IP
```

**Common Examples:**

```bash
# Basic login to a remote server
ssh john@192.168.1.10

# Login using a specific port (default is 22)
ssh -p 2222 john@192.168.1.10

# Login using a private key file
ssh -i ~/.ssh/my_key.pem john@192.168.1.10

# Login and run a command directly (without interactive shell)
ssh john@192.168.1.10 "ls -la /var/www"

# Login with verbose output (for debugging)
ssh -v john@192.168.1.10

# Forward a local port to a remote port (port tunneling)
ssh -L 8080:localhost:80 john@192.168.1.10

# Copy SSH public key to remote server (passwordless login setup)
ssh-copy-id john@192.168.1.10
```

**Common Options:**

| Option | Description |
|--------|-------------|
| `-p PORT` | Connect on a specific port |
| `-i FILE` | Use a specific private key file |
| `-v` | Verbose mode (for debugging) |
| `-L local:remote` | Local port forwarding |
| `-R remote:local` | Remote port forwarding |
| `-N` | Do not execute a remote command (useful for tunneling) |
| `-X` | Enable X11 forwarding (GUI apps over SSH) |
| `-C` | Enable compression |

**SSH Key Setup (Passwordless Login):**
```bash
# Step 1: Generate SSH key pair on your local machine
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# Step 2: Copy public key to the remote server
ssh-copy-id user@remote_host

# Step 3: Now login without password
ssh user@remote_host
```

**SSH Config File (`~/.ssh/config`):**
```bash
# Create shortcuts for frequent servers
Host myserver
    HostName 192.168.1.10
    User john
    Port 2222
    IdentityFile ~/.ssh/my_key.pem

# Now login simply with:
ssh myserver
```

> 💡 **Tip:** Always use SSH keys instead of passwords for better security and convenience.

---

## 2. Disk Usage Commands

### 💾 `df` — Disk Free

**What it does:**
`df` reports the amount of disk space used and available on mounted filesystems.

**Basic Syntax:**
```bash
df [OPTIONS] [FILE/FILESYSTEM]
```

**Common Examples:**

```bash
# Show disk usage of all mounted filesystems
df

# Show in human-readable format (KB, MB, GB)
df -h

# Show disk usage for a specific directory/filesystem
df -h /home

# Show filesystem type as well
df -hT

# Show all filesystems including dummy ones
df -ha

# Show disk usage in 1K blocks (default)
df -k

# Show inode usage instead of block usage
df -i

# Show output in specific block sizes
df --block-size=MB
```

**Sample Output Explained:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
tmpfs           1.9G     0  1.9G   0% /dev/shm
/dev/sdb1       100G   55G   40G  58% /data
```

| Column | Description |
|--------|-------------|
| `Filesystem` | The name of the filesystem/partition |
| `Size` | Total size of the filesystem |
| `Used` | Space currently used |
| `Avail` | Available space remaining |
| `Use%` | Percentage of space used |
| `Mounted on` | Mount point in the directory tree |

**Common Options:**

| Option | Description |
|--------|-------------|
| `-h` | Human-readable sizes (K, M, G) |
| `-H` | Human-readable using powers of 1000 |
| `-T` | Show filesystem type |
| `-i` | Show inode usage |
| `-a` | Include all filesystems |
| `-k` | Show sizes in 1K blocks |

> ⚠️ **Watch out:** When `Use%` approaches 100%, the system may become unstable. Keep disk usage below 80%.

---

### 📁 `du` — Disk Usage

**What it does:**
`du` estimates and reports file and directory space usage. Unlike `df` (which shows filesystem-level info), `du` drills down into directories and files.

**Basic Syntax:**
```bash
du [OPTIONS] [FILE/DIRECTORY]
```

**Common Examples:**

```bash
# Show disk usage of current directory recursively
du

# Human-readable output
du -h

# Show total size of a specific directory only (not subdirectories)
du -sh /var/log

# Show sizes of all items in a directory (one level deep)
du -h --max-depth=1 /home

# Show sizes and sort from largest to smallest
du -h /home | sort -rh

# Show the largest directories in /var
du -h /var/* | sort -rh | head -10

# Show disk usage of each file (not just directories)
du -ah /etc

# Exclude certain file types
du -h --exclude="*.log" /var

# Show size in MB
du -m /home/john
```

**Sample Output:**
```bash
$ du -sh /home/*
1.2G    /home/alice
500M    /home/bob
3.8G    /home/john
```

**Common Options:**

| Option | Description |
|--------|-------------|
| `-h` | Human-readable sizes |
| `-s` | Display only total for each argument |
| `-a` | Show sizes of all files, not just directories |
| `-c` | Produce a grand total |
| `--max-depth=N` | Limit recursion to N levels deep |
| `-m` | Show sizes in Megabytes |
| `-k` | Show sizes in Kilobytes |
| `--exclude=PATTERN` | Exclude files matching pattern |

**`df` vs `du` — Key Difference:**

| Feature | `df` | `du` |
|---------|------|------|
| Scope | Whole filesystem / partition | Specific directory or file |
| Use case | How full is my disk? | What's taking up the space? |
| Speed | Very fast | Can be slow on large dirs |

> 💡 **Tip:** Use `df -h` first to check which partition is full, then use `du -sh /*` to find what's consuming space.

---

## 3. Process Management Commands

### 🔍 `ps` — Process Status

**What it does:**
`ps` displays information about currently running processes. It takes a snapshot of the process table at the moment it is run.

**Basic Syntax:**
```bash
ps [OPTIONS]
```

**Common Examples:**

```bash
# Show processes running in current shell session
ps

# Show all processes for all users (most common usage)
ps aux

# Show processes in a tree/hierarchy format
ps auxf

# Show processes for a specific user
ps -u john

# Show a specific process by PID
ps -p 1234

# Show processes and sort by CPU usage
ps aux --sort=-%cpu

# Show processes and sort by memory usage
ps aux --sort=-%mem

# Show only the PID and name of all processes
ps -eo pid,comm

# Find a process by name
ps aux | grep nginx
```

**`ps aux` Output Explained:**
```
USER       PID  %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1   0.0  0.1  16952  1076 ?        Ss   08:00   0:01 /sbin/init
john      1234   2.5  3.2 354120 32000 pts/0    Sl   10:15   0:30 python3 app.py
```

| Column | Description |
|--------|-------------|
| `USER` | The user who owns the process |
| `PID` | Process ID (unique identifier) |
| `%CPU` | CPU usage percentage |
| `%MEM` | Memory usage percentage |
| `VSZ` | Virtual memory size (KB) |
| `RSS` | Resident set size — actual RAM used (KB) |
| `TTY` | Terminal associated with process |
| `STAT` | Process state (S=sleeping, R=running, Z=zombie) |
| `START` | Time the process started |
| `TIME` | Total CPU time consumed |
| `COMMAND` | The command that started the process |

**Process STAT Codes:**

| Code | Meaning |
|------|---------|
| `R` | Running |
| `S` | Sleeping (interruptible) |
| `D` | Sleeping (uninterruptible, usually I/O) |
| `Z` | Zombie (finished but not cleaned up) |
| `T` | Stopped |
| `s` | Session leader |
| `l` | Multi-threaded |
| `+` | Foreground process group |

---

### 📊 `top` — Table of Processes

**What it does:**
`top` provides a real-time, dynamic view of running processes. It continuously updates and shows system summary and a list of processes sorted by CPU usage by default.

**Basic Syntax:**
```bash
top [OPTIONS]
```

**Launch and Interactive Commands:**

```bash
# Start top
top

# Start top showing processes of a specific user
top -u john

# Start top with a specific refresh interval (e.g., every 2 seconds)
top -d 2

# Run top in batch mode (useful for logging/scripting)
top -b -n 1

# Sort by memory usage from the start
top -o %MEM
```

**Interactive Keyboard Shortcuts (while top is running):**

| Key | Action |
|-----|--------|
| `q` | Quit top |
| `k` | Kill a process (prompts for PID) |
| `r` | Renice a process (change priority) |
| `M` | Sort by Memory usage |
| `P` | Sort by CPU usage (default) |
| `T` | Sort by Time |
| `u` | Filter by specific user |
| `1` | Toggle individual CPU cores view |
| `h` | Show help |
| `f` | Field management (add/remove columns) |
| `z` | Toggle color mode |
| `Space` | Refresh immediately |

**Understanding `top` Output:**
```
top - 14:32:01 up 5 days,  3:21,  2 users,  load average: 0.52, 0.58, 0.59
Tasks: 213 total,   1 running, 212 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.3 us,  0.7 sy,  0.0 ni, 96.8 id,  0.1 wa,  0.0 hi,  0.1 si
MiB Mem :  15906.9 total,   4821.3 free,   8234.5 used,   2851.1 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   7312.0 avail Mem
```

| Field | Description |
|-------|-------------|
| `load average` | System load over 1, 5, 15 minutes |
| `us` | User space CPU % |
| `sy` | System/kernel CPU % |
| `id` | Idle CPU % |
| `wa` | CPU waiting for I/O % |
| `buff/cache` | Memory used for buffers/cache |

> 💡 **Tip:** Use `htop` (an improved version of top) for a more colorful and user-friendly interface: `sudo apt install htop`

---

### 🔎 `fuser` — File User

**What it does:**
`fuser` identifies which processes are using a specific file, directory, or socket. Extremely useful when you can't unmount a filesystem because something is using it.

**Basic Syntax:**
```bash
fuser [OPTIONS] FILE/SOCKET
```

**Common Examples:**

```bash
# Find which processes are using a file
fuser /var/log/syslog

# Find processes using a directory
fuser /home/john

# Find which process is using a network port (e.g., port 80)
fuser 80/tcp

# Find process using UDP port
fuser 53/udp

# Show verbose output with usernames and PID details
fuser -v /var/log/syslog

# Kill all processes using a file
fuser -k /var/log/syslog

# Kill all processes using a mount point (before unmounting)
fuser -km /mnt/usb

# Show all files used by a specific process
fuser -v /proc/1234
```

**Sample Output:**
```bash
$ fuser -v /var/log/syslog
                     USER        PID ACCESS COMMAND
/var/log/syslog:     syslog     1023 F....  rsyslogd
                     root       2341 F....  tail
```

**ACCESS Code Meanings:**

| Code | Meaning |
|------|---------|
| `c` | Current directory |
| `e` | Executable being run |
| `f` | Open file |
| `F` | Open file for writing |
| `r` | Root directory |
| `m` | Memory-mapped file or shared library |

**Common Options:**

| Option | Description |
|--------|-------------|
| `-v` | Verbose output |
| `-k` | Kill all processes accessing the file |
| `-i` | Ask for confirmation before killing |
| `-u` | Show username along with PID |
| `-m` | Show all processes using the filesystem |
| `-n SPACE` | Select name space: `tcp`, `udp`, `file` |

> ⚠️ **Use Case:** If `umount /mnt/usb` gives "device is busy", run `fuser -km /mnt/usb` first.

---

### ⚡ `kill` — Terminate Processes

**What it does:**
`kill` sends a signal to a process. By default it sends `SIGTERM` (signal 15), requesting the process to terminate gracefully. You can also force-kill with `SIGKILL` (signal 9).

**Basic Syntax:**
```bash
kill [SIGNAL] PID
```

**Common Examples:**

```bash
# Gracefully terminate a process (SIGTERM - default)
kill 1234

# Force kill a process (SIGKILL - cannot be ignored)
kill -9 1234

# Same as kill -9 using signal name
kill -SIGKILL 1234

# Send SIGHUP (reload config, often used for daemons)
kill -1 1234

# Kill all processes by name (uses SIGTERM)
killall nginx

# Force kill all processes by name
killall -9 nginx

# Kill processes by name (more flexible)
pkill nginx

# Kill by pattern match
pkill -f "python3 app.py"

# Kill all processes of a specific user
pkill -u john
```

**Important Signals:**

| Signal | Number | Description |
|--------|--------|-------------|
| `SIGHUP` | 1 | Reload configuration (hangup) |
| `SIGINT` | 2 | Interrupt (same as Ctrl+C) |
| `SIGQUIT` | 3 | Quit and dump core |
| `SIGKILL` | 9 | Force kill — cannot be caught or ignored |
| `SIGTERM` | 15 | Graceful termination (default) |
| `SIGSTOP` | 19 | Pause process (cannot be caught) |
| `SIGCONT` | 18 | Resume a paused process |

**List all available signals:**
```bash
kill -l
```

**Finding PIDs to kill:**
```bash
# Method 1: using ps
ps aux | grep nginx

# Method 2: using pgrep
pgrep nginx

# Method 3: using pidof
pidof nginx
```

> 💡 **Best Practice:** Always try `kill PID` (SIGTERM) first. Only use `kill -9 PID` if the process doesn't respond, as SIGKILL prevents cleanup operations.

---

### 🔄 `nohup` — No Hang Up

**What it does:**
`nohup` runs a command that will **continue running even after you log out** of the terminal. Normally, when you close an SSH session, all processes started from it receive `SIGHUP` and terminate. `nohup` ignores this signal.

**Basic Syntax:**
```bash
nohup COMMAND [ARGS] &
```

> The `&` at the end runs the process in the background.

**Common Examples:**

```bash
# Run a script that continues after logout
nohup ./backup.sh &

# Run a Python script in background
nohup python3 app.py &

# Redirect output to a custom log file
nohup python3 app.py > myapp.log 2>&1 &

# Suppress all output (discard both stdout and stderr)
nohup python3 app.py > /dev/null 2>&1 &

# Run a long-running data processing job
nohup bash data_process.sh > process.log 2>&1 &

# Check the background job PID
echo $!
```

**Default Behavior:**
- Output (stdout) is redirected to `nohup.out` in the current directory by default
- `2>&1` redirects stderr to the same place as stdout

**Managing nohup processes:**
```bash
# After running with &, note the PID printed
nohup python3 app.py &
# Output: [1] 5678   ← This is your PID

# Check if still running
ps -p 5678

# Kill when needed
kill 5678
```

**`nohup` vs `screen` vs `tmux`:**

| Tool | Use Case |
|------|---------|
| `nohup` | Simple one-command persistence after logout |
| `screen` | Full terminal session persistence, multiple windows |
| `tmux` | Modern alternative to screen with split panes |

> 💡 **Tip:** Always redirect output explicitly: `nohup command > output.log 2>&1 &` to avoid cluttering with `nohup.out`.

---

### 🧠 `free` — Memory Usage

**What it does:**
`free` displays the total, used, and free amounts of physical RAM and swap memory in the system.

**Basic Syntax:**
```bash
free [OPTIONS]
```

**Common Examples:**

```bash
# Show memory in default (kilobytes)
free

# Show in human-readable format
free -h

# Show in megabytes
free -m

# Show in gigabytes
free -g

# Continuously display memory every 2 seconds
free -h -s 2

# Show totals row as well
free -ht

# Show memory usage just once in megabytes
free -m -c 1
```

**Sample Output Explained:**
```
              total        used        free      shared  buff/cache   available
Mem:          15906        8234        4821         312        2851        7312
Swap:          2048           0        2048
```

| Column | Description |
|--------|-------------|
| `total` | Total installed memory |
| `used` | Memory currently in use by processes |
| `free` | Completely unused memory |
| `shared` | Memory used by tmpfs (shared memory) |
| `buff/cache` | Memory used for kernel buffers and disk cache |
| `available` | Memory available for new processes (free + reclaimable cache) |

**Understanding Swap:**
- Swap is disk space used as overflow when RAM is full
- High swap usage means your system is under memory pressure
- `Swap used > 0` indicates RAM may be insufficient

> 💡 **Key Insight:** Don't panic if `free` is very low! Linux uses unused RAM for disk cache (`buff/cache`), which speeds things up. The `available` column tells you how much is truly available for programs.

**Common Options:**

| Option | Description |
|--------|-------------|
| `-h` | Human-readable output |
| `-m` | Output in megabytes |
| `-g` | Output in gigabytes |
| `-t` | Show total row |
| `-s N` | Repeat output every N seconds |
| `-c N` | Repeat N times then exit |

---

### 📈 `vmstat` — Virtual Memory Statistics

**What it does:**
`vmstat` (Virtual Memory Statistics) reports information about processes, memory, paging, block I/O, traps, and CPU activity. It gives a comprehensive real-time snapshot of system performance.

**Basic Syntax:**
```bash
vmstat [OPTIONS] [DELAY [COUNT]]
```

**Common Examples:**

```bash
# Show a one-time snapshot
vmstat

# Show stats every 2 seconds
vmstat 2

# Show stats every 2 seconds, 5 times total
vmstat 2 5

# Show stats in MB (megabytes)
vmstat -S m

# Show disk statistics (I/O per disk)
vmstat -d

# Show partition statistics
vmstat -p /dev/sda1

# Show memory stats in detail
vmstat -s

# Show active/inactive memory
vmstat -a
```

**Sample Output Explained:**
```
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 523456  12345 678901    0    0    50    30  320  560  5  2 92  1  0
```

**Column Groups:**

**Procs (Processes):**

| Column | Description |
|--------|-------------|
| `r` | Number of runnable processes (in run queue) |
| `b` | Number of processes in uninterruptible sleep |

**Memory:**

| Column | Description |
|--------|-------------|
| `swpd` | Virtual memory (swap) used (KB) |
| `free` | Idle memory (KB) |
| `buff` | Memory used as buffers (KB) |
| `cache` | Memory used as cache (KB) |

**Swap:**

| Column | Description |
|--------|-------------|
| `si` | Memory swapped in from disk (KB/s) |
| `so` | Memory swapped out to disk (KB/s) |

**I/O:**

| Column | Description |
|--------|-------------|
| `bi` | Blocks received from block device (blocks/s) |
| `bo` | Blocks sent to block device (blocks/s) |

**System:**

| Column | Description |
|--------|-------------|
| `in` | Interrupts per second |
| `cs` | Context switches per second |

**CPU:**

| Column | Description |
|--------|-------------|
| `us` | Time running user code (%) |
| `sy` | Time running kernel/system code (%) |
| `id` | Idle time (%) |
| `wa` | Time waiting for I/O (%) |
| `st` | Time stolen from VM (for virtual machines) |

**Reading vmstat for Performance Issues:**

| Symptom | Indicator |
|---------|-----------|
| High CPU load | `r` column > number of CPUs |
| Memory pressure | `so` (swap out) > 0 regularly |
| I/O bottleneck | `wa` consistently > 10% |
| Heavy disk I/O | `bi`/`bo` values are very high |
| Too many context switches | `cs` values extremely high |

> 💡 **Tip:** Run `vmstat 1 10` to capture 10 seconds of data and identify performance trends.

---

## 4. Quick Reference Cheat Sheet

### 🔐 Login

| Command | Description |
|---------|-------------|
| `ssh user@host` | Login to remote server |
| `ssh -p 2222 user@host` | Login on custom port |
| `ssh -i key.pem user@host` | Login with private key |
| `ssh-keygen -t rsa` | Generate SSH key pair |
| `ssh-copy-id user@host` | Copy public key to server |

### 💾 Disk Usage

| Command | Description |
|---------|-------------|
| `df -h` | Show disk usage (human-readable) |
| `df -hT` | Show disk usage with filesystem type |
| `du -sh /path` | Show total size of a directory |
| `du -h --max-depth=1 /path` | Show directory sizes (1 level) |
| `du -h / \| sort -rh \| head` | Find largest directories |

### ⚙️ Process Management

| Command | Description |
|---------|-------------|
| `ps aux` | Show all running processes |
| `ps aux \| grep name` | Find a process by name |
| `top` | Real-time process monitor |
| `top -u username` | Monitor a specific user's processes |
| `fuser -v /path` | Show what's using a file |
| `fuser 80/tcp` | Show what's using port 80 |
| `kill PID` | Gracefully stop a process |
| `kill -9 PID` | Force stop a process |
| `killall name` | Kill all processes by name |
| `nohup cmd > out.log 2>&1 &` | Run command that survives logout |
| `free -h` | Show memory usage |
| `vmstat 2 5` | Show system stats every 2s, 5 times |

---

## 📝 Notes & Best Practices

1. **SSH:** Always prefer key-based authentication over passwords. Keep your private key secure with a passphrase.

2. **Disk Monitoring:** Set up alerts when disk usage exceeds 80%. Use `df -h` regularly in cron jobs to monitor.

3. **Process Killing:** Always try graceful `kill PID` before resorting to `kill -9 PID`. The latter prevents apps from saving state.

4. **nohup Logging:** Always explicitly redirect output (`> log.log 2>&1`) when using nohup to track what your background process is doing.

5. **Memory:** In Linux, unused RAM is wasted RAM. The system uses it for cache. Focus on the `available` column in `free -h`, not `free`.

6. **vmstat Baseline:** Run `vmstat 1 60 > baseline.txt` on a healthy system to have a reference for future troubleshooting.

---

*📌 Document prepared as a learning reference for Linux System Administration basics.*
*🗓️ Last updated: May 2026*
