# LAB-04 — Storage and Application Troubleshooting

## Objective

Investigate and resolve an application save failure caused by insufficient storage space.

The lab required preparing an isolated secondary disk, recording its original condition, reproducing a controlled storage incident, distinguishing an application problem from a storage problem, restoring available capacity, and validating the correction after restarting Windows.

## Scenario

Maya Torres reported that Notepad opened normally and allowed her to edit a document, but the document could not be saved to the assigned data drive.

Windows displayed the following error:

`There is not enough space on the disk.`

The reported symptom had to be reproduced and diagnosed without filling the Windows system drive or deleting unknown files, user data, or Windows system directories.

## Lab Environment

- Workstation: `LAB-WS01`
- Operating system: Windows 11 Enterprise Evaluation
- Administrator account: `ITLabAdmin`
- Standard user: `mtorres`
- Reported user: Maya Torres
- Application: Notepad
- Secondary virtual disk: Approximately 1.1 GB
- Volume label: `LAB-DATA`
- Drive letter: `D:`
- File system: NTFS
- Partition style: GPT
- Related ticket: `INC-004 — Application Cannot Save File Due to Insufficient Disk Space`

## Skills Practiced

- Adding an isolated secondary virtual disk
- Initializing a disk using GPT
- Creating and formatting an NTFS volume
- Assigning and managing Windows drive letters
- Using Disk Management
- Measuring storage capacity and available space
- Creating controlled test files with `fsutil`
- Reproducing an insufficient disk space incident
- Distinguishing an application symptom from a storage root cause
- Reviewing hidden and system files safely
- Using `dir` and `del`
- Protecting user data during troubleshooting
- Validating file size and integrity
- Confirming that a correction persists after restarting Windows
- Performing final cleanup of controlled test data
- Documenting an incident in GitHub

## Procedure

### 1. Add an Isolated Secondary Virtual Disk

Powered off the virtual machine and added a dynamically allocated secondary virtual disk.

The first disk creation attempt failed because the virtual machine name contained:

`Name:`

The colon character is not permitted inside a Windows file or folder name and caused VirtualBox to return an invalid path error.

The virtual machine name was corrected to:

`IT-LAB-WIN11-01`

The secondary disk was then created and attached successfully.

A snapshot named:

`Before LAB-04 Incident`

was created before configuring the storage incident.

### 2. Initialize the Disk in Windows

Signed in as `ITLabAdmin` and opened Disk Management by running:

```text
diskmgmt.msc
```

The new disk initially appeared as:

- Unknown
- Not initialized
- Unallocated

The disk was initialized using GPT.

GPT was selected because it is the modern Windows partitioning standard and provides broader support than the older MBR partitioning scheme.

### 3. Create the LAB-DATA Volume

The drive letter `D:` was initially assigned to the virtual optical drive.

The optical drive letter was temporarily changed, allowing `D:` to be assigned consistently to the lab data volume.

A new simple volume was created using the full available disk capacity.

The volume was configured as:

- Label: `LAB-DATA`
- Drive letter: `D:`
- File system: NTFS
- Allocation: Default
- Quick format: Enabled
- Status: Healthy

![LAB-DATA volume created](../screenshots/LAB-04/01-lab-data-volume-created.png)

### 4. Record the Original Storage Condition

Opened Command Prompt with administrative privileges and ran:

```cmd
fsutil volume diskfree D:
```

The original volume condition was:

```text
Total bytes:       1,182,789,632
Total free bytes:  1,162,547,200
Used bytes:           16,048,128
Reserved bytes:        4,194,304
```

This established a baseline before creating the controlled incident.

### 5. Create the Controlled Storage Incident

Created the first controlled filler file by running:

```cmd
fsutil file createnew D:\LAB-04-FILLER-01.bin 1150000000
```

This created a file with a logical length of:

`1,150,000,000 bytes`

The available free space decreased from approximately 1.1 GB to:

`14,172,160 bytes`

A second controlled file was then created:

```cmd
fsutil file createnew D:\LAB-04-FILLER-02.bin 14170000
```

The volume was checked again:

```cmd
fsutil volume diskfree D:
```

The result showed:

```text
Total free bytes: 0
```

This confirmed that the controlled test files had consumed the available capacity of `D:`.

![Volume full with zero bytes free](../screenshots/LAB-04/02-volume-full-zero-bytes-free.png)

### 6. Perform the Initial Save Test

Signed out of `ITLabAdmin` and signed in as `mtorres`.

Created a small Notepad document containing:

```text
LAB-04 storage save validation test.
```

The file was saved as:

`D:\Maya-Storage-Test.txt`

Despite the volume reporting zero available bytes, the 36-byte document was saved and reopened successfully.

This was an unexpected result and did not reproduce the reported incident reliably.

The exact NTFS allocation behavior responsible for allowing the very small file to be written was not required to resolve the incident. A larger document was created to perform a more reliable test.

### 7. Create a Larger Test Document

Opened Command Prompt as `mtorres` and ran:

