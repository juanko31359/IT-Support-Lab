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
