# LAB-03 — Windows Processes, Services, and Startup Troubleshooting

## Objective

Investigate and resolve an unexpected startup application and an unavailable Windows service.

The lab required identifying a running process, tracing the mechanism that launched it, reviewing a Windows service configuration, applying the appropriate corrections, and validating that both solutions persisted after restarting the workstation.

## Scenario

Maya Torres reported two problems on workstation `LAB-WS01`:

1. Notepad opened automatically whenever she signed in to Windows.
2. Printing functionality was unavailable.

The reported symptoms had to be reproduced and investigated without terminating unfamiliar processes or modifying unrelated Windows services.

## Lab Environment

- Workstation: `LAB-WS01`
- Operating system: Windows 11 Enterprise Evaluation
- Administrator account: `ITLabAdmin`
- Standard user: `mtorres`
- Reported user: Maya Torres
- Unexpected application: Notepad
- Process name: `Notepad.exe`
- Windows service: Print Spooler
- Service name: `Spooler`
- Service executable: `C:\Windows\System32\spoolsv.exe`
- Related ticket: `INC-003 — Unexpected Startup Application and Print Service Failure`

## Skills Practiced

- Reproducing and confirming reported symptoms
- Identifying Windows processes in Task Manager
- Understanding process IDs
- Using the `tasklist` command
- Reviewing Startup apps
- Inspecting the current user's Startup folder
- Distinguishing an executable from its startup mechanism
- Querying Windows service status and configuration
- Using `sc query` and `sc qc`
- Changing a service startup type
- Starting a Windows service
- Validating corrections after a restart
- Documenting an incident in GitHub

## Procedure

### 1. Reproduce the Notepad Issue

Signed in to Windows as `mtorres`.

Notepad opened automatically without being launched manually, confirming the first reported symptom.

### 2. Identify the Running Process

Opened Task Manager and reviewed the running applications.

Notepad was identified as:

- Process: `Notepad.exe`
- PID: `12376`
- User: `mtorres`
- Architecture: `x64`
- Status: Running

The process ID was then confirmed from Command Prompt by running:

```cmd
tasklist /FI "PID eq 12376"
```

The command returned the same `Notepad.exe` process.

A process ID is assigned to a running process and may change the next time the application starts.

### 3. Review the Startup Configuration

Opened the Startup apps section in Task Manager.

Notepad appeared as an enabled startup application.

Using **Open file location** from Task Manager opened:

`C:\Windows\System32\notepad.exe`

This identified the executable that was running, but it did not identify the mechanism responsible for launching it during sign-in.

### 4. Locate the Startup Mechanism

Opened the current user's Startup folder by running:

```text
shell:startup
```

The following shortcut was found:

`Notepad Startup Test`

The shortcut properties showed:

- Target: `C:\Windows\System32\notepad.exe`
- Start in: `C:\Windows\System32`

This confirmed that the shortcut inside the `mtorres` Startup folder caused Notepad to launch after sign-in.

### 5. Remove the Startup Shortcut

Deleted the `Notepad Startup Test` shortcut from the current user's Startup folder.

Signed out and signed back in as `mtorres`.

Notepad no longer opened automatically.

The result was confirmed by running:

```cmd
tasklist /FI "IMAGENAME eq notepad.exe"
```

The command returned:

```text
INFO: No tasks are running which match the specified criteria.
```

### 6. Review the Print Spooler Service

Queried the Print Spooler service by running:

```cmd
sc query spooler
```

The result showed:

```text
STATE: STOPPED
```

Reviewed the service configuration by running:

```cmd
sc qc spooler
```

The result showed:

```text
START_TYPE: DISABLED
```

The service executable and service account were also reviewed:

- Binary path: `C:\Windows\System32\spoolsv.exe`
- Service account: `LocalSystem`

The executable path and service account were normal. The problem was the service configuration: the service was both stopped and disabled.

### 7. Restore the Print Spooler Configuration

Signed in as `ITLabAdmin` and opened Command Prompt with administrative privileges.

Changed the service startup type by running:

```cmd
sc config spooler start= auto
```

The command returned:

```text
[SC] ChangeServiceConfig SUCCESS
```

The space after `start=` is required by the `sc` command syntax.

Started the service by running:

```cmd
sc start spooler
```

The service initially entered:

```text
STATE: START_PENDING
```

This was a transitional state indicating that Windows was starting the service.

### 8. Confirm the Restored Service

Queried the service again after it finished starting.

The final status showed:

```text
STATE: RUNNING
```

The service configuration showed:

```text
START_TYPE: AUTO_START
```