```cmd
(for /L %i in (1,1,3000) do @echo LAB-04 storage validation line %i - This document is used to reproduce an insufficient disk space error.) > "%USERPROFILE%\Documents\LAB-04-Large-Test.txt"
```

This command:

- Repeated a controlled text line 3,000 times
- Created the document in the user's `Documents` folder on `C:`
- Avoided modifying the full `D:` volume during file creation

The file was reviewed using:

```cmd
dir "%USERPROFILE%\Documents\LAB-04-Large-Test.txt"
```

Its size was confirmed as:

`322,893 bytes`

### 8. Reproduce the Save Failure

Opened the larger document in Notepad:

```cmd
notepad "%USERPROFILE%\Documents\LAB-04-Large-Test.txt"
```

Attempted to save it as:

`D:\Maya-Storage-Large-Test.txt`

Notepad displayed:

`There is not enough space on the disk.`

![Notepad insufficient disk space error](../screenshots/LAB-04/03-notepad-insufficient-disk-space-error.png)

The destination path was checked using:

```cmd
dir D:\Maya-Storage-Large-Test.txt
```

The command returned:

```text
File Not Found
```

This confirmed that the larger destination file had not been created and that the reported save failure had been reproduced successfully.

### 9. Identify the Root Cause

Reviewed all contents of the volume by running:

```cmd
dir D:\ /a /-c
```

The options used were:

- `/a` — Display hidden and system items
- `/-c` — Display file sizes without digit separators

The review identified:

- `LAB-04-FILLER-01.bin`
- `LAB-04-FILLER-02.bin`
- `Maya-Storage-Test.txt`
- `$RECYCLE.BIN`
- `System Volume Information`

The two `LAB-04-FILLER` files were verified controlled test data.

The user file and Windows system directories were not selected for deletion.

![Controlled filler files identified](../screenshots/LAB-04/04-controlled-filler-files-identified.png)

The investigation confirmed that:

- Notepad opened and edited documents normally
- The save failure occurred only when writing to the full `D:` volume
- The application was not damaged
- The root cause was insufficient available storage space

### 10. Restore Sufficient Space

Opened an elevated Command Prompt and deleted only the second controlled filler file:

```cmd
del D:\LAB-04-FILLER-02.bin
```

The `del` command displayed no output because it completed without an error.

Available space was checked again:

```cmd
fsutil volume diskfree D:
```

The result showed:

```text
Total free bytes: 14,172,160
```

![Free space restored](../screenshots/LAB-04/05-free-space-restored.png)

### 11. Validate the Save Operation

Returned to Notepad and repeated the save operation using:

`D:\Maya-Storage-Large-Test.txt`

The document saved without displaying an error.

The saved file was checked using:

```cmd
dir D:\Maya-Storage-Large-Test.txt
```

The destination file size was:

`322,893 bytes`

This matched the original document size exactly.

The file was then closed and reopened using:

```cmd
notepad D:\Maya-Storage-Large-Test.txt
```

The document opened successfully and contained all 3,000 test lines.

### 12. Validate the Correction After Restart

Restarted Windows and signed in again as `mtorres`.

Windows installed pending updates during the restart and returned to the desktop normally.

The saved file was checked again:

```cmd
dir D:\Maya-Storage-Large-Test.txt
```

The results confirmed that:

- `LAB-DATA` remained mounted as `D:`
- The document remained available
- The file retained its size of `322,893` bytes
- Available storage space remained present

The document was reopened:

```cmd
notepad D:\Maya-Storage-Large-Test.txt
```

It loaded successfully with all 3,000 lines intact.

![Post-restart file validation](../screenshots/LAB-04/06-post-restart-file-validation.png)

### 13. Perform Final Storage Cleanup

The first correction restored enough space to prove that storage capacity was the root cause.

However, leaving only approximately 13.8 MB available would not represent a healthy final state for the volume.

After completing the functional and restart validations, the remaining controlled filler file was deleted:

```cmd
del D:\LAB-04-FILLER-01.bin
```

The volume was checked again:

```cmd
fsutil volume diskfree D:
```

The final result showed:

```text
Total bytes:       1,182,789,632
Total free bytes:  1,163,829,248
Used bytes:           14,766,080
Reserved bytes:        4,194,304
```

The volume contents were reviewed again:

```cmd
dir D:\ /a /-c
```

The final review confirmed:

- Both controlled filler files had been removed
- `Maya-Storage-Large-Test.txt` remained intact at `322,893` bytes
- `Maya-Storage-Test.txt` remained intact at `36` bytes
- `$RECYCLE.BIN` remained present
- `System Volume Information` remained present
- Approximately 98% of the volume was available

The two user test files totaled:

```text
322,893 + 36 = 322,929 bytes
```

This matched the total user file size reported by the directory listing.

![Final storage cleanup validation](../screenshots/LAB-04/07-final-storage-cleanup-validation.png)

## Validation

### LAB-DATA Volume Created

The secondary disk was initialized using GPT and configured as the healthy NTFS volume `LAB-DATA (D:)`.

