# LAB-02 — Local Users, Groups, and Folder Permissions

## Objective

Provision a standard local user account, assign access through group membership, configure NTFS folder permissions, and verify that the user can work with designated files without receiving administrative privileges.

## Scenario

A new support assistant, Maya Torres, requires a local account on workstation `LAB-WS01`.

The user must be able to access and modify files inside:

`C:\LabData\Support`

Access must be granted through the local group `Support-Team`, rather than assigning permissions directly to the user.

The account must remain a standard user without administrative privileges.

## Lab Environment

- Workstation: `LAB-WS01`
- Operating system: Windows 11 Enterprise Evaluation
- Account created: `mtorres`
- Display name: Maya Torres
- Local group: `Support-Team`
- Folder: `C:\LabData\Support`
- Permission assigned: `Modify`
- Related ticket: `SR-002 — Provision Local User and Folder Access`

## Skills Practiced

- Creating local Windows accounts
- Distinguishing standard users from administrators
- Creating and managing local groups
- Assigning access through group membership
- Reviewing inherited NTFS permissions
- Applying the principle of least privilege
- Testing file and administrative access
- Documenting a service request in GitHub

## Procedure

### 1. Create the Local User

Opened Local Users and Groups by running `lusrmgr.msc`.

Created the following local account:

- Username: `mtorres`
- Full name: Maya Torres
- Description: Support Assistant - Lab account
- Account type: Standard user

The account was configured to require a password change at the first sign-in.

### 2. Create the Local Group

Created the local group `Support-Team` with the description:

`Local group for support team folder access`

Added `mtorres` as a member of `Support-Team`.

### 3. Create the Support Folder

Created the following folder:

`C:\LabData\Support`

### 4. Review Existing NTFS Permissions

Reviewed the folder's Security properties.

The folder initially inherited permissions for:

- `Authenticated Users`
- `Users`
- `SYSTEM`
- `Administrators`

The `Authenticated Users` and `Users` entries allowed users to modify the folder independently of membership in `Support-Team`.

### 5. Correct the Folder Permissions

Disabled permission inheritance and converted the inherited permissions into explicit permissions.

Removed:

- `Authenticated Users`
- `Users`

Retained:

- `SYSTEM`
- `Administrators`

Added `Support-Team` and granted the `Modify` permission.

`Full control` was not granted.

### 6. Sign In as the New User

Signed out of the administrator account and signed in as Maya Torres.

The user successfully changed the temporary password during the first sign-in.

## Validation

### Local Account Created

The local account `mtorres` was created for Maya Torres.

![Local user account created](../screenshots/LAB-02/01-local-user-mtorres-created.png)

### Group Membership

The account belongs to `Users` and `Support-Team`, but not to `Administrators`.

![Local user group memberships](../screenshots/LAB-02/02-local-user-group-memberships.png)

### NTFS Permissions

The `Support-Team` group was granted `Modify` permission on `C:\LabData\Support`.

`Full control` was not granted.

![Support folder NTFS permissions](../screenshots/LAB-02/03-support-folder-ntfs-permissions.png)

### File Access Test

The user successfully created, edited, renamed, and deleted files inside the Support folder.

![File access test completed](../screenshots/LAB-02/04-file-access-test-completed.png)

### Administrative Access Test

The `whoami` command confirmed the active account as:

`lab-ws01\mtorres`

The `net session` command returned:

`System error 5 has occurred. Access is denied.`

This confirmed that the account did not have elevated administrative privileges.

![Standard user administrative access denied](../screenshots/LAB-02/05-standard-user-admin-access-denied.png)
