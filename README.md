README – macOS PF Firewall Configuration and Testing
Objective
Configure and test basic firewall rules on macOS to allow or block specific network traffic using PF (Packet Filter).

Tools Used
macOS Terminal

PF (Packet Filter) – built-in macOS firewall for packet-level filtering

nc (netcat) – for testing connections

Steps
1. Check PF status
bash
Copy code
sudo pfctl -s info
If inactive, PF will start automatically after loading rules.

2. Backup default PF configuration
bash
Copy code
sudo cp /etc/pf.conf /etc/pf.conf.backup
3. Edit PF rules
Open the config file:

bash
Copy code
sudo nano /etc/pf.conf
Add these lines near the top:

pgsql
Copy code
block in proto tcp from any to any port 23
pass in proto tcp from any to any port 22
Block inbound TCP traffic on port 23 (Telnet)

Allow inbound TCP traffic on port 22 (SSH)

4. Validate syntax
bash
Copy code
sudo pfctl -nf /etc/pf.conf
No output = no syntax errors.

5. Load new rules and enable PF
bash
Copy code
sudo pfctl -f /etc/pf.conf
sudo pfctl -e
6. Verify loaded rules
bash
Copy code
sudo pfctl -sr
Expected output:

pgsql
Copy code
block drop in proto tcp from any to any port = telnet
pass in proto tcp from any to any port = ssh
7. Test Telnet block
bash
Copy code
nc -vz 127.0.0.1 23
Expected result:

pgsql
Copy code
nc: connect to 127.0.0.1 port 23 (tcp) failed: Connection refused
8. Remove Telnet block
Open config:

bash
Copy code
sudo nano /etc/pf.conf
Delete:

pgsql
Copy code
block in proto tcp from any to any port 23
Reload PF:

bash
Copy code
sudo pfctl -f /etc/pf.conf
9. Deliverables
Screenshot of:

bash
Copy code
sudo pfctl -sr
showing both the block and allow rules in place.

Summary
PF filters network traffic at the packet level based on rules in /etc/pf.conf.
In this task:

Port 23 was blocked to prevent Telnet access.

Port 22 was allowed to maintain SSH connectivity.

Rules were tested using nc and then removed to restore normal network access.
