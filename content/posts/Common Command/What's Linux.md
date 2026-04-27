---
title: What's Linux
tags:
  - linux
categories:
  - Common Command
date: 2021-12-17
draft: true
---

## 環境變數

在做軟體安裝時都需要設定環境變數，會想瞭解這個問題是因為查了很多安裝 pyspark 的網站，發現不同網站寫入環境變數所開啟的檔案都不一樣，才想瞭解一下差異到底是什麼。
順便記錄我使用Linux所參考的網站。

- 修改 /etc/environment 檔案環境變數是對系統的修改，與使用者無關
- 修改 /etc/profile 檔案環境變數是對所有使用者有
- 修改 .bashrc 檔案環境變數只對當前使用者有用


## 遠端桌面連線

[https://weirenxue.github.io/2021/06/17/remote_ubuntu_xrdp/](https://weirenxue.github.io/2021/06/17/remote_ubuntu_xrdp/)

## 安裝 pysaprk

[https://www.datacamp.com/community/tutorials/installation-of-pyspark](https://www.datacamp.com/community/tutorials/installation-of-pyspark)

## Linux截圖

[https://yanwei-liu.medium.com/如何使用ubnutu截圖-cbacb6f3d435](https://yanwei-liu.medium.com/%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8ubnutu%E6%88%AA%E5%9C%96-cbacb6f3d435)

## 在 windows 電腦安裝 Ubuntu 18.04

之前有嘗試過使用 WSL 的方式安裝 Linux，失敗很多次，最後雖然有成功，但後來也沒有再繼續使用。
直到這次想要轉職，發現這個職位的工作技能需要有使用 Linux 的經驗，因此這次決定使用雙系統的方式來安裝 Linux。
由於沒有即時記錄參考的網站，只依稀記得當初的步驟，還是決定現將它記錄下來。
1. download Ubuntu 18.04 檔案
2. 切割磁碟（決定 Ubuntu 可以使用的檔案空間）
3. 使用 USB Live 作開機安裝 Ubuntu
4. 重新開機，在開機時叫出 BIOS（我的電腦是 MSI，按F11）
5. 選擇 USB 開機按鈕
6. 安裝 Ubuntu
7. 每次開機都要切換到 Ubuntu，否則就會直接開機 Windows
8. 步驟 7 修改 bios 的開機順序即可。

