# Linux Fundamentals

This repository contains a set of notes covering the fundamental concepts and commands in Linux, designed for beginners to learn and understand the basics of Linux operating systems.

---

## Table of Contents

1. [Introduction to Linux](#introduction-to-linux)
2. [Linux File System](#linux-file-system)
3. [Basic Linux Commands](#basic-linux-commands)
4. [File Permissions](#file-permissions)
5. [Text Processing Commands](#text-processing-commands)
6. [Process Management](#process-management)
7. [Package Management](#package-management)
8. [Networking Commands](#networking-commands)
9. [Shell Scripting Basics](#shell-scripting-basics)
10. [Conclusion](#conclusion)

---

## Introduction to Linux

Linux is an open-source, Unix-like operating system based on the Linux kernel. It is widely used in servers, desktops, mobile devices, and embedded systems.

### Key Concepts:
- **Kernel**: The core part of the operating system that manages system resources.
- **Distribution (Distro)**: A complete operating system built on top of the Linux kernel (e.g., Ubuntu, Fedora, CentOS).
  
### Linux vs. Windows:
- Open-source
- Command-line interface (CLI) focused (though graphical user interfaces like GNOME and KDE are available)
- Stronger security and performance in certain environments

---

## Linux File System

### Directory Structure:
- `/`: Root directory, the top-most directory in the file system.
- `/bin`: Essential command binaries (e.g., ls, cp).
- `/etc`: System configuration files.
- `/home`: User home directories.
- `/var`: Variable files such as logs.
- `/tmp`: Temporary files.

### Commands:

| **Command**  | **Description**                                   |
|--------------|---------------------------------------------------|
| `pwd`        | Prints the current working directory.             |
| `cd <dir>`   | Shortcut to get to your home directory            |
| `cd~`        | Lists the contents of the current directory.      |
| `ls`         | Lists the contents of the current directory.      |
| `ls -a`      | Lists the contents of the current directory including hidden files. |


---

## Basic Linux Commands

### File Manipulation:

| **Command**                | **Description**                                            |
|--------------------------- |------------------------------------------------------------|
| `touch <filename>`         | Creates an empty file or updates the timestamp of an existing file. |
| `cp <source> <destination>`| Copies files or directories from source to destination.    |
| `mv <source> <destination>`| Moves or renames files or directories.                     |
| `rm <file>`                | Removes a file.                                            |
| `rmdir <directory>`        | Removes an empty directory.                                |

### Viewing File Content:

| **Command**               | **Description**                                             |
|---------------------------|-------------------------------------------------------------|
| `cat <file>`              | Displays the content of a file.                             |
| `less <file>`             | Views a file one page at a time.                            |
| `head <file>`             | Displays the first 10 lines of a file.                      |
| `tail <file>`             | Displays the last 10 lines of a file.                       |

---

## File Permissions

### Understanding Permissions:
- **Owner, Group, Others**: Defines who has access to a file and what actions they can perform.
- Permissions: `r` (read), `w` (write), `x` (execute)

## Permission Groups

Permissions are divided into **three groups**, in this order:

| Position | Group  | Applies To |
|--------|--------|------------|
| 1st    | Owner  | File owner |
| 2nd    | Group  | File group |
| 3rd    | Others | Everyone else |

### Commands:

| **Command**                       | **Description**                                       |
|-----------------------------------|-------------------------------------------------------|
| `chmod <permissions> <file>`      | Changes the permissions of a file (read, write, execute) |
| `chown <user>:<group> <file>`     | Changes the owner and/or group of a file.             |

## Common Permission Numbers

| Number | Permissions | Meaning |
|-------|------------|--------|
| 0     | `---`       | No permissions |
| 1     | `--x`       | Execute only |
| 2     | `-w-`       | Write only |
| 3     | `-wx`       | Write + Execute |
| 4     | `r--`       | Read only |
| 5     | `r-x`       | Read + Execute |
| 6     | `rw-`       | Read + Write |
| 7     | `rwx`       | Read + Write + Execute |

---

## Text Processing Commands

### Cutting and Joining Data:

| **Command**                              | **Description**                               |
|------------------------------------------|-----------------------------------------------|
| `cut -d<delimiter> -f<fields> <file>`    | Cuts specific columns or fields from a file.  |
| `paste <file1> <file2>`                  | Merges lines of two files side-by-side.       |

### Sorting and Filtering:

| **Command**                              | **Description**                                                 |
|------------------------------------------|-----------------------------------------------------------------|
| `sort <file>`                            | Sorts the content of a file.                                    |
| `uniq <file>`                            | Removes duplicate lines from a file.                            |

### Regular Expressions:

| **Command**                              | **Description**                                                 |
|------------------------------------------|-----------------------------------------------------------------|
| `grep -E <pattern> <file>`               | Uses extended regular expressions to search inside a file.      |

---

## Process Management

### Checking Running Processes:

| **Command**               | **Description**                                        |
|---------------------------|--------------------------------------------------------|
| `ps`                       | Displays currently running processes.                 |
| `top`                      | Displays real-time process and system resource usage. |
| `htop`                     | Interactive process viewer (install separately).      |

### Managing Processes:

| **Command**               | **Description**                                        |
|---------------------------|--------------------------------------------------------|
| `kill <pid>`              | Terminates a process by its PID (Process ID).          |
| `killall <process_name>`  | Terminates all instances of a given process by name.   |
| `nohup <command>`         | Runs a command in the background, ignoring hangups.    |

### Managing Background Jobs:

| **Command**               | **Description**                                        |
|---------------------------|--------------------------------------------------------|
| `&`                        | Runs a command in the background.                     |
| `fg`                       | Brings a background job to the foreground.            |
| `bg`                       | Resumes a job in the background.                      |

---

## Package Management

### Package Managers:

| **Command**               | **Description**                                            |
|---------------------------|------------------------------------------------------------|
| `apt update`              | Updates the list of available packages (Debian-based systems).|
| `apt upgrade`             | Upgrades installed packages (Debian-based systems).        |
| `apt install <package>`   | Installs a package (Debian-based systems).                 |
| `apt remove <package>`    | Removes a package (Debian-based systems).                  |
| `yum update`              | Updates packages (Red Hat-based systems).                  |
| `yum install <package>`   | Installs a package (Red Hat-based systems).                |
| `yum remove <package>`    | Removes a package (Red Hat-based systems).                 |

---

## Networking Commands

### Basic Networking Commands:

| **Command**               | **Description**                                         |
|---------------------------|---------------------------------------------------------|
| `ifconfig`                | Displays network interface configuration.               |
| `ping <host>`             | Checks network connectivity to a remote host.           |
| `traceroute <host>`       | Traces the route packets take to reach a host.          |

### Managing Network Services:

| **Command**               | **Description**                                         |
|---------------------------|---------------------------------------------------------|
| `netstat`                 | Displays network connections, routing tables, interface stats, etc. |
| `ss`                      | Displays socket statistics and network connections.     |

### Configuring Network Interfaces:

| **Command**               | **Description**                                         |
|---------------------------|---------------------------------------------------------|
| `ip addr`                 | Displays the IP address configuration of all network interfaces. |
| `ip link set <interface> up/down` | Brings a network interface up or down.          |

---

## Conclusion

These notes provide a foundation for learning Linux. As you become more familiar with the basics, you can dive into advanced topics such as system administration, networking, security, and scripting.