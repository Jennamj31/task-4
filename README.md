# task-4
# 🔐 Task 4: Setup and Use a Firewall on Windows

## 🎯 Objective
Configure and test basic firewall rules to allow or block traffic using Windows Defender Firewall and PowerShell.

---

## 🛠 Tools Used
- **Operating System**: Windows 11
- **Firewall Tool**: Windows Defender Firewall with Advanced Security (`wf.msc`)
- **Command-line Utility**: PowerShell (`Test-NetConnection`)

---

##  Implementation

### 1️⃣ Open firewall configuration tool
- Opened `wf.msc` via **Windows Search** or **Run Dialog** (`Win + R → wf.msc`).
- Alternatively: `Control Panel → System and Security → Windows Defender Firewall → Advanced Settings`.

### 2️⃣ List current firewall rules
- Navigated through **Inbound Rules** and **Outbound Rules** in the left panel of the firewall console.
- Observed existing system and user-defined rules.

### 3️⃣ Add a rule to block inbound traffic on port 23 (Telnet)
- Inbound Rule → New Rule → Port → TCP → Specific local port: `23` → Block the connection → Applied to all profiles.
- Named the rule: `Block Telnet Port 23`

### 4️⃣ Test the rule (locally and remotely)
```powershell
Test-NetConnection -ComputerName 127.0.0.1 -Port 23
Test-NetConnection -ComputerName <remote-IP> -Port 23
```
- ✅ Output showed `TcpTestSucceeded: False`, confirming that port 23 was blocked.
- ❌ Remote test failed due to either firewall on remote or Telnet service not running.

### 5️⃣ Add rule to allow SSH (port 22)
- Inbound Rule → New Rule → Port → TCP → Specific local port: `22` → Allow the connection → Applied to all profiles.
- Named the rule: `Allow SSH Port 22`
- Verified:
```powershell
Test-NetConnection -ComputerName 127.0.0.1 -Port 22
```
- ⚠️ SSH not running on Windows by default, so `TcpTestSucceeded` was still false — but the port was not blocked.

### 6️⃣ Remove the test block rule to restore original state
- Located `Block Telnet Port 23` in Inbound Rules → Right-click → **Delete**.
- Rule was removed, restoring default behavior for port 23.

### 7️⃣ Document commands or GUI steps used
All actions were done via GUI (`wf.msc`) and PowerShell:
```powershell
Test-NetConnection -ComputerName 127.0.0.1 -Port 23
Test-NetConnection -ComputerName 127.0.0.1 -Port 22
```
GUI Steps:
- `wf.msc` → Inbound Rules → New Rule → Port → TCP → Block/Allow → Name → Finish

### 8️⃣ Summary: How firewall filters traffic
Firewalls work by inspecting **incoming and outgoing network traffic** based on defined rules.  
- A **block rule** drops packets for specified ports/protocols.
- An **allow rule** permits traffic, but the destination port must still have a listening service.
- Even if a port is open, without a service (like Telnet or SSH) listening, connection attempts will still fail.


---

## ✅ Conclusion
- Successfully created and tested inbound firewall rules on Windows.
- Demonstrated how firewall rules impact network connectivity.
- Removed test rules to maintain original system behavior.
- Learned how to diagnose blocked traffic using PowerShell.

