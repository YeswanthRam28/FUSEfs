# FUSE Filesystem Project

## Overview
This project demonstrates a simple **FUSE (Filesystem in Userspace)** filesystem implemented in C.  
It supports both **static files** and an **in-memory notes system**.

## Features
1. **Static files**
   - `/hello` → `"Hello FUSE World!"`
   - `/info.txt` → `"This is our FUSE project."`
   - `/time.txt` → dynamically shows current system time

2. **In-memory notes**
   - Directory: `/notes`
   - Supports creating, reading, writing, and listing note files entirely in memory.
   - Maximum of 100 notes with up to 1024 bytes each.

3. **Filesystem operations supported**
   - `getattr` → file attributes
   - `readdir` → list files in directories
   - `open` → check file access
   - `read` → read file contents
   - `write` → write to in-memory notes
   - `create` → create new in-memory notes

## Requirements
- Linux / WSL2 environment
- `fuse3` library
- `gcc` compiler
- `pkg-config`  

Install dependencies on Ubuntu / WSL:

```bash
sudo apt update
sudo apt install libfuse3-dev pkg-config build-essential -y
````

## Compilation

```bash
cd /path/to/FUSEfs
gcc myfs.c -o myfs `pkg-config fuse3 --cflags --libs`
```

## Running the filesystem

1. Create a mount point:

```bash
mkdir -p /tmp/myfs_mount
```

2. Mount the filesystem:

```bash
./myfs /tmp/myfs_mount &
```

3. Access files:

```bash
cd /tmp/myfs_mount
ls
cat hello
cat info.txt
cat time.txt
```

4. Notes operations:

```bash
mkdir -p notes  # optional, FUSE handles /notes
echo "My note content" > notes/note1.txt
cat notes/note1.txt
```

5. Unmount when done:

```bash
fusermount3 -u /tmp/myfs_mount
```

## Testing

A test script `test_myfs.sh` is provided to automatically verify:

* Mounting and unmounting
* Reading static files
* Creating, reading, and appending notes
* Listing `/notes` directory contents

Run the script:

```bash
./test_myfs.sh
```

## Notes

* All `/notes` files are **in-memory only** and are **lost on unmount**.
* Maximum 100 notes, 1024 bytes per note.
* Static files are **read-only**.
