# Windows Command Quick Reference

Quick reference of Windows commands used in the IT Support Lab project.

---

## Windows Run Commands

Open Windows Run with:

`Windows key + R`

| Command | Meaning |
|---|---|
| `lusrmgr.msc` | Opens **Local Users and Groups Manager** to manage local users and groups. |
| `shell:startup` | Opens the Startup folder for the currently signed-in user. Shortcuts placed here can launch automatically at sign-in. |
| `diskmgmt.msc` | Opens Disk Management to initialize disks, create volumes, format file systems, and manage drive letters. |

---

## Command Prompt — System and User Information

| Command | Meaning |
|---|---|
| `whoami` | Displays the account currently signed in or executing the command. |
| `net session` | Displays network sessions connected to the computer. Usually requires administrator privileges. |

---

## Command Prompt — Network Configuration and Troubleshooting

| Command | Meaning |
|---|---|
| `ipconfig /all` | Displays detailed IP configuration for all network adapters, including IPv4, gateway, DHCP, and DNS information. |
| `ping 10.0.2.2` | Tests connectivity to the VirtualBox default gateway used in the lab. |
| `ping 8.8.8.8` | Tests connectivity to an external IP address without depending on DNS. |
| `nslookup microsoft.com` | Queries DNS to resolve the domain name `microsoft.com` into IP addresses. |
| `tracert 8.8.8.8` | Displays the network route or hops used to reach the destination. |

### General Syntax

```cmd
ping <IP address or hostname>
```

Tests connectivity to the specified address or hostname.

```cmd
nslookup <domain name>
```

Queries DNS information for the specified domain.

```cmd
tracert <IP address or hostname>
```

Traces the network path to the specified destination.

---

## Command Prompt — Processes

| Command | Meaning |
|---|---|
| `tasklist` | Displays currently running processes. |
| `tasklist /FI "PID eq 12376"` | Filters the process list to show the process with PID `12376`. |
| `tasklist /FI "IMAGENAME eq notepad.exe"` | Checks whether a process named `notepad.exe` is currently running. |

### General Syntax

```cmd
tasklist /FI "PID eq <PID number>"
```

Searches for a running process using its Process ID.

```cmd
tasklist /FI "IMAGENAME eq <executable name>"
```

Searches for a running process using its executable name.

---

## Command Prompt — Windows Services

| Command | Meaning |
|---|---|
| `sc query spooler` | Displays the current state of the Print Spooler service, such as `STOPPED` or `RUNNING`. |
| `sc qc spooler` | Displays the saved configuration of the Print Spooler service, including its startup type and executable path. |
| `sc config spooler start= auto` | Changes the Print Spooler startup type to Automatic. Requires administrator privileges. |
| `sc start spooler` | Starts the Print Spooler service. Requires administrator privileges. |

### General Syntax

```cmd
sc query <service name>
```

Checks the current state of a Windows service.

```cmd
sc qc <service name>
```

Displays the configuration of a Windows service.

```cmd
sc config <service name> start= <startup type>
```

Changes the startup type of a Windows service.

```cmd
sc start <service name>
```

Starts a Windows service.

### Service States Seen

| State | Meaning |
|---|---|
| `STOPPED` | The service is not currently running. |
| `START_PENDING` | Windows is in the process of starting the service. |
| `RUNNING` | The service is currently running. |

### Startup Types Seen

| Startup Type | Meaning |
|---|---|
| `DISABLED` | The service cannot start normally. |
| `AUTO_START` | The service is configured to start automatically with Windows. |

---

## Command Prompt — Storage and Files

| Command | Meaning |
|---|---|
| `fsutil volume diskfree D:` | Displays the total, used, reserved, and available space on volume `D:`. |
| `fsutil file createnew D:\LAB-04-FILLER-01.bin 1150000000` | Creates a file with a specified logical length in bytes. Requires administrator privileges. |
| `dir D:\Maya-Storage-Large-Test.txt` | Confirms whether the specified file exists and displays its size. |
| `dir D:\ /a /-c` | Displays all items on `D:`, including hidden and system items, with exact byte values. |
| `del D:\LAB-04-FILLER-02.bin` | Deletes the specified verified file. |
| `notepad D:\Maya-Storage-Large-Test.txt` | Opens the specified text file in Notepad. |
| `(for /L %i in (1,1,3000) do @echo <text>) > "<file path>"` | Repeats text a specified number of times and redirects the output into a file. |

### General Syntax

```cmd
fsutil volume diskfree <drive letter>:
```

Displays storage information for the selected volume.

```cmd
fsutil file createnew <file path> <length in bytes>
```

Creates a file with the specified logical length.

```cmd
dir <file or folder path>
```

Displays information about the selected file or directory.

```cmd
dir <drive or folder> /a /-c
```

Displays hidden and system items and shows exact byte values without digit separators.

```cmd
del <file path>
```

Deletes the specified file. The target should be verified before running the command.

```cmd
notepad <file path>
```

Opens the specified file in Notepad.

### Parameters Introduced

| Parameter or operator | Meaning |
|---|---|
| `/a` | Displays all items, including hidden and system files and folders. |
| `/-c` | Removes digit separators from file sizes in `dir` output. |
| `/L` | Runs a numbered loop with the `for` command. |
| `>` | Redirects command output into a file, replacing the destination if it exists. |
| `%i` | Loop variable used by the `for` command in Command Prompt. |

---

## Common Parameters and Terms

| Term | Meaning |
|---|---|
| `/all` | Requests all available detailed information. |
| `/FI` | Applies a filter to the command output. |
| `PID` | Process Identifier assigned to a running process. |
| `IMAGENAME` | Executable name of a process, such as `notepad.exe`. |
| `eq` | Means “equal to” inside a filter. |
| `sc` | Service Control command. |
| `query` | Requests the current state of a service. |
| `qc` | Means Query Configuration. |
| `config` | Modifies a service configuration. |
| `start` | Sends a request to start a service. |

---

## PowerShell

No PowerShell commands have been used in LAB-01 through LAB-04.

New PowerShell commands will be added here as they are introduced in future labs.
