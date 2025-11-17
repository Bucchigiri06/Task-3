<img width="1910" height="983" alt="image" src="https://github.com/user-attachments/assets/9dbadf08-0c7d-47c4-bba5-d2208c4f60f7" />



# 📄 **Task 3 – Vulnerability Scan Report (Using Nmap)**

### **Tool Used:** Nmap 7.98

### **Target:** HIDDEN (Local Windows Machine)

### **Date:** 17 November 2025

---

# 🔍 **1. Objective**

To perform a basic vulnerability assessment on my PC using the free tool **Nmap**, identify open ports, exposed services, and potential vulnerabilities.

---

# 🛠 **2. Scan Command Used**

```
nmap -sV HIDDEN
```

This performs version detection and identifies services running on open ports.

---

# 📊 **3. Scan Results**

```
Starting Nmap 7.98 ( https://nmap.org ) at 2025-11-17 11:36 +0530
Nmap scan report for HIDDEN
Host is up (0.0011s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
2869/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
3306/tcp open  mysql         MySQL (unauthorized)
3389/tcp open  ms-wbt-server
Service Info: OS: Windows
```

---

# 🧩 **4. Security Analysis of Each Open Port**

### **✔ Port 135 – MSRPC**

* Used for Windows remote procedure calls.
* Attack risk: Remote code execution (historical worm vectors).
* Severity: **Medium**

---

### **✔ Port 139 – NetBIOS**

* Used for file & printer sharing.
* Attack risk: Information leakage, SMB attacks.
* Severity: **Medium**

---

### **✔ Port 445 – SMB (Microsoft-DS)**

* Used for Windows file sharing.
* Historically targeted by:

  * WannaCry
  * EternalBlue
* Severity: **High**

---

### **✔ Port 2869 – HTTPAPI (UPnP / SSDP)**

* Used by Windows for network device discovery.
* Attack risk: UPnP vulnerabilities, information disclosure.
* Severity: **Medium**

---

### **❗ Port 3306 – MySQL (Unauthorized)**

* MySQL service is running
* Nmap couldn’t authenticate:

  * Means password may be weak
  * Or MySQL is exposed unnecessarily

➡ **Major issue if not intentionally installed**

* Severity: **High**

---

### **❗ Port 3389 – Remote Desktop (RDP)**

* RDP exposed on LAN
* RDP is frequently attacked (bruteforce, credential theft).
* Without Network Level Authentication (NLA), it’s vulnerable.
* Severity: **High**

---

# 🚨 **5. Overall Vulnerability Summary**

| Port | Service      | Severity | Risk Summary                       |
| ---- | ------------ | -------- | ---------------------------------- |
| 135  | MSRPC        | Medium   | Windows remote service exposed     |
| 139  | NetBIOS      | Medium   | Info leakage, SMB enumeration      |
| 445  | SMB          | **High** | Past major RCE vulnerabilities     |
| 2869 | UPnP HTTPAPI | Medium   | Exposes device info                |
| 3306 | MySQL        | **High** | Unauthenticated DB service running |
| 3389 | RDP          | **High** | Remote login surface exposed       |

---

# 🎯 **6. Recommended Fixes**

### ✔ **1. Disable SMBv1**

WannaCry-style attacks target SMBv1.

Run PowerShell:

```
Disable-WindowsOptionalFeature -Online -FeatureName smb1protocol
```

---

### ✔ **2. Close Port 139 and 445 if not needed**

* Go to: Windows Features
* Turn off:

  * File and Printer Sharing
  * SMB 1.0 (if still enabled)

---

### ✔ **3. Disable RDP (Port 3389) if not required**

Settings → System → Remote Desktop → Turn off
OR
Ensure:

* NLA Enabled
* Strong password
* Firewall limits access

---

### ✔ **4. MySQL – If you did NOT install it**

Disable the service:

```
services.msc
```

Find **MySQL** → Stop → Disable

If you installed it intentionally:

* Set strong password
* Bind MySQL only to localhost (my.ini)

---

### ✔ **5. Block these ports on Windows Firewall**

* 135
* 139
* 445
* 3306
* 3389

unless required.

---

# 📝 **7. Conclusion**

The Nmap vulnerability scan revealed that several high-risk and medium-risk ports are open on the system, especially **SMB (445), MySQL (3306), and RDP (3389)**. These services can expose the system to unauthorized access, malware propagation, or brute-force attacks.
By applying the recommended fixes such as disabling unnecessary services, closing ports, and enabling security features, the overall security posture of the system can be significantly improved.

---

If you want, I can also create a **formatted README.md** or help you create the **GitHub repo structure**.
