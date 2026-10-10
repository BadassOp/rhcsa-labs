# Lab 03 — View and Search Files

## Objective

Practice reading file contents, inspecting selected lines, searching for text, and locating files using Linux command-line tools.

## Environment

- Operating system: RHEL
- Access: Local terminal or SSH session
- Privileges: Regular user
- Working directory: `~/file-search-lab`

## Tasks

### Task 1: Create Sample Files

```bash
mkdir -p ~/file-search-lab/{documents,logs}
cd ~/file-search-lab
```

Create a sample document:

```bash
cat > documents/notes.txt <<'EOF'
Linux is an open-source operating system kernel.
RHEL is an enterprise Linux distribution.
The Linux command line is useful for administration.
File management is an essential Linux skill.
SSH provides secure remote terminal access.
EOF
```

Create a sample log:

```bash
cat > logs/application.log <<'EOF'
INFO Application started
INFO User authenticated
WARNING Disk usage is high
ERROR Database connection failed
INFO Application restarted
ERROR Connection timeout
EOF
```

Verify:

```bash
ls -R
```

### Task 2: View File Contents

Display the entire document:

```bash
cat documents/notes.txt
```

Display the first two lines:

```bash
head -n 2 documents/notes.txt
```

Display the last two lines:

```bash
tail -n 2 documents/notes.txt
```

Browse the file interactively:

```bash
less documents/notes.txt
```

Press `q` to exit `less`.

**Expected result:** View complete files or selected portions of their contents.

### Task 3: Search for Text with grep

Search for a word:

```bash
grep "Linux" documents/notes.txt
```

Search without case sensitivity:

```bash
grep -i "linux" documents/notes.txt
```

Display matching line numbers:

```bash
grep -n "ERROR" logs/application.log
```

Count matching lines:

```bash
grep -c "INFO" logs/application.log
```

**Expected result:** Find matching lines and identify their line numbers or counts.

### Task 4: Search Recursively

Search all files under the lab directory for the word `ERROR`:

```bash
grep -r -n "ERROR" .
```

**Expected result:** Find the two error entries in `logs/application.log`.

### Task 5: Locate Files with find

Find all text files:

```bash
find . -type f -name "*.txt"
```

Find all files with the `.log` extension:

```bash
find . -type f -name "*.log"
```

Find files whose names contain `notes`:

```bash
find . -type f -name "*notes*"
```

**Expected result:** Display file paths matching each filename pattern.

### Task 6: Combine Commands with a Pipe

Display only error entries:

```bash
cat logs/application.log | grep "ERROR"
```

A more direct approach is:

```bash
grep "ERROR" logs/application.log
```

Count error entries:

```bash
grep -c "ERROR" logs/application.log
```

**Expected result:** Both search methods should identify two error entries, and the count command should return `2`.

## Verification Checklist

- Displayed complete file contents.
- Viewed the beginning and end of a file.
- Used `less` to browse a document.
- Searched text using `grep`.
- Displayed matching line numbers.
- Located files using `find`.
- Used pipes to filter command output.

## Cleanup

Inspect the lab files:

```bash
find ~/file-search-lab -maxdepth 3 -print
```

After verifying the path and confirming that you no longer need the sample files, remove the lab directory:

```bash
rm -r ~/file-search-lab
```

## Key Takeaways

- `cat`, `less`, `head`, and `tail` help inspect file contents.
- `grep` searches file contents for matching patterns.
- `find` locates files and directories by specified criteria.
- Pipes pass one command's output to another command.
- Sample files are useful for practicing searches without modifying system logs.
