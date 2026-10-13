# Process Management — Mental Model

## 1. How a Process Works

A process is a running instance of a program.

```text
Program on Disk
      |
      v
   Execution
      |
      v
    Process
      |
      ├── PID
      ├── PPID
      ├── Memory
      ├── CPU Time
      └── Process State
```

- **PID:** Process ID.
- **PPID:** Parent Process ID.
- **Memory:** Address space and other memory resources used by the process.
- **CPU Time:** Processor time consumed by the process.
- **Process State:** The process's current scheduling or lifecycle state.

## 2. Parent and Child Processes

```text
systemd (PID 1)
      |
      ├── Service Process
      |
      └── User Session
              |
              └── Shell
                    |
                    └── Command
```

This is a simplified example. Actual process trees depend on how programs are launched and managed.

## 3. Process States

Common states shown by Linux process tools include:

- **R — Running or runnable:** Executing or ready to run.
- **S — Interruptible sleep:** Waiting for an event.
- **D — Uninterruptible sleep:** Usually waiting for certain kernel or I/O operations.
- **T — Stopped:** Suspended or stopped by a signal.
- **Z — Zombie:** Terminated, but awaiting collection by its parent.

A process in state `D` may not respond immediately to termination signals because it is waiting in an uninterruptible kernel operation.

## 4. Process Control Lifecycle

```text
Start Process
      |
      v
Inspect with ps / top
      |
      v
Monitor Resource Usage
      |
      v
Send Signal if Required
      |
      v
Verify Process State
```

Sending a signal does not guarantee immediate termination. Always verify the result.

## 5. Foreground vs Background

```text
Shell
  |
  ├── Foreground Job
  |      └── Controls terminal interaction
  |
  └── Background Job
         └── Runs without occupying
             the foreground job slot
```

Background jobs started with `&` may still be affected by terminal closure or session termination. Use an appropriate session manager or service manager for workloads that must persist independently.

## 6. Process Priority

The nice value influences the scheduling priority of normal processes.

```text
Nice Value -20  → Higher Priority
Nice Value   0  → Default Nice Value
Nice Value  19  → Lower Priority
```

Nice values influence CPU scheduling; they do not reserve CPU resources or guarantee a particular amount of processing time.

## Key Idea

Process management means understanding what is running, what resources it consumes, how it is related to other processes, and how to control it safely.
