# Remote Linux Access Using SSH

## 1. What Is Remote Access?

Remote access allows us to control and interact with a computer over a network without physically using that machine.

**SSH (Secure Shell)** is a protocol used to securely access and manage remote systems through a command-line interface.

## 2. How It Works

```text
Windows Host
     |
     | SSH Connection
     | TCP Port 22
     v
RHEL Virtual Machine
     |
     v
Remote Terminal
```

- **Client:** The computer initiating the connection.
- **Server:** The remote machine accepting the connection.
- **IP Address:** Identifies the remote machine on the network.
- **Username:** Identifies the account used to log in.
- **Shell:** Interprets commands entered in the terminal.

## 3. Prerequisites

- A running RHEL virtual machine.
- SSH server installed and running on RHEL.
- A valid RHEL username and password, or another configured authentication method.
- Network connectivity between the Windows host and the VM.
- Firewall rules allowing SSH connections.

## 4. Configure RHEL for Remote Access

Open the terminal inside your RHEL virtual machine.

### Step 1: Check the IP Address

```bash
ip -br addr
```

Example output:

```text
enp0s3    UP    192.168.1.120/24
```

Here, `192.168.1.120` is the example IP address of the VM. Use the actual address displayed on your system.

### Step 2: Start the SSH Service

Check whether the SSH server is installed:

```bash
rpm -q openssh-server
```

If it is not installed, install it:

```bash
sudo dnf install openssh-server
```

Start SSH and enable it at boot:

```bash
sudo systemctl enable --now sshd
```

Verify the service:

```bash
systemctl status sshd
```

Look for `active (running)`.

### Step 3: Check the Firewall

```bash
sudo firewall-cmd --list-services
```

If `ssh` is not listed and the firewall is running, allow SSH:

```bash
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

## 5. Connect from Windows

Open **PowerShell** or Command Prompt on Windows.

### Step 1: Connect to RHEL

```powershell
ssh username@192.168.1.120
```

Replace `username` with your RHEL username and the example IP with your VM's actual IP address.

### Step 2: Verify the Connection

On the first connection, SSH may ask you to verify the server's host key. Verify the fingerprint when possible before accepting it.

Enter your password when prompted. Password characters generally won't appear while typing; this is normal.

After successful login, you are working inside the remote RHEL system.

### Step 3: Disconnect

```bash
exit
```

This closes the SSH session and returns you to your Windows terminal.

## 6. Understanding the Terminal and Shell

A **terminal** is the interface through which you interact with a command-line system. A **shell** is the program that interprets your commands and executes them.

For example, Bash is a commonly used Linux shell.

After connecting to RHEL, try these commands:

| Command       | Purpose                              |
| ------------- | ------------------------------------ |
| `whoami`      | Display the current username         |
| `hostname`    | Display the system hostname          |
| `hostnamectl` | Display system and OS information    |
| `pwd`         | Show the current working directory   |
| `ls`          | List files and directories           |
| `cd /etc`     | Change to the `/etc` directory       |
| `cd ..`       | Move to the parent directory         |
| `clear`       | Clear the terminal screen            |
| `man ls`      | Open the manual for the `ls` command |
| `exit`        | Exit the shell or SSH session        |

### Understanding the Prompt

Example:

```text
[user@rhel-server ~]$
```

- `user` — Current username.
- `rhel-server` — Hostname.
- `~` — Current user's home directory.
- `$` — Typical prompt for a regular user.

A root shell commonly uses `#` as its prompt.

## 7. VirtualBox Networking Considerations

Your VM's network configuration determines whether Windows can reach it.

- **Bridged Adapter:** The VM can usually obtain an address on the same network as the host, making direct SSH connections convenient.
- **NAT:** The VM can access external networks, but incoming SSH connections from the host generally require port forwarding.
- **Host-only Adapter:** Provides a private network between the host and VM, useful for isolated labs.

Use an IP address reachable from your Windows machine. If the connection times out, check the VM's network mode, IP address, SSH service, and firewall.

## 8. Verification

After connecting, run:

```bash
whoami
hostname
pwd
cat /etc/redhat-release
```

Confirm that the username, hostname, working directory, and RHEL version match your remote VM.

## Key Takeaways

- SSH provides secure command-line access to remote Linux systems.
- The SSH server runs through the `sshd` service on RHEL.
- The client connects using `ssh username@IP_ADDRESS`.
- The terminal provides the interface; the shell interprets commands.
- Network configuration, firewall rules, and valid credentials are necessary for a successful connection.

## References

- [OpenSSH Manual](https://www.openssh.com/manual.html)
- [Red Hat Enterprise Linux Documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/)
