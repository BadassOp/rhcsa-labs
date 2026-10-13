# Lab 01: Process Inspection and Monitoring

## Objective

Learn to inspect running processes, identify process IDs (PIDs), and monitor system resource usage.

## Tasks

### 1. Inspect running processes

```bash
ps
ps -ef
ps aux
```

- Identify the PID and PPID of several processes.
- Understand the difference between `ps -ef` and `ps aux`.

### 2. Search for a process

```bash
sleep 300 &
pgrep -a sleep
ps -ef | grep '[s]leep'
```

Record the PID of the `sleep` process.

### 3. Monitor processes

```bash
top
```

- Identify the processes consuming the most CPU and memory.
- Press `q` to exit.

If installed, you can also use:

```bash
free -h
uptime
```

### 4. Inspect a specific process

Replace `<PID>` with the actual PID you identified.

```bash
ps -p <PID> -o pid,ppid,user,stat,%cpu,%mem,cmd
```

### 5. Clean up

Stop only the test process you created:

```bash
pkill -x sleep
```

**Note:** Use this cleanup command only if you have no other `sleep` processes you need to keep running.

## Verification Checklist

- Inspected processes using `ps`.
- Identified a process PID and PPID.
- Located a process using `pgrep`.
- Monitored CPU and memory using `top`.
- Explained what PID and PPID represent.

## Expected Outcome

You can inspect running processes and understand basic process information and resource usage.
