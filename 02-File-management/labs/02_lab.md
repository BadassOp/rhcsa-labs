# Lab 02 — Create, Copy, Move, and Delete Files

## Objective

Practice creating files and directories, copying data, renaming files, moving files between directories, and deleting files safely.

## Environment

- Operating system: RHEL
- Access: Local terminal or SSH session
- Privileges: Regular user
- Working directory: `~/file-management-lab`

## Tasks

### Task 1: Create the Lab Environment

```bash
mkdir -p ~/file-management-lab/{source,backup,archive}
cd ~/file-management-lab
pwd
ls -l
```

**Expected result:** Three directories should exist: `source`, `backup`, and `archive`.

### Task 2: Create Files

```bash
touch source/file1.txt source/file2.txt
echo "Linux file management practice" > source/file1.txt
echo "RHCSA hands-on lab" > source/file2.txt
ls -l source
cat source/file1.txt
```

**Expected result:** Two files should exist in `source/`, each containing the specified text.

### Task 3: Copy Files

Copy one file:

```bash
cp source/file1.txt backup/
```

Copy a file with a new name:

```bash
cp source/file2.txt backup/file2-copy.txt
```

Verify:

```bash
ls -l backup
cat backup/file1.txt
```

**Expected result:** The original files remain in `source/`, and their copies exist in `backup/`.

### Task 4: Move and Rename Files

Rename a file:

```bash
mv source/file2.txt source/renamed-file.txt
```

Move a file to another directory:

```bash
mv source/renamed-file.txt archive/
```

Verify:

```bash
ls -l source
ls -l archive
```

**Expected result:** `renamed-file.txt` should now exist in `archive/`, not in `source/`.

### Task 5: Copy a Directory

```bash
cp -r source source-copy
ls -l source-copy
```

**Expected result:** The `source-copy/` directory should contain the files currently present in `source/`.

### Task 6: Delete Files and Empty Directories

Delete a file:

```bash
rm backup/file2-copy.txt
```

Check whether a directory is empty:

```bash
ls -la source-copy
```

Remove it only if empty:

```bash
rmdir source-copy
```

**Expected result:** The copied file is removed, and the empty directory is deleted.

Note: `rmdir` fails if the directory contains files or subdirectories.

## Verification Checklist

- Created directories and files.
- Wrote text to files.
- Copied files without removing originals.
- Renamed and moved a file.
- Copied a directory recursively.
- Deleted a file and an empty directory.

## Cleanup

Inspect the lab directory first:

```bash
find ~/file-management-lab -maxdepth 2 -print
```

After confirming the path and contents, remove the lab environment:

```bash
rm -r ~/file-management-lab
```

This permanently removes the specified directory and its contents. Run it only after verifying that the path is correct and contains no data you need.

## Key Takeaways

- `touch` creates an empty file if it does not exist.
- `cp` copies files and directories.
- `mv` moves or renames files.
- `rm` deletes files, while `rmdir` removes empty directories.
- Always verify paths before destructive operations.
