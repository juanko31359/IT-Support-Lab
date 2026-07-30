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

**Related service request:**  
[View SR-001 — New Workstation Setup and Baseline Inventory](/juanko31359/IT-Support-Lab/issues/1)

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
- [ ] Local users, groups, and permissions
- [ ] Windows processes, services, and startup troubleshooting
- [ ] Storage and application troubleshooting
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
