---
layout: post
title: "PortSwigger Academy - Authentication vulnerabilities"
date: 2025-05-21 00:00:00 +0800
author: [001]
categories: [PortSwigger Academy]
tags: [Academy]
---
# PortSwigger Academy - Authentication vulnerabilities
===
* 身分認證漏洞
* 在本節重點
* 網站最常用的身份驗證機制。
* 這些機制中存在潛在的漏洞。
* 不同身份驗證機制中固有的漏洞。
* 由於實施不當而引入的典型漏洞。
* 如何使您自己的身份驗證機制盡可能強大。

### 驗證(authentication) vs 授權(authorization)
驗證:確認登入帳號的使用者是否為本人

授權:確認帳號的是否有該功能權限


### Brute-forcing passwords 暴力破解密碼
指攻擊者使用試誤系統來猜測有效的使用者憑證

防禦方式
密碼策略，迫使用戶創建高熵密碼，至少在理論上，僅使用暴力破解更難破解。這通常涉及透過以下方式強制使用密碼：

* 最小字元數
* 小寫和大寫字母的混合
* 至少一個特殊字符

### Username enumeration 使用者名稱枚舉

使用者名稱枚舉通常發生在登入頁面上（例如，當您輸入有效的使用者名稱但密碼不正確時），或在註冊表單上（當您輸入已使用的使用者名稱時）。這大大減少了暴力登入所需的時間和精力，因為攻擊者能夠快速產生有效使用者名稱的候選清單。


### Bypassing two-factor authentication 繞過雙重認證


有時，雙重認證的實施有缺陷，以至於可以完全繞過它。

如果先提示使用者輸入密碼，然後提示使用者在單獨的頁面上輸入驗證碼，則使用者在輸入驗證碼之前實際上處於「登入」狀態。在這種情況下，值得測試一下在完成第一個身份驗證步驟後是否可以直接跳到「僅登入」頁面。有時，您會發現網站在加載頁面之前實際上並沒有檢查您是否完成了第二步。

簡單來說就是知道登入後的URL`/my-account`，可以登入之後直接輸入網址`/my-account`，跳過2FA認證，正常流程是login->2FA->login success。


