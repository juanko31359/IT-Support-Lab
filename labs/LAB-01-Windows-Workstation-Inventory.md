# LAB-01 — Windows Workstation Inventory

## Objective

Prepare and validate a newly installed Windows 11 workstation by collecting a technical baseline, reviewing its configuration, testing network connectivity, and documenting the results.

This lab simulates the initial inspection performed by an IT support technician before assigning a workstation to a user.

## Related Service Request

- Ticket: `SR-001 — New Workstation Setup and Baseline Inventory`
- GitHub Issue: `#1`
- Final status: Completed

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Workstation name | `LAB-WS01` |
| Operating system | Windows 11 Enterprise Evaluation |
| Windows version | 25H2 |
| OS build | 26200.8875 |
| Architecture | 64-bit operating system, x64-based processor |
| Assigned memory | 8 GB |
| Virtual storage | 80 GB |
| Network mode | NAT |
| Primary account | `ITLabAdmin` |
| Account type | Local administrator |

## Tools Used

- Windows Settings
- System Properties
- Local Users and Groups
- Device Manager
- Windows Update
- Command Prompt
- `ipconfig`
- `ping`
- `nslookup`
- `tracert`

## 1. System and Hardware Inventory

The workstation information was reviewed through:

```text
Settings → System → About
```

The original automatically generated device name was:

```text
DESKTOP-N1FESAD
```

It was renamed to:

```text
LAB-WS01
```

The naming convention means:

- `LAB`: laboratory environment
- `WS`: workstation
- `01`: first workstation

A consistent naming convention helps technicians identify and manage devices more efficiently.

### Hardware Baseline

| Item | Result |
|---|---|
| Processor presented to guest | 12th Gen Intel Core i9-12900H |
| Assigned RAM | 8 GB |
| Virtual disk | 80 GB |
| Display adapter | VirtualBox Graphics Adapter (WDDM) |
| Network adapter | Intel PRO/1000 MT Desktop Adapter |

The processor name is exposed from the host computer, while the memory, storage, graphics, and network resources available to Windows are controlled by the virtual machine configuration.

## 2. Workgroup and Domain Membership

System Properties confirmed:

```text
Computer name: LAB-WS01
Workgroup: WORKGROUP
Domain membership: None
```

`LAB-WS01` is currently a standalone workstation.

In a workgroup, each computer manages its own local users and passwords. In a domain environment, accounts and policies can be centrally managed through services such as Active Directory.

## 3. Local Account Verification

The following tool was opened through the Run dialog:

```text
lusrmgr.msc
```

`lusrmgr.msc` opens Local Users and Groups Manager, a Microsoft Management Console tool used to manage local accounts and groups.

The primary account was verified as:

```text
Username: ITLabAdmin
Account scope: Local
Group membership: Administrators
Privilege level: Local administrator
```

Membership in the `Administrators` group grants administrative privileges on this workstation.

Windows still uses User Account Control, or UAC, to request confirmation when an application needs elevated permissions.

## 4. Device and Driver Inspection

Device Manager was reviewed for:

- Unknown devices
- Yellow warning indicators
- Disabled devices
- Missing drivers
- Hardware errors

### Result

```text
Unknown devices: None observed
Disabled devices: None observed
Driver warnings: None observed
```

The virtual disk, display adapter, and network adapter were recognized correctly.

### Identified Virtual Devices

| Device category | Device |
|---|---|
| Disk drive | VBOX HARDDISK |
| Display adapter | VirtualBox Graphics Adapter (WDDM) |
| Network adapter | Intel PRO/1000 MT Desktop Adapter |

These devices are presented to Windows by VirtualBox and do not provide direct access to the physical disk, graphics card, or network adapter of the host computer.

## 5. Network Configuration

The graphical Ethernet properties and the following command were used:

```cmd
ipconfig /all
```

`ipconfig` displays the Internet Protocol configuration assigned to Windows. The `/all` parameter displays detailed adapter, DHCP, DNS, gateway, and lease information.

### IPv4 Baseline

| Setting | Value |
|---|---|
| IPv4 address | `10.0.2.15` |
| Subnet mask | `255.255.255.0` |
| Default gateway | `10.0.2.2` |
| DHCP server | `10.0.2.2` |
| DHCP enabled | Yes |
| DNS assignment | Automatic |
| Network profile | Public |
| VirtualBox mode | NAT |

The address `10.0.2.15` is a private address used inside the virtual network.

DHCP automatically supplied the IP address, subnet mask, default gateway, and DNS configuration.

## 6. Connectivity Testing

Connectivity was tested in layers to isolate possible failures.

### Test 1 — Default Gateway

```cmd
ping 10.0.2.2
```

Result:

```text
Packets sent: 4
Packets received: 4
Packet loss: 0%
```

This confirmed communication between `LAB-WS01` and the VirtualBox gateway.

### Test 2 — External IP Address

```cmd
ping 8.8.8.8
```

Result:

```text
Packets sent: 4
Packets received: 4
Packet loss: 0%
```

This confirmed that the workstation could reach an external IP address without depending on DNS name resolution.

### Test 3 — DNS Resolution

```cmd
nslookup microsoft.com
```

The DNS server successfully translated `microsoft.com` into valid IPv4 and IPv6 addresses.

This confirmed that DNS name resolution was working.

### Test 4 — Route Trace

```cmd
tracert 8.8.8.8
```

The destination was reached successfully. The NAT-based virtual environment exposed only the final destination during this test, so intermediate router visibility was limited.

### Diagnostic Sequence

The tests were performed in this order:

```text
1. Verify the assigned network configuration
2. Test the default gateway
3. Test an external IP address
4. Test DNS name resolution
5. Observe the network route
```

This sequence helps identify whether a connection failure is caused by:

- Missing IP configuration
- Local network or gateway failure
- Internet connectivity failure
- DNS failure
- Routing limitations

## 7. Windows Update Review

Windows Update reported:

```text
Required updates: Current
Failed updates visible: None
Pending restart: None
```

The update history included successfully installed:

- Windows security updates
- Critical updates
- .NET Framework security updates
- Microsoft Defender security intelligence updates
- Windows Security platform updates
- A Microsoft audio processing driver
- Windows Malicious Software Removal Tool

An optional Preview update was available but intentionally deferred to preserve a stable workstation baseline.

## 8. Activation Review

The Activation page confirmed:

```text
Edition: Windows 11 Enterprise Evaluation
Activation state: Active
License type: Time-limited evaluation
Evaluation period: 90 days
```

No product key, Product ID, or Device ID was included in the public documentation.

## Security and Privacy Practices

The following information was intentionally excluded:

- Passwords
- Product keys
- Product ID
- Device ID
- Personal email addresses
- MAC addresses
- Unnecessary system identifiers

Technical documentation should contain enough evidence to demonstrate the work without exposing credentials or unnecessary identifying information.

## Final Results

| Validation area | Status |
|---|---|
| Device naming | Passed |
| Operating system inventory | Passed |
| Local account verification | Passed |
| Device and driver inspection | Passed |
| DHCP configuration | Passed |
| Gateway connectivity | Passed |
| Internet connectivity | Passed |
| DNS resolution | Passed |
| Windows Update review | Passed |
| Windows activation | Passed |

## Conclusion

`LAB-WS01` was successfully prepared and validated as a stable Windows 11 workstation.

This lab demonstrated how an IT support technician can establish a workstation baseline, inspect local accounts and devices, validate network connectivity, review system maintenance, protect sensitive information, and document the completed work through a service request.
