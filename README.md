Sure! Here’s the same **README.md** content in a code block for easy copy-pasting:

````markdown
# FUSE Filesystem Project

## Overview

This project implements a **custom in-memory FUSE (Filesystem in Userspace)** filesystem using **FUSE3** in C. It demonstrates how to create a virtual filesystem that:

- Contains static read-only files (`hello`, `info.txt`, `time.txt`)
- Provides dynamic content (`time.txt` updates with current system time)
- Supports **in-memory writable notes** under the `/notes` directory
- Supports reading, writing, and creating new files in `/notes` dynamically

This project is suitable as an **Operating Systems coursework project** to explore FUSE concepts.

---

## Features

### 1. Static Files
| File        | Description                  |
|------------|-------------------------------|
| `/hello`    | Prints `Hello FUSE World!`   |
| `/info.txt` | Prints project information    |
| `/time.txt` | Shows current system time     |

### 2. Notes Directory
- `/notes` is a virtual directory where users can:
  - Create new notes
  - Write content to notes
  - Read notes
- All notes are stored **in-memory**; they are lost when the filesystem is unmounted.

---

## Installation

### Prerequisites

- Ubuntu / WSL with FUSE3 installed:
```bash
sudo apt update
sudo apt install libfuse3-dev pkg-config build-essential -y
````

* Navigate to the project folder:

```bash
cd /mnt/d/Projects/Fusefilesys/FUSEfs
```

---

## Usage

### 1. Compile the filesystem

```bash
gcc myfs.c -o myfs `pkg-config fuse3 --cflags --libs`
```

### 2. Create a mount point

```bash
mkdir -p /tmp/myfs_mount
```

### 3. Run the filesystem

```bash
./myfs /tmp/myfs_mount
```

### 4. Access files

```bash
cd /tmp/myfs_mount
ls
cat hello
cat info.txt
cat time.txt
```

### 5. Create and edit notes

```bash
echo "My first note" > notes/note1.txt
cat notes/note1.txt

echo "Appending to note1" >> notes/note1.txt
cat notes/note1.txt
```

### 6. Unmount filesystem

```bash
fusermount3 -u /tmp/myfs_mount
```

---

## Testing

A **bash script** `test_myfs.sh` is provided to automatically test:

* Mounting the filesystem
* Reading static files
* Creating, writing, and reading notes
* Dynamic time output
* Automatic unmounting

Run:

```bash
chmod +x test_myfs.sh
./test_myfs.sh
```

---

## Project Structure

```
FUSEfs/
├── myfs.c         # Main FUSE filesystem code
├── myfs           # Compiled executable
├── README.md      # This file
└── test_myfs.sh   # Automated testing script
```

---

## Improvements

Future improvements could include:

* Persistent storage for `/notes` (saving to disk)
* Support for deleting files in `/notes`
* Directory creation inside `/notes`
* File permissions and user access control
* Extended dynamic files beyond `/time.txt`

---
