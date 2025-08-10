<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/47ba326a-e79d-42e0-ac13-608c5a3ff57e" />

# **README – macOS PF Firewall Configuration and Testing (with Outputs)**

## **Objective**

Configure and test basic firewall rules on macOS to allow or block specific network traffic using **PF (Packet Filter)**.

---

## **Tools Used**

* **macOS Terminal**
* **PF (Packet Filter)** – built-in macOS firewall for packet-level filtering
* **`nc` (netcat)** – for testing connections

---

## **Steps & Expected Outputs**

### **1. Check PF status**

```bash
$ sudo pfctl -s info
Password:
Status: Disabled
Debug: Urgent
Hostid: 0x00000000
Checksum: No
State Table Entries: 0
```

---

### **2. Backup default PF configuration**

```bash
$ sudo cp /etc/pf.conf /etc/pf.conf.backup
```

*(No output if successful)*

---

### **3. Edit PF rules**

```bash
$ sudo nano /etc/pf.conf
```

Add:

```
block in proto tcp from any to any port 23
pass in proto tcp from any to any port 22
```

*(No output until you save and exit nano)*

---

### **4. Validate syntax**

```bash
$ sudo pfctl -nf /etc/pf.conf
```

*(No output if syntax is correct)*

---

### **5. Load new rules and enable PF**

```bash
$ sudo pfctl -f /etc/pf.conf
$ sudo pfctl -e
pf enabled
```

---

### **6. Verify loaded rules**

```bash
$ sudo pfctl -sr
block drop in proto tcp from any to any port = telnet
pass in proto tcp from any to any port = ssh
```

---

### **7. Test Telnet block**

```bash
$ nc -vz 127.0.0.1 23
nc: connectx to 127.0.0.1 port 23 (tcp) failed: Connection refused
```

---

### **8. Remove Telnet block**

```bash
$ sudo nano /etc/pf.conf
```

(Delete the Telnet block line)

```bash
$ sudo pfctl -f /etc/pf.conf
```

*(No output if successful)*

---

### **9. Final rules check**

```bash
$ sudo pfctl -sr
pass in proto tcp from any to any port = ssh
```

---

## **Summary**

PF filters network traffic at the packet level using `/etc/pf.conf`.
In this task:

* Port **23** was blocked to prevent Telnet connections.
* Port **22** was explicitly allowed for SSH.
* Testing with `nc` confirmed the block worked.
* Rule removal restored normal connectivity.
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/47ba326a-e79d-42e0-ac13-608c5a3ff57e" />

