---
layout: post
title: "Prompt Injection"
date: 2025-08-12 00:00:00 +0800
author: [001]
categories: [AI]
tags: [AI]
---

說道AI/LLM就會聯想到Prompt Injection，以下區分幾個小節來說明

# 什麼是Prompt Injection?

Prompt Engeineer:輸入指令讓AI做事情的話就是Prompt，例如你跟ChatGPT說"91乘9是多少?"，這句話就是Prompt。
但是Prompt也有詳細的差別

|Prompt|AI Output|
|------|---------|
|91乘9是多少?|91 × 9 = 819|
|91乘9是多少?列出詳細步驟|計算：91 × 9<br>1.拆解乘數<br>把91拆成90和1兩部分：<br>91=90+1<br>2.分別乘以9<br>90×9=810<br>91×9=9<br>將結果相加<br>810+9=819|


上面例子可知道，不同的問法會得到不同的回答，那是不是可以透過特別設計的問題，讓AI回復不安全的回答






參考資料:https://github.com/jthack/PIPE
