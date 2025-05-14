
# 🛡️ QRadar 101 Lab Investigation Report

This report documents the findings from the QRadar 101 lab focused on analyzing a simulated security incident using IBM QRadar Community Edition.

---

## 🔍 Investigation Questions & Answers

### 1. How many log sources are available?
**Answer:** `15`  
**How it was determined:**  
- Grouped log activity by **Log Source**.
- Counted unique entries in the table at the bottom of the view.

![1](https://github.com/user-attachments/assets/d6764108-5980-43af-94fe-95dcf112912c)

---

### 2. What is the IDS software used to monitor the network?
**Answer:** `Suricata`  
**How it was determined:**  
- Identified log source **SO-Suricata**.
- Logged event type: `NIDS Alert`, confirming it is the network intrusion detection system.




---

### 3. What is the domain name used in the network?
**Answer:** `hackdefend.local`  
**How it was determined:**  
- Found in the event payload from `DC.hackdefend.local`.
- Example: `Computer=DC.hackdefend.local`

![2](https://github.com/user-attachments/assets/6f679f19-a6a9-454e-b81b-062c19d9b8c1)

---

### 4. Multiple IPs were communicating with the malicious server. One of them ends with "20". Provide the full IP.
**Answer:** `192.168.20.20`  
**How it was determined:**  
- This IP was found in DNS and connection logs interacting with external IPs.
- Confirmed as a system communicating with the attacker.





---

### 5. What is the SID of the most frequent alert rule in the dataset?
**Answer:** `2027865`  
**How it was determined:**  
- Grouped Suricata events by `RULE SID (custom)`.
![3](https://github.com/user-attachments/assets/f2a16ad3-1847-44ad-a7da-1cc5a3984975)
- SID `2027865` had the highest event count: `72`.

![4](https://github.com/user-attachments/assets/4cd359ed-9965-4d0b-88b0-338965fd1c6b)

---

### 6. What is the attacker’s IP address?
**Answer:** `192.20.80.25`  
**How it was determined:**  
- Found in the **Offenses tab** as the **Offense Source** of an alert titled:
  > “Excessive Firewall Denies involving Source IP”

![5](https://github.com/user-attachments/assets/32165cbe-2df1-4ce7-b437-1868069b0733)



---

### 7. The attacker was searching for data belonging to one of the company's projects. What is the name of the project?
**Answer:** `Project48`  
**How it was determined:**  
- In PowerShell logs, attacker used `Get-ChildItem` command with:
  > `Filter: Project48`  
- Indicates explicit search for files related to the project.

![6](https://github.com/user-attachments/assets/56a3afc5-146a-451d-b557-aa2d552875d7)
![7](https://github.com/user-attachments/assets/3f90a6ca-6144-44b8-8731-b05bf69e49f2)

---

### 8. What is the IP address of the first infected machine?
**Answer:** `192.168.10.15`  
**How it was determined:**  
- Filtered events with `Source IP = 192.20.80.25` and sorted by timestamp.
- First observed malicious traffic was to `192.168.10.15`.

![8](https://github.com/user-attachments/assets/1be6ac57-f904-46cc-a6ee-d82b9a4b1878)


---

### 9. What is the username of the infected employee using 192.168.10.15?
**Answer:** `nour`  
**How it was determined:**  
- Filtered Event ID `4624` (login success) involving `192.168.10.15`.
- Identified successful logons by user `nour`.

![9](https://github.com/user-attachments/assets/f19e91ce-0c1a-42c4-bff1-aff326a63cd9)


---

### 10. Hackers do not like logging. What logging was the attacker checking to see if enabled?
**Answer:** `PowerShell Logging`  
**How it was determined:**  
- Events showed the attacker (`nour`) repeatedly initiated PowerShell sessions.
- Included `Module Logging Command Invocation`, consistent with assessing logging state.

![10](https://github.com/user-attachments/assets/52942312-1f2d-4b16-a257-73fda46be27d)


---

## ✅ Summary

- **Attacker IP:** `192.20.80.25`  
- **First Infected Host:** `192.168.10.15`  
- **Compromised User:** `nour`  
- **Targeted Project:** `Project48`  
- **Security Insight:** Attackers often probe PowerShell logging early in their operation. This should be monitored for signs of living-off-the-land behavior.

---
