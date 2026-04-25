# PIE TIME — PicoCTF Easy

**Date:** 2026-04-25
**Time spent:** ~30 min
**Category (actual):** Pwn / Binary Exploitation
**Outcome:** Solved
**Dependency level:** 1 / 4

## 我試過什麼 & 結果

- 用 `checksec` 確認 PIE 開啟
- 看到 source code，發現 main 要求輸入跳轉位址
- 原本打算寫 brute force script
- 用 nc 連線時看到伺服器吐 main 位址
- 改用 leak + offset 計算 win 位址，直接輸入

## 我卡在哪裡 & 為什麼

- 一開始誤以為 brute force 是合理計畫（沒判斷 PIE 熵 ~28 bits 不可行）
- 看到 main 位址那一刻才反應過來「這就是 leak」— 沒在事前主動找 leak 來源
- 跳過了 byte-order packing（題目接受 decimal），這塊技術沒實際練到

## 漏洞機制

PIE binary 每次載入 base 隨機，但 binary 內 symbol 的相對 offset 固定。
程式主動印出 main 位址 → 等於送 attacker 一個 leak。
攻擊者：win_addr = leaked_main + (win_offset - main_offset)，跳過去即可。

## 下次看到類似模式，我會先做什麼

- 看到 PIE → 立刻 `checksec` 確認 + 找 leak 來源（不是想 brute force）
- 看到任何位址被印出來 → 那就是 leak
- 記得 PIE 不能 brute force（28-bit 熵）
- 下一題遇到要 raw bytes 輸入時，回頭學 `p64` pack（這題跳過了）

## Tags

#pwn #pie-bypass #address-leak #picoctf #easy