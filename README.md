# IT Support Lab

Hands-on Windows administration, networking, troubleshooting, and help desk support labs documented through technical reports and simulated service tickets.

## About This Repository

This repository documents my practical development in entry-level IT support and systems administration.

Each lab includes:

- A realistic support scenario
- Manual inspection and troubleshooting
- Technical explanations
- Commands and administrative tools
- Validation results
- Security and privacy considerations
- A related service request documented through GitHub Issues

The objective is to demonstrate not only which tools were used, but also how the results were interpreted and documented.

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Client workstation | `LAB-WS01` |
| Operating system | Windows 11 Enterprise Evaluation |
| Network mode | NAT |
| Primary administrative account | `ITLabAdmin` |
| Ticketing simulation | GitHub Issues |
| Documentation format | Markdown |

## Completed Labs

### LAB-01 — Windows Workstation Inventory

Prepared and validated a newly installed Windows 11 workstation by reviewing:

- Operating system and hardware information
- Device naming convention
- Workgroup membership
- Local administrator privileges
- Device Manager and driver status
- DHCP, IPv4, gateway, and DNS configuration
- Gateway and Internet connectivity
- DNS name resolution
- Windows Update history
- Windows activation
- Security and privacy considerations

**Documentation:**  
[View LAB-01 — Windows Workstation Inventory](labs/LAB-01-Windows-Workstation-Inventory.md)

**Evidence:**  
[View LAB-01 Evidence Gallery](screenshots/LAB-01/README.md)

**Related service request:**  
[View SR-001 — New Workstation Setup and Baseline Inventory](/juanko31359/IT-Support-Lab/issues/1)

### LAB-02 — Local Users, Groups, and Folder Permissions

Provisioned a standard local user account and configured secure access to a designated support folder by:

- Creating the local account `mtorres`
- Creating and managing the local group `Support-Team`
- Assigning access through group membership
- Reviewing and correcting inherited NTFS permissions
- Granting `Modify` permission without `Full control`
- Testing file creation, editing, renaming, and deletion
- Confirming that administrative access was denied
- Documenting and closing the related service request

**Documentation:**  
[View LAB-02 — Local Users, Groups, and Folder Permissions](labs/LAB-02-Local-Users-Groups-and-Folder-Permissions.md)

**Evidence:**  
[View LAB-02 Evidence Gallery](screenshots/LAB-02/README.md)

**Related service request:**  
[View SR-002 — Provision Local User and Folder Access](/juanko31359/IT-Support-Lab/issues/2)

### LAB-03 — Windows Processes, Services, and Startup Troubleshooting

Investigated and resolved an unexpected startup application and an unavailable Windows service by:

- Reproducing the reported startup behavior
- Identifying `Notepad.exe` and its process ID
- Reviewing Startup apps and the user's Startup folder
- Distinguishing the executable from the mechanism that launched it
- Querying the Print Spooler state and configuration
- Restoring the service to `AUTO_START`
- Confirming that the service remained `RUNNING` after restarting Windows
- Documenting and closing the related incident

**Documentation:**  
[View LAB-03 — Windows Processes, Services, and Startup Troubleshooting](labs/LAB-03-Windows-Processes-Services-and-Startup-Troubleshooting.md)

**Evidence:**  
[View LAB-03 Evidence Gallery](screenshots/LAB-03/README.md)

**Related incident:**  
[View INC-003 — Unexpected Startup Application and Print Service Failure](/juanko31359/IT-Support-Lab/issues/3)

### LAB-04 — Storage and Application Troubleshooting

Investigated and resolved an application save failure caused by insufficient storage space by:

- Preparing an isolated secondary virtual disk
- Initializing the disk using GPT
- Creating the NTFS volume `LAB-DATA (D:)`
- Recording the original storage capacity and available space
- Creating controlled filler files to reproduce the incident
- Confirming that Notepad was functioning correctly
- Identifying insufficient storage as the root cause
- Restoring space without deleting user files or system directories
- Validating the saved document's size and contents
- Confirming that the correction persisted after restarting Windows
- Removing the remaining controlled test data
- Restoring the volume to approximately 98% available space
- Documenting and closing the related incident

**Documentation:**  
[View LAB-04 — Storage and Application Troubleshooting](labs/LAB-04-Storage-and-Application-Troubleshooting.md)

**Evidence:**  
[View LAB-04 Evidence Gallery](screenshots/LAB-04/README.md)

**Related incident:**  
[View INC-004 — Application Cannot Save File Due to Insufficient Disk Space](/juanko31359/IT-Support-Lab/issues/4)

## Skills Practiced

- Windows 11 workstation setup
- System and hardware inventory
- Computer naming conventions
- Local users and groups
- Administrative privileges and UAC
- Device and driver inspection
- IPv4 addressing
- DHCP
- Default gateways
- DNS resolution
- ICMP connectivity testing
- Command Prompt
- `ipconfig`
- `ping`
- `nslookup`
- `tracert`
- Windows Update review
- Technical documentation
- Help desk ticket workflow
- GitHub Issues
- Markdown
- Privacy-aware documentation
- Disk Management
- GPT and NTFS volume configuration
- Windows storage capacity analysis
- `fsutil`
- Controlled storage incident reproduction
- File integrity validation
- Application-versus-storage troubleshooting

## Support Workflow

The labs use the following support process:

1. Receive or define a service request
2. Identify the affected asset and scope
3. Inspect the current configuration
4. Perform technical validation
5. Record actions and results
6. Confirm success criteria
7. Document the resolution
8. Close the ticket as completed

## Learning Roadmap

- [x] Windows workstation baseline and inventory
- [x] Local users, groups, and permissions
- [x] Windows processes, services, and startup troubleshooting
- [x] Storage and application troubleshooting
- [ ] Network connectivity troubleshooting
- [ ] Windows Event Viewer
- [ ] PowerShell system administration
- [ ] Shared folders and NTFS permissions
- [ ] Windows Server and Active Directory
- [ ] Domain users, groups, and Group Policy
- [ ] Multi-device help desk scenarios

## Security and Privacy

Public documentation intentionally excludes:

- Passwords
- Product keys
- Product IDs
- Device IDs
- Personal email addresses
- Unnecessary MAC addresses
- Credentials and confidential information

## Author

**Juan Carlos Lozano Cortes**

Bilingual English–Spanish IT support and cybersecurity learner building practical experience through documented hands-on labs.
