# Permissions and ACLs — Useful Commands

## Inspect Permissions and Ownership

| Command             | Purpose                                 |
| ------------------- | --------------------------------------- |
| `ls -l file.txt`    | Display file permissions and ownership  |
| `ls -ld directory/` | Display the directory's own permissions |
| `stat file.txt`     | Display detailed file metadata          |
| `id username`       | Display user and group identities       |
| `umask`             | Display the current file-creation mask  |

## Change Permissions

| Command                   | Purpose                                                  |
| ------------------------- | -------------------------------------------------------- |
| `chmod 644 file.txt`      | Set owner read/write and group/others read permissions   |
| `chmod 755 script.sh`     | Set owner full permissions and group/others read/execute |
| `chmod u+x script.sh`     | Add execute permission for the owner                     |
| `chmod g-w file.txt`      | Remove group write permission                            |
| `chmod o-r file.txt`      | Remove read permission for others                        |
| `chmod -R 755 directory/` | Apply permissions recursively; use caution               |

## Change Ownership

| Command                               | Purpose                                   |
| ------------------------------------- | ----------------------------------------- |
| `sudo chown user file.txt`            | Change the file owner                     |
| `sudo chown user:group file.txt`      | Change owner and group                    |
| `sudo chgrp group file.txt`           | Change group ownership                    |
| `sudo chown -R user:group directory/` | Recursively change ownership; use caution |

## Special Permissions

| Command                 | Purpose                                   |
| ----------------------- | ----------------------------------------- |
| `chmod u+s file`        | Set SUID                                  |
| `chmod g+s directory/`  | Set SGID on a directory                   |
| `chmod +t directory/`   | Set the sticky bit                        |
| `chmod 4755 file`       | Set SUID with permissions `755`           |
| `chmod 2775 directory/` | Set SGID with permissions `775`           |
| `chmod 1777 directory/` | Set the sticky bit with permissions `777` |

Use special permissions only when their behavior is understood. Applying SUID to arbitrary executables can create serious security risks.

## ACL Commands

| Command                               | Purpose                                                        |
| ------------------------------------- | -------------------------------------------------------------- |
| `getfacl file.txt`                    | Display file ACLs and associated permission information        |
| `setfacl -m u:alice:r file.txt`       | Grant user `alice` read permission through an ACL              |
| `setfacl -m g:developers:rw file.txt` | Grant group `developers` read/write permissions through an ACL |
| `setfacl -x u:alice file.txt`         | Remove the named user's ACL entry                              |
| `setfacl -b file.txt`                 | Remove extended ACL entries                                    |
| `setfacl -m d:u:alice:rwx directory/` | Add a default ACL for user `alice` on a directory              |

Default ACLs affect newly created filesystem objects beneath the directory, subject to the creating application's requested permissions and the filesystem's behavior. They do not retroactively change existing files.

## Verify Access

```bash
ls -l file.txt
stat file.txt
getfacl file.txt
```

For a directory:

```bash
ls -ld directory/
getfacl directory/
```

## Important Notes

- File permissions and ACLs are distinct from SELinux policy.
- An ACL mask can limit the effective permissions of named users, named groups, and the owning group.
- Execute permission on parent directories is generally required to traverse a path.
- Recursive permission or ownership changes can affect many files; inspect targets before using `-R`.
- Avoid using `chmod 777` as a generic solution to permission errors. Diagnose the actual cause instead.
