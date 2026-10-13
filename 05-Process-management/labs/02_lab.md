# Lab 02: Signals and Job Control

## Objective

Practice foreground and background jobs, job control, and process termination using signals.

## Tasks

### 1. Start a foreground process

```bash
sleep 300
```

Press `Ctrl+Z` to suspend it.

Check the job:

```bash
jobs
```

Resume it in the background:

```bash
bg
```

Bring it back to the foreground:

```bash
fg
```

Press `Ctrl+C` to terminate it.

### 2. Start a background process

```bash
sleep 600 &
jobs
pgrep -a sleep
```

Record the PID.

### 3. Terminate using SIGTERM

Replace `<PID>` with the PID of the test process you created.

```bash
kill -TERM <PID>
```

Verify that it has stopped:

```bash
pgrep -a sleep
```

### 4. Understand common signals

| Signal         | Purpose                                                                                 |
| -------------- | --------------------------------------------------------------------------------------- |
| `SIGTERM` (15) | Requests graceful termination                                                           |
| `SIGKILL` (9)  | Forces termination; cannot be caught or ignored                                         |
| `SIGSTOP`      | Suspends a process                                                                      |
| `SIGCONT`      | Resumes a suspended process                                                             |
| `SIGHUP` (1)   | Traditionally indicates a terminal hangup; some services use it to reload configuration |

Inspect available signals:

```bash
kill -l
```

### 5. Practice SIGKILL safely

Start a new test process:

```bash
sleep 600 &
pgrep -a sleep
```

Use the PID of that specific test process:

```bash
kill -KILL <PID>
```

**Warning:** SIGKILL does not allow the process to perform normal cleanup. Use it only for this disposable test process in this lab.

## Verification Checklist

- Suspended a foreground process using `Ctrl+Z`.
- Used `jobs`, `bg`, and `fg`.
- Terminated a test process using SIGTERM.
- Practiced SIGKILL on a test process only.
- Explained the difference between SIGTERM and SIGKILL.

## Expected Outcome

You can manage shell jobs and safely send signals to processes you control.
