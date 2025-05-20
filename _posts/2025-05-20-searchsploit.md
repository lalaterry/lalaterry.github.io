---
layout: post
title: "searchploit"
date: 2025-05-20 00:00:00 +0800
author: [001]
categories: [工具]
tags: [tools]
---

# searchploit update
```
searchploit -u
```

# Basic Search
```
kali@kali:~$ searchsploit afd windows local
--------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                         |  Path
--------------------------------------------------------------------------------------- ---------------------------------
Microsoft Windows (x86) - 'afd.sys' Local Privilege Escalation (MS11-046)              | windows_x86/local/40564.c
Microsoft Windows - 'afd.sys' Local Kernel (PoC) (MS11-046)                            | windows/dos/18755.c
Microsoft Windows - 'AfdJoinLeaf' Local Privilege Escalation (MS11-080) (Metasploit)   | windows/local/21844.rb
Microsoft Windows 7 (x64) - 'afd.sys' Dangling Pointer Privilege Escalation (MS14-040) | windows_x86-64/local/39525.py
Microsoft Windows 7 (x86) - 'afd.sys' Dangling Pointer Privilege Escalation (MS14-040) | windows_x86/local/39446.py
Microsoft Windows XP - 'afd.sys' Local Kernel Denial of Service                        | windows/dos/17133.c
Microsoft Windows XP/2003 - 'afd.sys' Local Privilege Escalation (K-plugin) (MS08-066) | windows/local/6757.txt
Microsoft Windows XP/2003 - 'afd.sys' Local Privilege Escalation (MS11-080)            | windows/local/18176.py
--------------------------------------------------------------------------------------- ---------------------------------
```

# Title Searching

```
kali@kali:~$ searchsploit -t oracle windows
--------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                         |  Path
--------------------------------------------------------------------------------------- ---------------------------------
Oracle 10g (Windows x86) - 'PROCESS_DUP_HANDLE' Local Privilege Escalation             | windows_x86/local/3451.c
Oracle 9i XDB (Windows x86) - FTP PASS Overflow (Metasploit)                           | windows_x86/remote/16731.rb
Oracle 9i XDB (Windows x86) - FTP UNLOCK Overflow (Metasploit)                         | windows_x86/remote/16714.rb
Oracle 9i XDB (Windows x86) - HTTP PASS Overflow (Metasploit)                          | windows_x86/remote/16809.rb
Oracle MySQL (Windows) - FILE Privilege Abuse (Metasploit)                             | windows/remote/35777.rb
Oracle MySQL (Windows) - MOF Execution (Metasploit)                                    | windows/remote/23179.rb
Oracle MySQL for Microsoft Windows - Payload Execution (Metasploit)                    | windows/remote/16957.rb
Oracle VirtualBox Guest Additions 5.1.18 - Unprivileged Windows User-Mode Guest Code Do| multiple/dos/41932.cpp
Oracle VM VirtualBox 5.0.32 r112930 (x64) - Windows Process COM Injection Privilege Esc| windows_x86-64/local/41908.txt
--------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Result
kali@kali:~$
```


# Copy To Folder

```
kali@kali:~$ searchsploit MS14-040
--------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                         |  Path
--------------------------------------------------------------------------------------- ---------------------------------
Microsoft Windows 7 (x64) - 'afd.sys' Dangling Pointer Privilege Escalation (MS14-040) | exploits/windows_x86-64/local/39525.py
Microsoft Windows 7 (x86) - 'afd.sys' Dangling Pointer Privilege Escalation (MS14-040) | exploits/windows_x86/local/39446.py
--------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Result
kali@kali:~$
kali@kali:~$ searchsploit -m 39446 win_x86-64/local/39525.py
  Exploit: Microsoft Windows 7 (x86) - 'afd.sys' Dangling Pointer Privilege Escalation (MS14-040)
      URL: https://www.exploit-db.com/exploits/39446
     Path: /usr/share/exploitdb/exploits/windows_x86/local/39446.py
    Codes: CVE-2014-1767, MS14-040
 Verified: False
File Type: Python script text executable, ASCII text
Copied to: /Users/b/Projects/git/forks/exploitdb/39446.py


  Exploit: Microsoft Windows 7 (x64) - 'afd.sys' Dangling Pointer Privilege Escalation (MS14-040)
      URL: https://www.exploit-db.com/exploits/39525
     Path: /usr/share/exploitdb/exploits/windows_x86-64/local/39525.py
    Codes: CVE-2014-1767, MS14-040
 Verified: False
File Type: Python script text executable, ASCII text
Copied to: /Users/b/Projects/git/forks/exploitdb/39525.py


kali@kali:~$
```


