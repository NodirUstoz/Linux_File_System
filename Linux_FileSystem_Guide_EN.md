# Linux File System and CTF Fundamentals

<div align="center">

### Educational Guide for Students

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)
![CTF](https://img.shields.io/badge/CTF-FF6F00?style=for-the-badge&logo=hackthebox&logoColor=white)

</div>

---

| | |
|---|---|
| **Author** | Nodir Ustoz |
| **Level** | Beginner - Intermediate |
| **Goal** | Deep understanding of Linux file system and CTF competition preparation |
| **Reading time** | ~45 minutes |

---

> **Tip:** After studying this guide, practice on [OverTheWire Bandit](https://overthewire.org/wargames/bandit/) platform!

---

## Table of Contents

1. [Introduction and basic concepts](#1-introduction-and-basic-concepts)
2. [Linux file system architecture](#2-linux-file-system-architecture)
3. [Navigation and basic commands](#3-navigation-and-basic-commands)
4. [File search methods](#4-file-search-methods)
5. [Reading file contents](#5-reading-file-contents)
6. [Getting help - --help, man, info](#6-getting-help-----help-man-info)
7. [Text editors - nano and vim](#7-text-editors---nano-and-vim)
8. [Permissions system](#8-permissions-system)
9. [Important directories and their functions](#9-important-directories-and-their-functions)
10. [Working with archives](#10-working-with-archives)
11. [Disk and mount operations](#11-disk-and-mount-operations)
12. [Symbolic links](#12-symbolic-links)
13. [Practical exercises](#13-practical-exercises)
14. [CTF strategies](#14-ctf-strategies)

---

## 1. Introduction and basic concepts

### 1.1 What is Linux?

**Linux** is an open-source operating system kernel created by Linus Torvalds in 1991. Today, Linux is widely used on servers, supercomputers, mobile devices (Android), and many other areas.

> **Fun fact:** 100% of the world's 500 most powerful supercomputers run on Linux.

### 1.2 What is CTF (Capture The Flag)?

**CTF** is a cybersecurity competition where participants solve various challenges and find secret strings called "flags".

**Flag formats:**
```
FLAG{this_is_a_simple_flag}
CTF{capture_the_flag}
HTB{hack_the_box_flag}
flag{lowercase_letters}
```

### 1.3 Why Linux CLI (Command Line Interface)?

| Graphical Interface (GUI) | Command Line (CLI) |
|---------------------------|-------------------|
| Visual, easy to start | Fast and efficient |
| Consumes more resources | Minimal resources |
| Hard to automate | Easy to automate with scripts |
| Limited for server management | Primary tool for server management |

### 1.4 Terminal and shell

Understanding the terminal and shell is fundamental to working with Linux. These two components work together to provide a powerful command-line interface.

#### 1.4.1 What is a Terminal?

**Terminal** (also called terminal emulator) is a text-based interface window that provides a way to interact with the operating system through text commands. It's the modern equivalent of the physical terminals that were used to connect to mainframe computers.

**Key characteristics:**
- Text-based input and output
- Command-line interface (CLI)
- Can run multiple shells
- Supports colors, fonts, and customization
- Provides access to system resources

**Common terminal emulators:**
- **GNOME Terminal** - Default on GNOME desktop environments
- **Konsole** - KDE's terminal emulator
- **Terminal.app** - macOS default terminal
- **Windows Terminal** - Modern Windows terminal
- **Alacritty** - Fast, GPU-accelerated terminal
- **iTerm2** - Advanced macOS terminal

**Terminal vs Console:**
- **Terminal**: A program that emulates a physical terminal (software)
- **Console**: Physical hardware device or the system console (hardware)

#### 1.4.2 What is a Shell?

**Shell** is a command-line interpreter program that processes commands and executes them. It acts as an interface between the user and the operating system kernel. The shell reads commands from the terminal, interprets them, and executes the appropriate programs.

**Shell functions:**
- **Command execution**: Runs programs and scripts
- **Command interpretation**: Parses and processes commands
- **Environment management**: Manages environment variables
- **Scripting**: Allows automation through shell scripts
- **Job control**: Manages background processes
- **Input/output redirection**: Handles pipes and redirections

**How shell works:**
```
User Input → Terminal → Shell → Kernel → System Calls → Hardware
                ↑                                    ↓
                └────────── Output ←─────────────────┘
```

#### 1.4.3 Popular Shells

##### Bash (Bourne Again Shell)

**Bash** is the most widely used shell on Linux systems and is the default shell on most distributions.

**Features:**
- Command history with arrow keys
- Tab completion for commands and filenames
- Command aliasing
- Environment variable expansion
- Powerful scripting capabilities
- Job control (background processes)
- Brace expansion (`{1..10}`)
- Command substitution (`$(command)`)

**Checking your shell:**
```bash
echo $SHELL                    # Current shell path
echo $0                        # Shell name
ps -p $$                       # Process information
```

**Bash version:**
```bash
bash --version
# Output: GNU bash, version 5.1.16(1)-release (x86_64-pc-linux-gnu)
```

**Bash configuration files:**
- `~/.bashrc` - Interactive non-login shell configuration
- `~/.bash_profile` - Login shell configuration
- `~/.bash_history` - Command history
- `/etc/bash.bashrc` - System-wide configuration

##### Zsh (Z Shell)

**Zsh** is a powerful shell with modern features, popular among developers.

**Features:**
- Advanced tab completion
- Better globbing (pattern matching)
- Plugin system (Oh My Zsh framework)
- Spelling correction
- Theme support
- Shared command history
- Better array handling

**Installation:**
```bash
# Debian/Ubuntu
sudo apt install zsh

# CentOS/RHEL
sudo yum install zsh

# macOS
brew install zsh
```

**Making Zsh default:**
```bash
chsh -s $(which zsh)
```

**Popular Zsh frameworks:**
- **Oh My Zsh** - Community-driven framework
- **Prezto** - Configuration framework
- **Powerlevel10k** - Fast theme

##### Fish (Friendly Interactive Shell)

**Fish** is designed to be user-friendly with syntax highlighting and auto-suggestions.

**Features:**
- Syntax highlighting as you type
- Automatic suggestions based on history
- Web-based configuration
- Sane defaults (no configuration needed)
- Better error messages
- Tab completion with descriptions

**Installation:**
```bash
# Debian/Ubuntu
sudo apt install fish

# macOS
brew install fish
```

**Fish configuration:**
```bash
fish_config                    # Opens web-based configuration
```

##### sh (Bourne Shell)

**sh** is the original Unix shell, now often a symlink to another shell (like dash on Debian/Ubuntu).

**Characteristics:**
- POSIX-compliant
- Minimal features
- Fast startup time
- Used for system scripts
- Basic functionality

**Note:** Many systems use `dash` (Debian Almquist Shell) as `/bin/sh` for faster script execution.

#### 1.4.4 Shell Types: Login vs Non-Login

**Login Shell:**
- Started when you log in (SSH, console login)
- Reads: `/etc/profile`, `~/.bash_profile` or `~/.profile`
- Full environment setup

**Non-Login Shell:**
- Started from GUI terminal or `bash` command
- Reads: `~/.bashrc` or `/etc/bash.bashrc`
- Faster startup

**Checking shell type:**
```bash
echo $0                        # Shows shell name
shopt -q login_shell && echo "Login shell" || echo "Non-login shell"
```

#### 1.4.5 Interactive vs Non-Interactive Shell

**Interactive Shell:**
- User can type commands
- Prompt is displayed
- Reads from terminal
- Example: Opening a terminal window

**Non-Interactive Shell:**
- Runs scripts
- No prompt
- Reads from file or pipe
- Example: `bash script.sh`

**Checking interactivity:**
```bash
[ -t 0 ] && echo "Interactive" || echo "Non-interactive"
```

#### 1.4.6 Shell Features and Capabilities

**Command History:**
```bash
history                        # View command history
!!                             # Repeat last command
!n                             # Execute command number n
!string                        # Execute last command starting with string
Ctrl+R                         # Search history interactively
```

**Tab Completion:**
- **Single Tab**: Complete if unique
- **Double Tab**: Show all possibilities
- Works for commands, files, and directories

**Aliases:**
```bash
alias ll='ls -la'              # Create alias
alias grep='grep --color=auto' # Colorized grep
unalias ll                     # Remove alias
alias                          # List all aliases
```

**Environment Variables:**
```bash
export PATH=$PATH:/new/path    # Add to PATH
echo $HOME                     # Display variable
env                            # List all variables
set                            # List all variables and functions
```

**Input/Output Redirection:**
```bash
command > file                 # Redirect stdout
command >> file                # Append stdout
command < file                 # Redirect stdin
command 2> error.log           # Redirect stderr
command > file 2>&1            # Redirect both
command | another              # Pipe output
```

#### 1.4.7 Choosing the Right Shell

**For Beginners:**
- **Bash** - Default, well-documented, widely supported
- **Fish** - User-friendly, great for learning

**For Developers:**
- **Zsh** - Powerful, extensible, modern features
- **Bash** - Universal, works everywhere

**For System Scripts:**
- **sh** - POSIX-compliant, fast, portable

**For CTF/Competitions:**
- **Bash** - Available everywhere, essential to know

#### 1.4.8 Practical Examples

**Switching shells temporarily:**
```bash
bash                           # Switch to bash
zsh                            # Switch to zsh
fish                           # Switch to fish
exit                           # Return to previous shell
```

**Running commands in different shells:**
```bash
bash -c "echo 'Hello from Bash'"
zsh -c "echo 'Hello from Zsh'"
sh -c "echo 'Hello from sh'"
```

**Shell script shebang:**
```bash
#!/bin/bash                    # Use bash
#!/bin/sh                      # Use sh (portable)
#!/usr/bin/env bash            # Use bash from PATH
```

#### 1.4.9 Terminal and Shell Tips

**Terminal Tips:**
- Use `Ctrl+Shift+T` to open new tabs (most terminals)
- Use `Ctrl+Shift+C` to copy (most terminals)
- Use `Ctrl+Shift+V` to paste (most terminals)
- Customize colors and fonts for better readability
- Use terminal multiplexers (tmux, screen) for sessions

**Shell Tips:**
- Learn keyboard shortcuts (`Ctrl+A`, `Ctrl+E`, `Ctrl+U`, `Ctrl+K`)
- Use tab completion extensively
- Create useful aliases for common commands
- Keep your `.bashrc` or `.zshrc` organized
- Use functions for complex operations

**Essential Keyboard Shortcuts:**
- `Ctrl+C` - Interrupt current command
- `Ctrl+D` - Exit shell (EOF)
- `Ctrl+L` - Clear screen
- `Ctrl+A` - Move to beginning of line
- `Ctrl+E` - Move to end of line
- `Ctrl+U` - Delete to beginning of line
- `Ctrl+K` - Delete to end of line
- `Ctrl+W` - Delete previous word
- `Alt+.` - Insert last argument of previous command
- `Ctrl+R` - Search command history

> **Pro Tip:** Understanding terminal and shell is crucial for Linux mastery. Spend time practicing with different shells and customizing your environment. For CTF competitions, Bash is essential as it's available on virtually every Linux system.

---

## 2. Linux file system architecture

### 2.1 "Everything is a file" philosophy

In Linux, everything is represented as a file:
- Regular files (text, binary)
- Directories
- Devices - `/dev/sda`, `/dev/null`
- Processes - in `/proc/`
- Network sockets

### 2.2 File system hierarchy (FHS)

```
/                      # Root - beginning of all files
├── bin/               # Essential commands (ls, cp, mv)
├── boot/              # Boot files
├── dev/               # Device files
├── etc/               # Configuration files
├── home/              # User directories
│   ├── student/
│   └── admin/
├── lib/               # Libraries
├── media/             # Removable devices
├── mnt/               # Temporary mount points
├── opt/               # Optional software
├── proc/              # Process information (virtual)
├── root/              # Root user home directory
├── run/               # Runtime data
├── sbin/              # System commands
├── srv/               # Service data
├── sys/               # Kernel information (virtual)
├── tmp/               # Temporary files
├── usr/               # User programs
│   ├── bin/
│   ├── lib/
│   ├── local/
│   └── share/
└── var/               # Variable data
    ├── log/
    ├── www/
    └── backups/
```

### 2.3 The tree command - visualizing structure

The `tree` command displays directory structure as a tree:

```bash
tree                           # Show current directory
tree -a                        # Include hidden files
tree -L 2                      # Depth of only 2 levels
tree -d                        # Directories only
tree -f                        # Show full paths
tree -h                        # File sizes (human-readable)
tree -p                        # Show permissions
tree --dirsfirst               # Directories first
tree -I "node_modules|.git"    # Exclude patterns
```

**Usage examples:**
```bash
# Project structure with sizes
tree -L 3 -h --dirsfirst

# Only Python files
tree -P "*.py"

# Exclude certain directories
tree -I "__pycache__|*.pyc"

# Save to file
tree > structure.txt

# With colors (if supported)
tree -C
```

**Example output:**
```
.
├── src/
│   ├── main.py
│   ├── utils/
│   │   ├── helper.py
│   │   └── config.py
│   └── tests/
│       └── test_main.py
├── docs/
│   └── README.md
├── requirements.txt
└── setup.py

4 directories, 6 files
```

**Installing tree:**
```bash
# Debian/Ubuntu
sudo apt install tree

# CentOS/RHEL
sudo yum install tree

# macOS
brew install tree

# Windows (with Git Bash or WSL)
# Usually pre-installed in Git Bash
```

### 2.4 File types

Linux has 7 file types:

| Symbol | Type | Description |
|--------|------|-------------|
| `-` | Regular file | Text, binary, images, etc. |
| `d` | Directory | Contains other files |
| `l` | Symbolic link | Pointer to another file |
| `c` | Character device | Terminals, keyboard |
| `b` | Block device | Disks, USB |
| `p` | Named pipe (FIFO) | For process communication |
| `s` | Socket | Network communication |

### 2.5 Inode concept

**Inode** is a data structure that stores file metadata.

Inode contains:
- File type and permissions
- Owner and group
- File size
- Timestamps
- Pointers to data blocks

```bash
# View inode numbers
ls -li

# Information about specific inode
stat file.txt
```

---

## 3. Navigation and basic commands

### 3.1 pwd - print working directory

Shows current directory:

```bash
pwd
# Output: /home/student
```

### 3.2 cd - change directory

Moving between directories:

```bash
cd /home           # Absolute path
cd folder          # To folder in current directory
cd ..              # One level up (parent directory)
cd ../..           # Two levels up
cd ~               # To home directory
cd -               # To previous directory
cd                 # To home directory (same as cd ~)
```

### 3.3 ls - list directory contents

Displaying directory contents:

```bash
ls                 # Simple list
ls -l              # Detailed (long format)
ls -a              # All files (including hidden)
ls -la             # Detailed + all (MOST COMMONLY USED)
ls -lh             # Human-readable sizes (KB, MB, GB)
ls -lt             # Sorted by time
ls -lS             # Sorted by size
ls -lR             # Recursive (including subdirectories)
ls -li             # With inode numbers
```

### 3.4 whoami and id

User information:

```bash
whoami             # Current username
# Output: student

id                 # Full identification
# Output: uid=1000(student) gid=1000(student) groups=1000(student),27(sudo)
```

### 3.5 uname - system information

```bash
uname -a           # All system information
uname -r           # Kernel version
uname -m           # Architecture (x86_64)
uname -n           # Hostname
```

---

## 4. File search methods

### 4.1 find - powerful search tool

`find` is the most powerful search command in Linux.

**Basic syntax:**
```bash
find [where] [conditions] [action]
```

#### Search by name:

```bash
find / -name "flag.txt"                    # Exact name
find / -name "*.txt"                       # By extension
find / -name "flag*"                       # Starts with flag
find / -name "*secret*"                    # Contains secret
find / -iname "FLAG.txt"                   # Case-insensitive
```

#### Search by type:

```bash
find / -type f                             # Files only
find / -type d                             # Directories only
find / -type l                             # Symbolic links only
```

#### Search by size:

```bash
find / -size +1M                           # Larger than 1 MB
find / -size -100c                         # Less than 100 bytes (c=bytes)
find / -size +1k -size -10k                # Between 1KB and 10KB
```

#### Search by time:

```bash
find / -mtime -1                           # Modified in last 24 hours
find / -mtime +7                           # Modified more than 7 days ago
find / -mmin -30                           # Modified in last 30 minutes
find / -atime -1                           # Accessed in last 24 hours
find / -newer reference.txt                # After reference file
```

#### Search by permissions:

```bash
find / -perm 644                           # Exactly 644 permissions
find / -perm -4000                         # SUID bit set
find / -perm -2000                         # SGID bit set
find / -readable                           # Readable files
find / -writable                           # Writable files
find / -executable                         # Executable files
```

#### Executing actions with find:

```bash
find / -name "*.txt" -exec cat {} \;       # Read each file
find / -name "*.log" -exec rm {} \;        # Delete each file
find / -name "flag*" -exec ls -la {} \;    # Show detailed info
```

#### Hiding errors:

```bash
find / -name "flag*" 2>/dev/null           # Hide error messages
```

### 4.2 locate - fast search

`locate` searches from a database, so it's faster than `find`:

```bash
locate flag                                # Search for word flag
locate -i flag                             # Case-insensitive
locate "*.txt"                             # With pattern
```

### 4.3 which, whereis, type

Finding command location:

```bash
which python                               # Path to executable
whereis python                             # Binary, source, man pages
type ls                                    # Command type
```

### 4.4 Globbing (wildcard patterns)

Shell's pattern matching capability:

| Pattern | Meaning | Example |
|---------|---------|---------|
| `*` | Any characters | `*.txt` - all txt files |
| `?` | Single character | `file?.txt` - file1.txt, fileA.txt |
| `[abc]` | a, b or c | `file[123].txt` |
| `[a-z]` | from a to z | `file[a-z].txt` |
| `[!abc]` | except a, b, c | `file[!0-9].txt` |
| `{a,b}` | a or b | `file.{txt,md}` |

---

## 5. Reading file contents

### 5.1 cat - concatenate

Full file content output:

```bash
cat file.txt                               # Read file
cat -n file.txt                            # With line numbers
cat -A file.txt                            # Show special characters
cat file1.txt file2.txt                    # Combine two files
cat file1.txt file2.txt > combined.txt     # Save combination
```

### 5.2 less and more - pagination

For reading large files:

```bash
less file.txt                              # Modern pager
more file.txt                              # Simple pager
```

**Navigation in less:**
- `Space` or `f` - next page
- `b` - previous page
- `g` - beginning of file
- `G` - end of file
- `/pattern` - search
- `n` - next result
- `q` - quit

### 5.3 head and tail

Reading beginning and end of file:

```bash
head file.txt                              # First 10 lines
head -n 5 file.txt                         # First 5 lines
head -c 100 file.txt                       # First 100 bytes

tail file.txt                              # Last 10 lines
tail -n 20 file.txt                        # Last 20 lines
tail -f logfile.log                        # Real-time monitoring
```

### 5.4 strings - text from binary files

Extracting readable text from binary files:

```bash
strings binary_file                        # All texts
strings -n 10 binary_file                  # Strings 10+ characters
strings binary_file | grep flag            # Search for flag
strings binary_file | grep -E "[A-Z]{3,}\{.*\}"   # Flag format
```

> **Very important for CTF!** Many flags are hidden inside binary files.

### 5.5 file - determining file type

```bash
file mystery                               # Determine file type
file *                                     # Check all files
file -i file.txt                           # MIME type
```

### 5.6 xxd and hexdump - hex view

Viewing file in hex format:

```bash
xxd file.bin                               # Hex dump
xxd -l 100 file.bin                        # Only 100 bytes
xxd -r hex.txt > binary                    # From hex to binary

hexdump -C file.bin                        # Canonical format
```

---

## 6. Getting help - --help, man, info

> **Important for CTF:** To fully understand any command, first read its help documentation. Many CTF challenges have solutions hidden in obscure flags or options!

### 6.1 The --help flag

Almost all Linux commands support the `--help` or `-h` flag:

```bash
# Basic syntax
command --help
command -h

# Examples
ls --help                                  # Brief help about ls
grep --help                                # All grep options
find --help                                # find syntax
chmod --help                               # chmod options
cat --help                                 # Information about cat
```

**Structure of --help output:**

```
Usage: ls [OPTION]... [FILE]...        <- Syntax
List information about the FILEs       <- Description

Mandatory arguments to long options... <- Notes

  -a, --all             do not ignore entries starting with .
  |    |                |__ Description
  |    |__ Long format (--all)
  |__ Short format (-a)

  -l                    use a long listing format
  -h, --human-readable  with -l, print sizes like 1K 234M 2G
  -R, --recursive       list subdirectories recursively
  -S                    sort by file size, largest first
  -t                    sort by time, newest first
  -r, --reverse         reverse order while sorting

Exit status:           <- Exit codes
 0  if OK,
 1  if minor problems,
 2  if serious trouble.
```

### 6.2 man (manual) pages

`man` displays the most complete and detailed documentation:

```bash
# Basic syntax
man command

# Examples
man ls                                     # Full ls documentation
man grep                                   # grep manual
man find                                   # Detailed find documentation
man chmod                                  # About chmod
man bash                                   # About Bash (very long!)
```

**Navigation within man:**

| Key | Function |
|-----|----------|
| `Space` / `PgDn` | Next page |
| `b` / `PgUp` | Previous page |
| `Enter` / `Down Arrow` | One line down |
| `k` / `Up Arrow` | One line up |
| `/pattern` | Search pattern (forward) |
| `?pattern` | Search pattern (backward) |
| `n` | Next result |
| `N` | Previous result |
| `g` | Go to beginning |
| `G` | Go to end |
| `q` | Quit |
| `h` | Help within man |

**man sections:**

```bash
# man pages are divided into 9 sections
man 1 passwd                               # passwd command (user commands)
man 5 passwd                               # /etc/passwd file format (file formats)

# Sections:
# 1 - User commands
# 2 - System calls
# 3 - Library functions
# 4 - Special files (/dev)
# 5 - File formats (configuration files)
# 6 - Games
# 7 - Miscellaneous
# 8 - System administration commands
# 9 - Kernel routines

# Search across all sections
man -a passwd                              # All passwd man pages
man -k password                            # Man pages related to password
man -f passwd                              # Brief info about passwd (whatis)
```

**man page structure:**

```
NAME         - Command name and brief description
SYNOPSIS     - Syntax (how to use it)
DESCRIPTION  - Detailed description
OPTIONS      - List of all options
EXAMPLES     - Usage examples
FILES        - Related files
SEE ALSO     - Related commands
BUGS         - Known bugs
AUTHOR       - Author
```

### 6.3 info pages

Some commands have more detailed documentation via `info`:

```bash
info coreutils                             # About GNU core utilities
info grep                                  # Detailed grep documentation
info find                                  # Detailed find documentation
info bash                                  # Full Bash documentation
```

**info navigation:**

| Key | Function |
|-----|----------|
| `n` | Next node |
| `p` | Previous node |
| `u` | Up one level |
| `Enter` | Open link |
| `l` | Return to last node |
| `Space` | Next page |
| `Del` | Previous page |
| `/` | Search |
| `q` | Quit |

### 6.4 Additional help methods

```bash
# whatis - brief description
whatis ls                                  # ls(1) - list directory contents
whatis grep                                # grep(1) - print lines matching a pattern

# apropos - search by keyword
apropos "copy file"                        # Commands for copying files
apropos permission                         # Related to permissions
apropos search                             # Search commands

# type - command type
type ls                                    # ls is aliased to 'ls --color=auto'
type cd                                    # cd is a shell builtin
type grep                                  # grep is /usr/bin/grep

# which - command location
which python                               # /usr/bin/python
which grep                                 # /usr/bin/grep

# whereis - command, man, and source location
whereis ls                                 # ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz

# command -v - check if command exists
command -v git && echo "Git is installed"
```

### 6.5 Help strategies

**1. Order for learning a new command:**

```bash
# Step 1: Brief help
command --help | head -30

# Step 2: Description with whatis
whatis command

# Step 3: Read the manual
man command

# Step 4: Search for examples
man command | grep -A5 "EXAMPLE"
```

**2. Useful for CTF:**

```bash
# View all flags in a command
grep --help | grep -E "^\s+-"

# Only short options
ls --help 2>&1 | grep -oE "\s-[a-zA-Z]" | sort -u

# Extract only OPTIONS from man
man grep | sed -n '/^OPTIONS/,/^[A-Z]/p'

# Information about specific flag
man find | grep -A3 "\-perm"
```

**3. Offline help:**

```bash
# Save man pages to file
man ls > ls_manual.txt
man grep | col -b > grep_manual.txt       # Without formatting

# Find all man files
man -w ls                                  # /usr/share/man/man1/ls.1.gz
zcat /usr/share/man/man1/ls.1.gz          # Read man file directly
```

> **Tip:** When you encounter an unfamiliar command in CTF, immediately explore it with `--help` and `man`. Often the solution is hidden in a special flag!

---

## 7. Text editors - nano and vim

> **Important for CTF:** Many servers only have terminal editors available. Knowing `nano` and `vim` is essential for CTF and working with real servers!

### 7.1 nano - simple editor

`nano` is the most user-friendly terminal editor for beginners.

#### 7.1.1 Launching nano

```bash
nano                                       # Open empty file
nano file.txt                              # Open existing/new file
nano +10 file.txt                          # Open at line 10
nano +10,5 file.txt                        # Line 10, column 5
nano -l file.txt                           # With line numbers
nano -m file.txt                           # Mouse support
nano -i file.txt                           # Auto-indent
nano -E file.txt                           # Tab -> spaces
nano -c file.txt                           # Show cursor position
nano -w file.txt                           # Don't wrap long lines
```

#### 7.1.2 nano basic commands

**In nano, `^` means Ctrl, `M-` means Alt**

| Combination | Function |
|-------------|----------|
| `^G` (Ctrl+G) | Display help |
| `^X` (Ctrl+X) | Exit (prompts to save) |
| `^O` (Ctrl+O) | Save (WriteOut) |
| `^R` (Ctrl+R) | Insert file (Read file) |
| `^W` (Ctrl+W) | Search (Where is) |
| `^\` (Ctrl+\) | Search and replace |
| `^K` (Ctrl+K) | Cut line |
| `^U` (Ctrl+U) | Paste (Uncut) |
| `^J` (Ctrl+J) | Justify paragraph |
| `^C` (Ctrl+C) | Cursor position |
| `^_` (Ctrl+_) | Go to line/column number |
| `^T` (Ctrl+T) | Spell checker / Execute |

**Navigation:**

| Combination | Function |
|-------------|----------|
| `^A` | Beginning of line |
| `^E` | End of line |
| `^Y` | Previous page (Page Up) |
| `^V` | Next page (Page Down) |
| `M-\` (Alt+\) | Beginning of file |
| `M-/` (Alt+/) | End of file |
| `^Space` | One word forward |
| `M-Space` | One word backward |
| `^Up Arrow` | Previous block |
| `^Down Arrow` | Next block |

**Editing:**

| Combination | Function |
|-------------|----------|
| `^K` | Cut line |
| `^U` | Paste cut content |
| `M-6` (Alt+6) | Copy line |
| `^6` (Ctrl+6) | Start marking (Mark) |
| `M-A` (Alt+A) | Start marking |
| `^D` | Delete character |
| `^H` | Backspace |
| `M-Del` | Delete word |
| `M-T` | Cut from cursor to end of line |
| `M-U` | Undo |
| `M-E` | Redo |

#### 7.1.3 nano search and replace

```bash
# Search (Ctrl+W)
^W -> type pattern -> Enter

# Search options (within Ctrl+W):
M-C    Toggle case sensitivity
M-R    Enable regex
M-B    Search backward
^R     Search in file
^W     Next result

# Replace (Ctrl+\)
^\ -> search term -> Enter -> replacement -> Enter
Y      Replace one
A      Replace all
N      Skip
^C     Cancel
```

#### 7.1.4 nano configuration

```bash
# ~/.nanorc or /etc/nanorc
set linenumbers                            # Line numbers
set autoindent                             # Auto-indent
set tabsize 4                              # Tab size
set tabstospaces                           # Tab -> spaces
set mouse                                  # Mouse support
set smooth                                 # Smooth scroll
set constantshow                           # Cursor position
set backup                                 # Create backup
set nohelp                                 # Hide bottom help bar

# Syntax highlighting
include "/usr/share/nano/*.nanorc"
include "/usr/share/nano/python.nanorc"
include "/usr/share/nano/sh.nanorc"
```

---

### 7.2 vim - powerful editor

`vim` (Vi IMproved) is a professional editor. It has a steep learning curve but is extremely powerful.

#### 7.2.1 vim modes

**vim operates in 4 main modes:**

```
+-------------------------------------------------------------+
|                      NORMAL MODE                            |
|                    (default mode)                           |
|                                                             |
|    +----------+    +----------+    +----------+             |
|    |    i     |    |    v     |    |    :     |             |
|    |    I     |    |    V     |    |          |             |
|    |    a     |    |   ^V     |    |          |             |
|    |    A     |    |          |    |          |             |
|    |    o     |    |          |    |          |             |
|    |    O     |    |          |    |          |             |
|    v          |    v          |    v          |             |
| +----------+  | +----------+  | +----------+  |             |
| |  INSERT  |  | |  VISUAL  |  | | COMMAND  |  |             |
| |   MODE   |  | |   MODE   |  | |   MODE   |  |             |
| +----------+  | +----------+  | +----------+  |             |
|    |          |    |          |    |          |             |
|    | Esc      |    | Esc      |    | Enter    |             |
|    +----------+----+----------+----+----------+             |
|               |               |                             |
|               +---------------+                             |
|                    ^                                        |
|                    |                                        |
|              Return to Normal                               |
+-------------------------------------------------------------+
```

| Mode | Enter | Exit | Function |
|------|-------|------|----------|
| **Normal** | Esc | - | Navigation, commands |
| **Insert** | i, I, a, A, o, O | Esc | Text entry |
| **Visual** | v, V, Ctrl+v | Esc | Text selection |
| **Command** | : | Enter/Esc | Ex commands |

#### 7.2.2 Launching vim

```bash
vim                                        # Empty file
vim file.txt                               # Open file
vim +10 file.txt                           # Go to line 10
vim +/pattern file.txt                     # Go to pattern location
vim -O file1 file2                         # Vertical split
vim -o file1 file2                         # Horizontal split
vim -p file1 file2                         # With tabs
vim -R file.txt                            # Read only
vim -d file1 file2                         # Diff mode
vim -c "set number" file.txt               # With command
vim +$ file.txt                            # End of file
```

#### 7.2.3 Normal mode commands

**Navigation:**

| Command | Function |
|---------|----------|
| `h` `j` `k` `l` | Left, Down, Up, Right (or arrow keys) |
| `w` | Beginning of next word |
| `W` | Beginning of next WORD (after space) |
| `b` | Beginning of previous word |
| `B` | Beginning of previous WORD |
| `e` | End of word |
| `E` | End of WORD |
| `0` | Beginning of line |
| `^` | First non-blank character |
| `$` | End of line |
| `gg` | Beginning of file |
| `G` | End of file |
| `10G` | Go to line 10 |
| `:10` | Go to line 10 |
| `{` | Previous paragraph |
| `}` | Next paragraph |
| `%` | Jump to matching bracket |
| `H` | Top of screen (High) |
| `M` | Middle of screen (Middle) |
| `L` | Bottom of screen (Low) |
| `Ctrl+f` | Page down (Forward) |
| `Ctrl+b` | Page up (Back) |
| `Ctrl+d` | Half page down |
| `Ctrl+u` | Half page up |
| `zz` | Center current line |
| `zt` | Current line to top |
| `zb` | Current line to bottom |

**Editing (in Normal mode):**

| Command | Function |
|---------|----------|
| `x` | Delete character |
| `X` | Delete previous character |
| `dd` | Delete line |
| `dw` | Delete word |
| `d$` or `D` | Delete to end of line |
| `d0` | Delete to beginning of line |
| `dG` | Delete to end of file |
| `dgg` | Delete to beginning of file |
| `yy` | Copy line (yank) |
| `yw` | Copy word |
| `y$` | Copy to end of line |
| `p` | Paste after cursor |
| `P` | Paste before cursor |
| `r` | Replace single character |
| `R` | Replace mode (overwrite) |
| `cc` | Delete line and enter insert mode |
| `cw` | Delete word and enter insert mode |
| `c$` or `C` | Delete to end of line and insert |
| `~` | Toggle case (a<->A) |
| `u` | Undo |
| `Ctrl+r` | Redo |
| `.` | Repeat last command |
| `J` | Join lines |
| `>>` | Indent (shift right) |
| `<<` | Dedent (shift left) |

**Entering Insert mode:**

| Command | Function |
|---------|----------|
| `i` | Insert before cursor |
| `I` | Insert at beginning of line |
| `a` | Insert after cursor (append) |
| `A` | Insert at end of line |
| `o` | Open new line below and insert |
| `O` | Open new line above and insert |
| `s` | Delete character and insert |
| `S` | Delete line and insert |

#### 7.2.4 Visual mode

```bash
v                                          # Character-wise visual
V                                          # Line-wise visual
Ctrl+v                                     # Block visual (columns)

# In Visual mode:
d                                          # Delete selection
y                                          # Copy selection
c                                          # Delete selection and insert
>                                          # Indent
<                                          # Dedent
~                                          # Toggle case
u                                          # Lowercase
U                                          # Uppercase
```

#### 7.2.5 Command mode (:)

**Save and exit:**

```bash
:w                                         # Save
:w filename                                # Save as
:w!                                        # Force save
:q                                         # Quit
:q!                                        # Quit without saving
:wq                                        # Save and quit
:wq!                                       # Force save and quit
:x                                         # Same as :wq
ZZ                                         # Same as :wq (in Normal mode)
ZQ                                         # Same as :q! (in Normal mode)
:e filename                                # Open another file
:e!                                        # Reload file
```

**Search and replace:**

```bash
/pattern                                   # Search forward
?pattern                                   # Search backward
n                                          # Next result
N                                          # Previous result
*                                          # Search word under cursor forward
#                                          # Search word under cursor backward

# Replace
:s/old/new/                                # First occurrence in current line
:s/old/new/g                               # All in current line
:%s/old/new/g                              # All in entire file
:%s/old/new/gc                             # With confirmation
:5,10s/old/new/g                           # Lines 5-10
:'<,'>s/old/new/g                          # Visual selection

# Flags:
# g - global (all occurrences)
# c - confirm (ask for confirmation)
# i - case insensitive
# I - case sensitive
```

**Working with files:**

```bash
:split                                     # Horizontal split
:vsplit                                    # Vertical split
:sp filename                               # Split with file
:vsp filename                              # Vertical split with file
Ctrl+w w                                   # Switch between windows
Ctrl+w h/j/k/l                             # Navigate windows by direction
Ctrl+w c                                   # Close window
Ctrl+w o                                   # Close other windows
:tabnew filename                           # New tab
gt                                         # Next tab
gT                                         # Previous tab
:tabc                                      # Close tab
```

**Settings:**

```bash
:set number                                # Line numbers
:set nonumber                              # Disable line numbers
:set nu!                                   # Toggle
:set relativenumber                        # Relative numbers
:set hlsearch                              # Search highlighting
:set nohlsearch                            # Disable highlighting
:noh                                       # Clear current highlighting
:set ignorecase                            # Case insensitive
:set smartcase                             # Smart case
:set autoindent                            # Auto indent
:set tabstop=4                             # Tab size
:set expandtab                             # Tab -> spaces
:set syntax=python                         # Syntax
:syntax on                                 # Syntax highlighting
:set paste                                 # Paste mode (preserve formatting)
:set nopaste                               # Disable paste mode
```

#### 7.2.6 vim powerful combinations

**Operator + Motion:**

```bash
d + motion                                 # Delete
y + motion                                 # Yank (copy)
c + motion                                 # Change (delete + insert)
> + motion                                 # Indent
< + motion                                 # Dedent
gU + motion                                # Uppercase
gu + motion                                # Lowercase

# Examples:
d2w                                        # Delete 2 words
y3j                                        # Copy 3 lines
c$                                         # Change to end of line
>3j                                        # Indent 3 lines
gUw                                        # Uppercase word
```

**Text objects:**

```bash
# i = inner (inside), a = around (including surroundings)
diw                                        # Delete inside word
daw                                        # Delete word + space
di"                                        # Delete inside "..."
da"                                        # Delete "..." including quotes
di(                                        # Delete inside (...)
da(                                        # Delete (...) including parens
di{                                        # Delete inside {...}
di[                                        # Delete inside [...]
dit                                        # Delete inside HTML tag
dat                                        # Delete HTML tag
dip                                        # Delete inside paragraph
dap                                        # Delete paragraph

# Works with y, c as well:
yiw                                        # Copy word
ci"                                        # Change inside "..." and insert
ca{                                        # Change {...} and insert
```

**Macros:**

```bash
qa                                         # Start recording macro in register 'a'
# ... commands ...
q                                          # Stop recording
@a                                         # Execute macro 'a'
@@                                         # Repeat last macro
10@a                                       # Execute 'a' macro 10 times
```

#### 7.2.7 vim configuration (~/.vimrc)

```vim
" Basic settings
set number                                 " Line numbers
set relativenumber                         " Relative numbers
set cursorline                             " Current line highlighting
set showmatch                              " Show matching bracket
set hlsearch                               " Search highlighting
set incsearch                              " Incremental search
set ignorecase                             " Case insensitive
set smartcase                              " Smart case

" Indentation
set tabstop=4                              " Tab = 4 spaces
set shiftwidth=4                           " Indent = 4 spaces
set expandtab                              " Tab -> spaces
set autoindent                             " Auto indent
set smartindent                            " Smart indent

" UI
syntax on                                  " Syntax highlighting
set background=dark                        " Dark theme
set wildmenu                               " Command autocomplete
set laststatus=2                           " Status bar
set ruler                                  " Cursor position
set scrolloff=5                            " Scroll offset

" Key mappings
let mapleader = ","                        " Leader key
nnoremap <leader>w :w<CR>                  " ,w = save
nnoremap <leader>q :q<CR>                  " ,q = quit
nnoremap <C-s> :w<CR>                      " Ctrl+s = save
inoremap jj <Esc>                          " jj = Escape
```

#### 7.2.8 Comparison: vim vs nano

| Feature | nano | vim |
|---------|------|-----|
| Learning curve | Easy | Steep |
| Power | Limited | Very powerful |
| Speed | Moderate | Very fast |
| Availability | Almost everywhere | Everywhere |
| Mouse support | Yes | Limited |
| Macros | No | Yes |
| Plugins | No | Many |
| Large files | Slow | Fast |

> **Tip:** Use `nano` for beginners, `vim` for professionals. For CTF, knowing both is essential!

---

## 8. Permissions system

### 8.1 Permissions matrix

In Linux, each file has three permission types for three categories:

```
         Owner    Group    Others
         -----    -----    ------
Read     r (4)    r (4)    r (4)
Write    w (2)    w (2)    w (2)
Execute  x (1)    x (1)    x (1)
```

### 8.2 Reading permissions

```bash
ls -la file.txt
# -rw-r--r-- 1 student group 1234 Jan 10 10:00 file.txt
#  │││ │││ │││
#  │││ │││ └┴┴── Others: r-- (read only)
#  │││ └┴┴────── Group: r-- (read only)
#  └┴┴────────── Owner: rw- (read and write)
```

### 8.3 Binary number system — the foundation of chmod

> 💡 **Why is this important?** Linux permissions are based on the binary number system. Without understanding this, memorizing chmod codes becomes difficult and illogical.

#### 8.3.1 What is the binary number system?

In everyday life, we use the **decimal** number system — it consists of 10 digits: `0, 1, 2, 3, 4, 5, 6, 7, 8, 9`.

The **binary** number system consists of only **2 digits**: `0` and `1`.

Computers work in binary because electrical signals have only two states:
- ⚡ **Current ON** = `1` (ON, true, yes)
- 💤 **Current OFF** = `0` (OFF, false, no)

#### 8.3.2 Converting from decimal to binary

**Position values (from right to left):**

```
Position:     7      6      5      4      3      2      1      0
            ─────  ─────  ─────  ─────  ─────  ─────  ─────  ─────
Power of    2⁷     2⁶     2⁵     2⁴     2³     2²     2¹     2⁰
two:        128    64     32     16     8      4      2      1
```

**Numbers 0 to 10 in binary representation:**

| Decimal | Binary | Calculation | Explanation |
|---------|--------|-------------|-------------|
| **0** | `0000` | 0 | Nothing |
| **1** | `0001` | 2⁰ = 1 | Only position 1 |
| **2** | `0010` | 2¹ = 2 | Only position 2 |
| **3** | `0011` | 2¹ + 2⁰ = 2+1 | Positions 2 and 1 |
| **4** | `0100` | 2² = 4 | Only position 4 |
| **5** | `0101` | 2² + 2⁰ = 4+1 | Positions 4 and 1 |
| **6** | `0110` | 2² + 2¹ = 4+2 | Positions 4 and 2 |
| **7** | `0111` | 2² + 2¹ + 2⁰ = 4+2+1 | Positions 4, 2, and 1 |
| **8** | `1000` | 2³ = 8 | Only position 8 |
| **9** | `1001` | 2³ + 2⁰ = 8+1 | Positions 8 and 1 |
| **10** | `1010` | 2³ + 2¹ = 8+2 | Positions 8 and 2 |

> 🎯 **Remember:** Each position doubles: 1 → 2 → 4 → 8 → 16 → 32...

#### 8.3.3 Converting from binary to decimal

**Algorithm:** Add the values of all positions where there is a `1`.

**Example:** Converting `1011` to decimal

```
Position:      3      2      1      0
              ───    ───    ───    ───
Value:         8      4      2      1
              ───    ───    ───    ───
Digit:         1      0      1      1
              ───    ───    ───    ───
Calculation:  8×1  + 4×0  + 2×1  + 1×1  = 8 + 0 + 2 + 1 = 11
```

**Answer:** `1011₂` = `11₁₀`

#### 8.3.4 The chmod secret — where do 4, 2, 1 come from?

Linux permissions are based on a **3-bit** binary system:

```
Bit position:       2      1      0
                   ───    ───    ───
Binary value:      2²     2¹     2⁰
                   ───    ───    ───
Decimal value:      4      2      1
                   ───    ───    ───
Permission type:    r      w      x
                  (read) (write) (execute)
```

**This means:**
- `r` (read) = bit 2 = **4**
- `w` (write) = bit 1 = **2**
- `x` (execute) = bit 0 = **1**

#### 8.3.5 Reading permissions using binary

How to read `ls -l` output:

```
Where there's a letter put 1, where there's a dash put 0:

-rwxrwxrwx  →  Convert to binary
 │││ │││ │││
 111 111 111  →  Convert each group to decimal
  7   7   7   →  chmod 777
```

**Detailed examples:**

| Permission | U (Owner) | G (Group) | O (Others) | chmod |
|------------|-----------|-----------|------------|-------|
| `rwxrwxrwx` | 111 = 7 | 111 = 7 | 111 = 7 | **777** |
| `rwxr-xr-x` | 111 = 7 | 101 = 5 | 101 = 5 | **755** |
| `rw-r--r--` | 110 = 6 | 100 = 4 | 100 = 4 | **644** |
| `rw-------` | 110 = 6 | 000 = 0 | 000 = 0 | **600** |
| `rwx------` | 111 = 7 | 000 = 0 | 000 = 0 | **700** |
| `r--------` | 100 = 4 | 000 = 0 | 000 = 0 | **400** |

#### 8.3.6 Complete chmod codes table

| Binary | Decimal | Permission | Description |
|--------|---------|------------|-------------|
| `000` | 0 | `---` | No permissions |
| `001` | 1 | `--x` | Execute only |
| `010` | 2 | `-w-` | Write only |
| `011` | 3 | `-wx` | Write and execute |
| `100` | 4 | `r--` | Read only |
| `101` | 5 | `r-x` | Read and execute |
| `110` | 6 | `rw-` | Read and write |
| `111` | 7 | `rwx` | All permissions |

#### 8.3.7 Practical examples

```bash
# Example 1: chmod 755 script.sh
# Owner: rwx (111 = 7), Group: r-x (101 = 5), Others: r-x (101 = 5)
# Result: -rwxr-xr-x

# Example 2: chmod 644 document.txt
# Owner: rw- (110 = 6), Group: r-- (100 = 4), Others: r-- (100 = 4)
# Result: -rw-r--r--

# Example 3: chmod 700 private_dir
# Owner: rwx (111 = 7), Group: --- (000 = 0), Others: --- (000 = 0)
# Result: drwx------

# Example 4: chmod 600 secret.key
# Owner: rw- (110 = 6), Group: --- (000 = 0), Others: --- (000 = 0)
# Result: -rw-------
```

> 🔐 **Security tip:** Never use `777`! It gives full permissions to everyone and is dangerous!

---

### 8.4 Numeric (octal) permissions — summary

Each permission corresponds to a binary position:
- `r` = 4 (in binary: `100`)
- `w` = 2 (in binary: `010`)
- `x` = 1 (in binary: `001`)

They are added together:

| Permission | Calculation | Value |
|------------|-------------|-------|
| `rwx` | 4+2+1 | 7 |
| `rw-` | 4+2+0 | 6 |
| `r-x` | 4+0+1 | 5 |
| `r--` | 4+0+0 | 4 |
| `-wx` | 0+2+1 | 3 |
| `-w-` | 0+2+0 | 2 |
| `--x` | 0+0+1 | 1 |
| `---` | 0+0+0 | 0 |

**Common combinations:**

| Number | Permission | Usage |
|--------|------------|-------|
| 777 | rwxrwxrwx | Everything for everyone (dangerous!) |
| 755 | rwxr-xr-x | Programs, scripts |
| 700 | rwx------ | Private directory |
| 644 | rw-r--r-- | Regular files |
| 600 | rw------- | Secret files |
| 400 | r-------- | Read only |

### 8.5 chmod - changing permissions

**Numeric method:**
```bash
chmod 755 script.sh                        # rwxr-xr-x
chmod 644 file.txt                         # rw-r--r--
chmod 600 secret.key                       # rw-------
```

**Symbolic method:**
```bash
chmod u+x script.sh                        # Add execute for owner
chmod g-w file.txt                         # Remove write from group
chmod o=r file.txt                         # Others read only
chmod a+r file.txt                         # Add read for all
```

### 8.6 SUID, SGID, sticky bit

Special permissions:

#### SUID (set user ID) - 4xxx

File with SUID executes with owner's permissions:

```bash
ls -la /usr/bin/passwd
# -rwsr-xr-x 1 root root ... /usr/bin/passwd
#    ^
#    SUID bit (letter s instead of x)
```

**Finding SUID files:**
```bash
find / -perm -4000 2>/dev/null
```

#### SGID (set group ID) - 2xxx

Execution with group permissions:

```bash
find / -perm -2000 2>/dev/null
```

#### Sticky bit - 1xxx

In directory, only file owner can delete their file:

```bash
ls -la /tmp
# drwxrwxrwt 10 root root ... /tmp
#          ^
#          Sticky bit (letter t)
```

---

## 9. Important directories and their functions

### 9.1 /etc - configuration files

```bash
/etc/passwd                                # User list
/etc/shadow                                # Password hashes (root needed)
/etc/group                                 # Groups
/etc/sudoers                               # sudo permissions
/etc/hosts                                 # IP-hostname mappings
/etc/crontab                               # Scheduled tasks
```

### 9.2 /home - user directories

```bash
/home/username/                            # User home directory
/home/username/.bashrc                     # Bash configuration
/home/username/.bash_history               # Command history
/home/username/.ssh/                       # SSH keys
```

> **CTF tip:** `.bash_history` may contain important information!

### 9.3 /var - variable data

```bash
/var/log/                                  # Log files
/var/log/syslog                            # System logs
/var/log/auth.log                          # Authentication logs
/var/www/html/                             # Web server files
/var/backups/                              # Backups
```

### 9.4 /tmp - temporary files

Directory where all users can write:

```bash
ls -la /tmp/
ls -la /tmp/.*                             # Hidden files
```

### 9.5 /proc - virtual file system

Process information:

```bash
/proc/[PID]/                               # Directory for each process
/proc/[PID]/cmdline                        # Process command
/proc/[PID]/environ                        # Environment variables
/proc/version                              # Kernel version
```

**Important for CTF:**
```bash
strings /proc/*/environ 2>/dev/null | grep -i flag
strings /proc/*/environ 2>/dev/null | grep -i password
```

---

## 10. Working with archives

### 10.1 tar - tape archive

**Creating archive:**
```bash
tar -cvf archive.tar files/                # Regular archive
tar -czvf archive.tar.gz files/            # Compress with Gzip
tar -cjvf archive.tar.bz2 files/           # Compress with Bzip2
tar -cJvf archive.tar.xz files/            # Compress with XZ
```

**Extracting archive:**
```bash
tar -xvf archive.tar                       # Regular extraction
tar -xzvf archive.tar.gz                   # Extract Gzip
tar -xjvf archive.tar.bz2                  # Extract Bzip2
tar -xJvf archive.tar.xz                   # Extract XZ
tar -xvf archive.tar -C /destination/      # To specific location
```

**Viewing contents:**
```bash
tar -tvf archive.tar                       # Show list
```

### 10.2 zip / unzip

```bash
zip archive.zip file1 file2                # Create Zip
zip -r archive.zip directory/              # Recursive
unzip archive.zip                          # Extract
unzip -l archive.zip                       # Show list
```

### 10.3 gzip / gunzip

```bash
gzip file.txt                              # Creates file.txt.gz
gunzip file.txt.gz                         # Extracts
zcat file.txt.gz                           # Read without extracting
```

---

## 11. Disk and mount operations

### 11.1 df - disk free

```bash
df                                         # Free space
df -h                                      # Human-readable
df -T                                      # Filesystem type
```

### 11.2 du - disk usage

```bash
du file.txt                                # File size
du -h file.txt                             # Human-readable
du -sh directory/                          # Total directory size
du -sh *                                   # Each element
du -ah . | sort -rh | head -20             # Top 20 largest
```

### 11.3 lsblk - block devices

```bash
lsblk                                      # Block devices
lsblk -f                                   # Filesystem info
```

### 11.4 mount

```bash
mount                                      # All mounts
mount /dev/sdb1 /mnt/usb                   # Mount USB
umount /mnt/usb                            # Unmount
```

---

## 12. Symbolic links

### 12.1 Hard link vs symbolic link

| Property | Hard Link | Symbolic Link |
|----------|-----------|---------------|
| Inode | Same | New |
| File deleted | Works | Breaks |
| To directory | Not allowed | Allowed |
| Different filesystem | Not allowed | Allowed |

### 12.2 Creating symlink

```bash
ln -s /path/to/target linkname             # Create symlink
ln -s /etc/passwd ~/passwd_link            # Example
```

### 12.3 Viewing symlinks

```bash
ls -la                                     # Starts with l
readlink linkname                          # Show target
readlink -f linkname                       # Full path
```

### 12.4 Finding symlinks

```bash
find / -type l 2>/dev/null                 # All symlinks
find / -xtype l 2>/dev/null                # Broken symlinks
```

---

## 13. Practical exercises

### Exercise 1: Basic navigation

**Task:** Perform the following operations:

1. Determine your current directory
2. Go to your home directory
3. Go to `/etc` and find the `passwd` file
4. Return to previous directory

**Solution:**
```bash
pwd
cd ~
cd /etc && ls -la | grep passwd
cd -
```

### Exercise 2: File search

**Task:** Find the following files:

1. All `.txt` files in the system
2. Hidden files in `/home`
3. Files larger than 1MB in `/var`

**Solution:**
```bash
find / -name "*.txt" 2>/dev/null
find /home -name ".*" -type f 2>/dev/null
find /var -size +1M -type f 2>/dev/null
```

### Exercise 3: Permission analysis

**Task:** Explain the following permissions:

1. `-rw-r--r--` → 644 - Owner: read+write, Group/Others: read only
2. `-rwxr-xr-x` → 755 - Owner: all, Group/Others: read+execute
3. `drwxr-x---` → 750 - Directory, Owner: all, Group: read+execute, Others: no access
4. `-rwsr-xr-x` → 4755 - SUID set

---

## 14. CTF strategies

### 14.1 First steps (60 seconds)

First actions in every CTF:

```bash
# 1. Who am I and where?
id && pwd && whoami

# 2. What's around?
ls -la
tree -L 2 2>/dev/null

# 3. Quick flag search
find / -name "*flag*" 2>/dev/null
cat /home/*/flag* /root/flag* 2>/dev/null
```

### 14.2 Deeper investigation (2-5 minutes)

```bash
# Check Bash history
cat /home/*/.bash_history 2>/dev/null

# Check SUID files
find / -perm -4000 2>/dev/null

# Check backups
ls -la /var/backups/

# Check environment variables
cat /proc/*/environ 2>/dev/null | tr '\0' '\n' | grep -iE "(flag|pass|secret)"

# World-writable files
find / -writable -type f 2>/dev/null | head -20
```

### 14.3 Flag format regex

```bash
# Search for various flag formats
grep -rE "FLAG\{[^}]+\}" / 2>/dev/null
grep -rE "CTF\{[^}]+\}" / 2>/dev/null
grep -rE "HTB\{[^}]+\}" / 2>/dev/null
grep -rE "flag\{[^}]+\}" / 2>/dev/null

# General pattern
grep -roE "[A-Z]{2,10}\{[a-zA-Z0-9_-]+\}" / 2>/dev/null
```

### 14.4 Checking binary files

```bash
# Search for flag with strings
strings suspicious_file | grep -iE "flag|password|secret"

# From executable files
find / -type f -executable 2>/dev/null | xargs strings 2>/dev/null | grep FLAG

# Determine file type
file mystery_file
```

### 14.5 Most important one-liners

```bash
# Ultimate flag hunter
grep -rE "(FLAG|CTF|HTB|flag)\{[^}]+\}" / 2>/dev/null

# SUID files for privilege escalation
find / -perm -4000 -type f -exec ls -la {} \; 2>/dev/null

# Files you don't own but can read
find / ! -user $(whoami) -readable -type f 2>/dev/null | head -20

# Files modified in last 30 minutes
find / -mmin -30 -type f 2>/dev/null

# Search for password
grep -rn "password" /home /etc /opt /var 2>/dev/null | head -20
```

### 14.6 Avoiding mistakes

**Beginner mistakes:**

1. Not using `2>/dev/null` - output is filled with error messages
2. Using `ls` instead of `ls -la` - hidden files not visible
3. Forgetting `strings` command - flags in binary files not found
4. Only checking `/home` - `/opt`, `/var`, `/tmp` are also important
5. Ignoring `.bash_history`

---

## Conclusion

This guide covers the fundamental concepts of the Linux file system, commands, and CTF competition preparation strategies. The most important advice is **practice**. Practice daily on virtual machines and CTF platforms.

**Remember:**
- Always use `ls -la`
- `2>/dev/null` cleans the output
- `strings` is essential for binary files
- `.bash_history` may contain valuable information
- SUID files are important for privilege escalation

Good luck!

---

**Additional Resources:**

- [Linux Journey](https://linuxjourney.com/)
- [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/)
- [TryHackMe](https://tryhackme.com/)
- [HackTheBox](https://www.hackthebox.com/)
- [picoCTF](https://picoctf.org/)

---

*This guide was prepared by Nodir Ustoz. For questions and suggestions, please contact the author.*