This confirmed that Print Spooler was running and configured to start automatically with Windows.

### 9. Perform Final Validation

Restarted Windows and signed in again as `mtorres`.

Confirmed that Notepad did not open automatically.

Ran:

```cmd
tasklist /FI "IMAGENAME eq notepad.exe"
```

The command confirmed that no Notepad process was running.

Ran:

```cmd
sc query spooler
```

The Print Spooler service remained in:

```text
STATE: 4 RUNNING
```

This confirmed that both corrections persisted after restarting the workstation.

## Validation

### Automatic Notepad Launch Reproduced

Notepad opened automatically after `mtorres` signed in, confirming the reported symptom.

![Notepad automatic launch reproduced](../screenshots/LAB-03/01-notepad-auto-launch-reproduced.png)

### Notepad Process Identified

Task Manager identified `Notepad.exe` running under `mtorres` with PID `12376`.

![Notepad process and PID confirmed](../screenshots/LAB-03/02-notepad-process-pid-confirmed.png)

### Startup Shortcut Identified

The shortcut responsible for launching Notepad was found in the current user's Startup folder.

![Notepad startup shortcut identified](../screenshots/LAB-03/03-notepad-startup-shortcut-identified.png)

### Print Spooler Misconfiguration Confirmed

The Print Spooler service was found stopped and disabled.

![Print Spooler stopped and disabled](../screenshots/LAB-03/04-print-spooler-stopped-disabled.png)

### Print Spooler Restored

The service was configured as `AUTO_START` and confirmed in the `RUNNING` state.

![Print Spooler restored and running](../screenshots/LAB-03/05-print-spooler-restored-running.png)

### Final Validation After Restart

After restarting Windows:

- No `Notepad.exe` process was running.
- Print Spooler remained in `STATE: 4 RUNNING`.

![Final validation after restart](../screenshots/LAB-03/06-final-validation-after-restart.png)

## Troubleshooting

### Notepad Attempted to Restore a Missing File

When Notepad opened automatically, it also displayed a message stating that it could not find a previously opened test file.

This was a separate Notepad session-restoration behavior and was not the cause of the automatic startup problem.

The startup issue was traced independently to the shortcut in the `mtorres` Startup folder.

### Open File Location Did Not Reveal the Launcher

Using **Open file location** from Task Manager opened the executable:

`C:\Windows\System32\notepad.exe`

This answered the question:

> What program is running?

It did not answer:

> What caused the program to launch during sign-in?

The startup source was found by reviewing Startup apps and the current user's Startup folder.

This demonstrated the difference between an application's executable and the mechanism that launches it.

### Print Spooler Entered START_PENDING

Immediately after running `sc start spooler`, the service entered:

```text
STATE: START_PENDING
```

This did not represent a failure. It indicated that Windows had accepted the start request and was still initializing the service.

The service was queried again after a short wait and reached:

```text
STATE: RUNNING
```

### Administrative Rights Were Required

The service was diagnosed from the standard user's session, but changing its startup configuration required administrative privileges.

The correction was therefore performed from an elevated Command Prompt under `ITLabAdmin`.

## Lessons Learned

- A reported problem should be reproduced before making configuration changes.
- Task Manager can identify a process, its user, status, and process ID.
- A PID identifies a specific running instance and may change when the process starts again.
- `tasklist` provides a command-line method for confirming running processes.
- The executable path identifies the program but does not always identify what launched it.
- Startup problems should be traced through Startup apps, user Startup folders, system Startup folders, registry Run keys, scheduled tasks, services, and logon scripts as necessary.
- A Windows service has both a current state and a startup type.
- `STOPPED` describes the current state of a service.
- `DISABLED` describes a configuration that prevents the service from starting normally.
- `START_PENDING` is a temporary state while a service is starting.
- `RUNNING` confirms that the service has started successfully.
- Service configuration changes generally require administrative privileges.
- A correction should be validated after restarting Windows to confirm that it persists.
- Troubleshooting documentation should separate the reported symptoms, diagnostic checks, findings, actions, and final results.

## Conclusion

The objectives of LAB-03 were completed successfully.

The automatic Notepad launch was traced to a shortcut inside the `mtorres` Startup folder. The shortcut was removed, and validation confirmed that Notepad no longer launched automatically.

The Print Spooler service was found stopped and disabled. Its startup type was restored to `AUTO_START`, the service was started, and validation after restarting Windows confirmed that it remained in the `RUNNING` state.

This lab demonstrated practical Windows process identification, startup troubleshooting, service diagnosis, administrative correction, post-restart validation, and incident documentation.
