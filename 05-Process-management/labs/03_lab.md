# Lab 03: Process Priorities

## Objective

Understand process priority and practice using `nice` and `renice`.

## Tasks

### 1. Inspect process priorities

```bash
ps -eo pid,ni,pri,stat,comm --sort=-ni
```

Understand these fields:

- `PID`: Process ID
- `NI`: Nice value
- `PRI`: Kernel scheduling priority
- `STAT`: Process state
- `COMM`: Command name

For ordinary Linux processes, the nice value typically ranges from `-20` to `19`. Lower nice values generally mean a higher scheduling priority.

### 2. Start a process with a nice value

```bash
nice -n 10 sleep 600 &
```

Find the process:

```bash
pgrep -a sleep
```

Record its PID and inspect its nice value:

```bash
ps -p <PID> -o pid,ni,pri,cmd
```

Expected: the test process should normally show a nice value of `10`.

### 3. Change the nice value

Replace `<PID>` with the PID of the test process.

```bash
renice 15 -p <PID>
ps -p <PID> -o pid,ni,pri,cmd
```

Verify that the nice value is now `15`.

### 4. Understand permissions

Try setting a lower nice value:

```bash
renice 5 -p <PID>
```

An unprivileged user generally cannot increase scheduling priority by lowering the nice value. If permission is denied, record the result. Do not use `sudo` merely to bypass this exercise.

### 5. Clean up

```bash
kill -TERM <PID>
```

Use the PID of your test process and verify that it has stopped.

## Verification Checklist

- Inspected process nice values.
- Started a process using `nice`.
- Changed its nice value using `renice`.
- Understood why changing to a lower nice value may require elevated privileges.
- Cleaned up the test process.

## Expected Outcome

You can inspect and adjust process nice values and explain how they influence scheduling priority.
