# File Management — Useful Commands

## Navigation

| Command   | Purpose                       |
| --------- | ----------------------------- |
| `pwd`     | Print the current directory   |
| `ls`      | List directory contents       |
| `ls -la`  | List all entries with details |
| `cd /etc` | Change directory              |
| `cd ..`   | Move to the parent directory  |
| `cd ~`    | Move to the home directory    |

## Create and Manage Files

| Command                  | Purpose                                     |
| ------------------------ | ------------------------------------------- |
| `touch file.txt`         | Create an empty file or update timestamps   |
| `mkdir docs`             | Create a directory                          |
| `mkdir -p a/b/c`         | Create nested directories                   |
| `cp file.txt backup.txt` | Copy a file                                 |
| `cp -r dir1 dir2`        | Copy a directory recursively                |
| `mv old.txt new.txt`     | Move or rename a file                       |
| `rm file.txt`            | Delete a file                               |
| `rmdir emptydir`         | Remove an empty directory                   |
| `rm -r directory`        | Recursively delete a directory; use caution |

## View File Contents

| Command                     | Purpose                     |
| --------------------------- | --------------------------- |
| `cat file.txt`              | Display file contents       |
| `less file.txt`             | Browse a file interactively |
| `head file.txt`             | Display the first 10 lines  |
| `tail file.txt`             | Display the last 10 lines   |
| `tail -f /var/log/messages` | Follow new log entries      |

## Search

| Command                        | Purpose                                       |
| ------------------------------ | --------------------------------------------- |
| `find /etc -name "*.conf"`     | Find matching filenames                       |
| `grep "error" file.txt`        | Search for a text pattern                     |
| `grep -i "error" file.txt`     | Search without case sensitivity               |
| `grep -r "pattern" directory/` | Search recursively                            |
| `locate filename`              | Search an indexed file database, if available |

## Redirection and Pipes

| Command                    | Purpose                         |
| -------------------------- | ------------------------------- |
| `echo "Hello" > file.txt`  | Overwrite a file with text      |
| `echo "Hello" >> file.txt` | Append text to a file           |
| `command 2> errors.txt`    | Redirect standard error         |
| `ls -l \| less`            | Browse command output           |
| `grep "root" /etc/passwd`  | Filter lines matching a pattern |

## Safety Notes

- Check your current directory with `pwd` before destructive operations.
- Inspect targets with `ls` before using `rm`.
- Be especially careful with `rm -r` and wildcard patterns.
- Use `sudo` only when elevated privileges are necessary.
- Avoid practicing destructive commands on important system directories.
