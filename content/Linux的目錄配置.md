---
title: "Linux 的目錄配置"
date: 2018-04-30T13:55:37+08:00
categories: [Linux]
tags: [Linux]
toc: true
math: true
---

# 目錄配置的內容

| 目錄| 應放置檔案內容 |
|---|---|
| / | 根目錄，一般建議只接檔案。|
| /root | 系統管理員（root)的家目錄。|
| /boot |  Linux系統開機會用到的檔案。|
| /home | 使用者的家目錄。|
| /lib,/lib64 | 系統會使用到函式庫的內容。|
| /tmp |  暫時放置檔案資料的地方。|
| /proc |是虛擬（假）的檔案系統，放置執行中程式的資料。|
| /var | 系統執行中變動檔案放置的目錄，ex:log。|
| /usr |unix software resource。|  
| /lost+found |放置遺失的資料（孤兒）或壞掉的資料。|
| /media | 可移除媒體裝置的掛載點，ex:USB。|
| /bin,/usr/bin | 放置使用者可執行的binary file的目錄，ex:ls、rm。|
| /sbin |放置系統管理員用的執行檔。|
| /etc | 系統的設定檔幾乎都放在這。|
| /dev | 裝置或設備都以檔案型態存在這個目錄，ex:鍵盤、硬碟。|

