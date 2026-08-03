# LAB-03 Evidence

Visual evidence for **LAB-03 — Windows Processes, Services, and Startup Troubleshooting**.

The account and employee information used in this lab are fictitious. Passwords and other credentials were not included in the public documentation.

## 1. Automatic Notepad Launch Reproduced

After signing in as `mtorres`, Notepad opened automatically without being launched manually.

This confirmed the first reported symptom.

![Notepad automatic launch reproduced](01-notepad-auto-launch-reproduced.png)

## 2. Notepad Process Identified

Task Manager identified the running process as `Notepad.exe`.

The process was running under the `mtorres` account with PID `12376`.

![Notepad process and PID confirmed](02-notepad-process-pid-confirmed.png)

## 3. Startup Shortcut Identified

The shortcut responsible for launching Notepad was found in the current user's Startup folder.

The shortcut target was:

`C:\Windows\System32\notepad.exe`

![Notepad startup shortcut identified](03-notepad-startup-shortcut-identified.png)

## 4. Print Spooler Misconfiguration Confirmed

The `sc query spooler` command showed that the Print Spooler service was:

`STOPPED`

The `sc qc spooler` command showed that its startup type was:

`DISABLED`

![Print Spooler stopped and disabled](04-print-spooler-stopped-disabled.png)

## 5. Print Spooler Restored

The Print Spooler startup type was changed to `AUTO_START`, and the service was started successfully.

The service configuration and running state were then confirmed.

![Print Spooler restored and running](05-print-spooler-restored-running.png)

## 6. Final Validation After Restart

After restarting Windows and signing in as `mtorres`, the `tasklist` command confirmed that no `Notepad.exe` process was running.

The Print Spooler service remained in:

`STATE: 4 RUNNING`

This confirmed that both corrections persisted after the restart.

![Final validation after restart](06-final-validation-after-restart.png)
