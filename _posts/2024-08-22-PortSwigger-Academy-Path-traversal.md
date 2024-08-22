---
layout: post
title: "PortSwigger Academy - Path traversal"
date: 2024-08-22 12:00:00 +0800
author: [001]
categories: [PortSwigger Academy]
tags: [Academy]
---


# PortSwigger Academy - Path traversal

### Path traversal
路徑遍歷（Path traversal）也稱為目錄遍歷（Directory traversal）。這些漏洞使攻擊者能夠讀取運行應用程序的服務器上的任意文件。這可能包括：

* 應用程序代碼和數據。
* 後端系統的憑據。
* 敏感的操作系統文件。

在某些情況下，攻擊者可能能夠寫入服務器上的任意文件，從而修改應用程序數據或行為，最終完全控制服務器。

> Reading arbitrary files via path traversal
通過路徑遍歷讀取任意文件

假設有一個顯示待售商品圖片的購物應用程序。這個應用程序可能使用以下HTML加載圖片：`<img src="/loadImage?filename=218.png">`
loadImage URL 需要一個文件名參數，並返回指定文件的內容。圖片文件存儲在 `/var/www/images/` 位置。為了返回圖片，應用程序將請求的文件名附加到這個基目錄，並使用文件系統API讀取文件的內容。換句話說，應用程序從以下文件路徑讀取：`/var/www/images/218.png`

這個應用程序沒有實施針對路徑遍歷攻擊的防禦。因此，攻擊者可以請求以下URL來從服務器的文件系統中檢索 `/etc/passwd` 文件：
`https://insecure-website.com/loadImage?filename=../../../etc/passwd`

這會導致應用程序從以下文件路徑讀取：

```
/var/www/images/../../../etc/passwd
```

在文件路徑中，序列 ../ 是有效的，意思是返回上一級目錄結構。連續三個 ../ 序列從 /var/www/images/ 返回到文件系統根目錄，因此實際讀取的文件是：

```
/etc/passwd
```

在類Unix操作系統中，這是一個包含服務器上註冊用戶詳細信息的標準文件，但攻擊者可以使用相同的技術檢索其他任意文件。

在Windows系統中，`../ `和 `..\` 都是有效的目錄遍歷序列。以下是針對基於Windows的服務器的等效攻擊示例：
`https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`







