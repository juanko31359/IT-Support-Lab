# LAB-02 Evidence

Visual evidence for **LAB-02 — Local Users, Groups, and Folder Permissions**.

The account and employee information used in this lab are fictitious. Passwords and other credentials were not included in the public documentation.

## 1. Local User Created

The local account `mtorres` was created for the fictitious support assistant Maya Torres.

![Local user mtorres created](01-local-user-mtorres-created.png)

## 2. User Group Memberships

The account belongs to the local groups `Users` and `Support-Team`.

The account is not a member of `Administrators`, confirming that it remains a standard user.

![Local user group memberships](02-local-user-group-memberships.png)

## 3. Support Folder NTFS Permissions

The local group `Support-Team` was granted `Modify` permission on:

`C:\LabData\Support`

`Full control` was not granted.

![Support folder NTFS permissions](03-support-folder-ntfs-permissions.png)

## 4. File Access Test

The user successfully created, edited, renamed, and deleted test files inside the Support folder.

The remaining file confirms that the account could create, modify, and rename content.

![File access test completed](04-file-access-test-completed.png)

## 5. Administrative Access Test

The `whoami` command confirmed that the active session belonged to:

`lab-ws01\mtorres`

The `net session` command returned:

`System error 5 has occurred. Access is denied.`

This confirmed that the account did not have elevated administrative privileges.

![Standard user administrative access denied](05-standard-user-admin-access-denied.png)
