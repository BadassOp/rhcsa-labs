# Process Management — Useful Commands

## Inspect Processes

| Command        | Purpose                                                |
| -------------- | ------------------------------------------------------ |
| `ps`           | Display processes associated with the current terminal |
| `ps aux`       | Display a detailed list of processes                   |
| `ps -ef`       | Display processes in full-format listing               |
| `ps -p PID -f` | Inspect a specific process                             |
| `pstree`       | Display processes in a tree                            |
| `pgrep sshd`   | Find process IDs by process name                       |
| `pidof sshd`   | Display PIDs for a named program, if available         |

## Monitor Resources

| Command    | Purpose                                                   |
| ---------- | --------------------------------------------------------- |
| `top`      | Monitor processes and system resource usage interactively |
| `free -h`  | Display memory usage                                      |
| `uptime`   | Display uptime and load averages                          |
| `vmstat 1` | Display system activity at one-second intervals           |

## Signals and Process Control

| Command              | Purpose                                    |
| -------------------- | ------------------------------------------ |
| `kill PID`           | Send the default `SIGTERM` signal          |
| `kill -15 PID`       | Request graceful process termination       |
| `kill -9 PID`        | Send `SIGKILL` to force termination        |
| `kill -l`            | List signal names                          |
| `pkill process_name` | Send a signal to processes matching a name |
| `kill -STOP PID`     | Stop a process                             |
| `kill -CONT PID`     | Resume a stopped process                   |

Always identify the target process before sending signals. Use `SIGKILL` only when normal termination fails and forceful termination is justified.

## Foreground and Background Jobs

| Command     | Purpose                                  |
| ----------- | ---------------------------------------- |
| `command &` | Start a command in the background        |
| `jobs`      | List jobs managed by the current shell   |
| `Ctrl+Z`    | Suspend the foreground job               |
| `bg`        | Resume a suspended job in the background |
| `fg`        | Bring a job into the foreground          |
| `wait`      | Wait for background jobs to finish       |

Shell jobs are not the same as all system processes. `jobs`, `bg`, and `fg` operate on jobs managed by the current shell.

## Process Priorities

| Command                    | Purpose                                      |
| -------------------------- | -------------------------------------------- |
| `nice -n 10 command`       | Start a command with a specified nice value  |
| `renice 10 -p PID`         | Adjust the nice value of an existing process |
| `ps -o pid,ni,comm -p PID` | Inspect a process's nice value               |

Nice values generally range from `-20` to `19`. Lower values indicate a higher scheduling priority; changing to a more favorable priority may require elevated privileges.

## Useful Inspection Examples

Find a process:

```bash
pgrep -a sshd
```

Inspect a process:

```bash
ps -p PID -o pid,ppid,stat,ni,%cpu,%mem,comm
```

Replace `PID` with the actual numeric process ID.

Monitor processes:

```bash
top
```

## Important Notes

- A PID identifies a process while it exists; PIDs can be reused later.
- `SIGTERM` requests termination, while `SIGKILL` cannot be caught or ignored.
- A process may require elevated privileges to inspect or control fully.
- A zombie process has already terminated but still has an entry awaiting collection by its parent.
- Use `systemctl` to manage services rather than treating every service process as an isolated command-line job.