![LAB-DATA volume created](../screenshots/LAB-04/01-lab-data-volume-created.png)

### Volume Filled to Capacity

The controlled filler files reduced the available storage on `D:` to zero bytes.

![Volume full with zero bytes free](../screenshots/LAB-04/02-volume-full-zero-bytes-free.png)

### Save Failure Reproduced

Notepad displayed an insufficient disk space error when the larger document was saved to the full volume.

![Notepad insufficient disk space error](../screenshots/LAB-04/03-notepad-insufficient-disk-space-error.png)

### Root Cause Identified

The controlled filler files were identified as the files consuming the available volume capacity.

![Controlled filler files identified](../screenshots/LAB-04/04-controlled-filler-files-identified.png)

### Free Space Restored

Deleting the verified second filler file restored enough space to repeat the save operation successfully.

![Free space restored](../screenshots/LAB-04/05-free-space-restored.png)

### Correction Persisted After Restart

The saved document remained available, retained its full size, and reopened after Windows restarted.

![Post-restart file validation](../screenshots/LAB-04/06-post-restart-file-validation.png)

### Final Cleanup Confirmed

Both controlled filler files were removed while the user files and Windows system directories remained intact.

![Final storage cleanup validation](../screenshots/LAB-04/07-final-storage-cleanup-validation.png)

## Troubleshooting

### Virtual Disk Creation Initially Failed

The original virtual machine name contained:

`Name:`

When VirtualBox attempted to use that text in the virtual disk path, Windows rejected the colon character.

The virtual machine name was corrected before creating the disk again.

The existing virtual disk filenames were not renamed manually.

### Drive Letter D Was Initially Unavailable

The virtual optical drive occupied `D:`.

Its drive letter was changed temporarily so that the lab data volume could use `D:` consistently.

### The Small File Saved Despite Zero Reported Free Bytes

The first 36-byte Notepad document saved and reopened even though `fsutil` reported zero available bytes.

Because this did not reproduce the incident reliably, the test was redesigned using a larger 322,893-byte document.

The unexpected result was documented instead of being ignored.

### The First Large Save Attempt Appeared Ambiguous

One save attempt appeared to complete before Notepad displayed the insufficient disk space error.

The destination path was checked using:

```cmd
dir D:\Maya-Storage-Large-Test.txt
```

The result was:

```text
File Not Found
```

This provided objective confirmation that the destination file had not been created.

### The DEL Command Displayed No Success Message

The `del` command normally returns directly to the command prompt when a file is deleted successfully.

The absence of output did not indicate failure.

The result was verified by checking both the available free space and the directory contents.

### Functional Recovery Was Different From Final Cleanup

Deleting `LAB-04-FILLER-02.bin` restored enough space to validate the save operation and prove the root cause.

Deleting `LAB-04-FILLER-01.bin` afterward restored the volume to a healthy final state.

This demonstrated the difference between:

- Restoring immediate functionality
- Completing safe incident cleanup

## Lessons Learned

- Storage incidents should be reproduced on an isolated test volume, not the Windows system drive.
- Disk Management can initialize disks, create volumes, format file systems, and assign drive letters.
- GPT is the modern partitioning standard used by current Windows systems.
- `fsutil volume diskfree` reports total, used, reserved, and available storage in bytes.
- `fsutil file createnew` can create controlled files of a specific logical size.
- An application error may be caused by a dependency such as storage rather than by the application itself.
- Unexpected test results should be validated and documented instead of ignored.
- A larger test file may be required to reproduce a storage failure reliably.
- `dir` can confirm whether a file exists and whether its final size matches the source.
- `/a` allows hidden and system items to be reviewed before deleting anything.
- Controlled test files must be distinguished from user files and Windows system directories.
- The `del` command may complete successfully without displaying a confirmation message.
- File size comparison can confirm that a saved document was written completely.
- Closing and reopening a file validates readability after the write operation.
- Restart validation confirms that the volume, file, and correction persist.
- Restoring just enough capacity for a test does not necessarily leave a storage volume in a healthy final state.
- Final cleanup should remove controlled test data while preserving user and system content.
- Troubleshooting documentation should separate symptoms, checks, findings, actions, and results.

## Conclusion

The objectives of LAB-04 were completed successfully.

The reported Notepad save failure was reproduced on the isolated `LAB-DATA (D:)` volume after controlled files consumed all available storage space.

The investigation confirmed that Notepad was functioning normally and that the root cause was insufficient capacity on the destination volume.

A verified controlled filler file was removed, restoring enough space for the larger document to save successfully. The document retained its full size of `322,893` bytes, reopened with all 3,000 lines intact, and remained available after restarting Windows.

The remaining controlled filler file was then removed as part of final cleanup. The user files and Windows system directories remained intact, and the volume finished with `1,163,829,248` available bytes.

This lab demonstrated practical Windows storage configuration, controlled incident reproduction, application-versus-storage diagnosis, safe data removal, file integrity validation, restart verification, final cleanup, and professional incident documentation.
