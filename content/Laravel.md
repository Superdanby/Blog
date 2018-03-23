---
title: "Laravel 使用心得(竟然是中文!!)"
date: 2017-09-25T00:20:00+08:00
categories: [Laravel]
tags: [Laravel, Laravel 5, SELinux]
toc: true
math: false
---

# 前情提要

暑假的時候，因為學校總務處想要一個新的網站，後來決定使用  [Laravel](https://laravel.com/) 來架設，不過那時候我還只會一點點的 html && css，所以很多東西都是邊學邊作。

主要選用[Laravel](https://laravel.com/)的原因是舊的網站是php寫的，想說有些東西可以複製貼上(結果幾乎沒有OAO)。

# 安裝

如果是用 Fedora 可以在[這裏](https://developer.fedoraproject.org/start/sw/web-app/laravel5.html)找到所有安裝步驟。

接著最麻煩的問題就來了：SELinux會導致權限不足

很多人都會說直接把SELinux關掉就可以了 `setenforce 0` ，但是這非常危險！正確的做法是到新建的project底下執行: `chcon -R -t httpd_sys_rw_content_t storage`

另外，如果有資料夾是要直接給apache存取的，不要把權限設爲 777 而是應該把那個資料夾的group設爲apache並且將權限設爲 775

# 使用
接下來就是瘋狂地看 documentation。說實在話，我覺的官網文檔寫的並不好，資料很多很雜，看完不一定知道要怎麼做才能作出自己想要的東西，還是必須要瘋狂去 Google。

# 成品
[國立中正大學總務處網站](https://affairs.ccu.edu.tw/)

# Tips
如果後臺登入想要用 username 而不是 email 的話，可以參考這個[影片](https://www.youtube.com/watch?v=jAJZO3LurOU&lc=z13rungrysiav1pfn23pujoxhmr4i12tm.1480351316314888)
