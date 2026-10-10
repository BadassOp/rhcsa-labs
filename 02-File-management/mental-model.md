# File Management — Mental Model

## 1. Linux Filesystem Hierarchy

Linux organizes files and directories into a single hierarchy beginning at the root directory `/`.

```text
/
├── home/       User home directories
├── etc/        System configuration
├── var/        Variable data and logs
├── tmp/        Temporary files
├── usr/        Applications and shared resources
├── root/       Root user's home directory
├── dev/        Device files
├── proc/       Process and kernel information
└── boot/       Boot-related files
```

## 2. Absolute vs Relative Paths

**Absolute path:** Starts from `/` and identifies a location independently of the current directory.

Example: `/home/student/notes.txt`

**Relative path:** Specifies a location relative to the current working directory.

Example: `documents/notes.txt`

Special directory references:

- `.` — Current directory.
- `..` — Parent directory.
- `~` — Current user's home directory.

## 3. File Operation Lifecycle

```text
Create
  ↓
View / Inspect
  ↓
Copy / Move / Rename
  ↓
Search / Organize
  ↓
Delete (when no longer needed)
```

## 4. Standard Input and Output

```text
Command
  |
  ├── Standard Input  (stdin)  → FD 0
  ├── Standard Output (stdout) → FD 1
  └── Standard Error  (stderr) → FD 2
```

- `>` redirects standard output and overwrites the destination file.
- `>>` appends standard output to a file.
- `2>` redirects standard error.
- `|` sends one command's standard output to another command.

## Key Idea

File management is more than memorizing commands. Understand where you are in the directory hierarchy, which path you are operating on, and whether a command creates, modifies, moves, or deletes data.
