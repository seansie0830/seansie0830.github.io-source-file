---
title: "如何利用hugo套件建立靜態網頁型網誌並佈署在github page上"
date: 2022-07-25T23:33:38+08:00
draft: true
---

# 安裝
本次全程linux作業系統（debian-based)上進行，windows的大概大同小異，可以參考官方說明。

https://gohugo.io/getting-started/installing/

這邊有遇到一些坑，也會會跟大家分享，希望有幫助到碰倒相同問題的人。

debian-based的作業系統安裝軟體通常都會用`apt`這個工具，不過本人用`apt `安裝的時候發現版本過舊
與很多主題都無法相容。
所以為了安裝到最新版本的hugo，建議使用`homebrew`這個工具(原本是macOS的工具，不過linux也可以用)，本人用這個才成功。

> ### 更新
>
> (2022.10.15)
>
> 因為有些人在安裝homebrew時速度會很慢，建議可以直接下載hugo的deb包會更快速
>
> 網址如下 https://github.com/gohugoio/hugo/releases
>
> 建議使用`wget` 下載 命令就是`wget`空一格+網址
>
> 接著用`dpkg`安裝，命令如下
>
> ```
> dpkg -i [YOUR PACKAGE]
> ```
>
> 

- 命令如下

```bash

 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

```

需要```curl```與`git` 沒有記得安裝 ` sudo apt install curl git`

- 安裝完記得還要執行一段命令，來把`homebrew`加入`PATH`，如果沒有執行就無法使用，我就是因為沒注意到，試了一陣子才發現。

  安裝完畢會出現這些訊息，執行下面的命令

  ```bash
  ==> Next steps:
  - Run these two commands in your terminal to add Homebrew to your PATH:
      echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /root/.profile
      eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
  - Install Homebrew's dependencies if you have sudo access:
      sudo apt-get install build-essential
  ```

  

- 最後輸入`brew install hugo`安裝
- 最後輸入`hugo version` 驗證安裝，如果成功安裝會出現版本資訊。

# 新增一個網頁

1.首先開終端機，輸入`hugo new site [YOUR SITE NAME]` YOUR SITE NAME 填入自己網頁的名字。

2.接著建立完，輸入`ls`命令會出現以下資料夾

```
archetypes  config.toml  content  data  layouts  public  resources  static  themes
```

輸入`cd themes`，切換目錄到主題資料夾，並到hugo的官方主題商店(https://themes.gohugo.io/)下載主題，挑一個自己喜歡的

> 下載方式，在(https://themes.gohugo.io/找到自己喜歡的主題，按下download，前往github，並使用`git clone` 

3.接著編輯`config.toml` **設定檔**

輸入`cd ..` 回到上層目錄，使用編輯器編輯`config.toml`設定檔，若在**終端機**環境，建議使用`nano`編輯器，簡單易懂。

`nano`編輯器使用`sudo apt install nano`安裝。

```toml
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = "ananke"
```

在下面新增 `theme = "[YOUR THEME]" `  YOUR THEME指你的主題。

4.如果要新增貼文 ，輸入`hugo new posts/my-first-post.md` my-first-post 填入貼文名字。

下完指令後，系統會在`.content/posts`中新增`markdown`檔案，可以使用以下的語法

> **markdown**語法簡介
>
> 1. 標題 (用加在前面的#號代表)
>
>    舉例
>
>    ```
>    # 我是大標題
>    ## 我是第二大標題
>    ```
>
>    # 我是大標題
>
>    ## 我第二大標題
>
>    ***#號越多越小***，記得要空格
>
>    2.常見字體格式
>
>    舉例
>
>    ```
>    **粗體**  *斜體*  <u>底線</u> ***粗體&斜體***
>    ```
>
>    **粗體** *斜體* <u>底線</u>  ***粗體與斜體***
>
>    3.若沒有加記號，就是正常內文。

5.完成後，輸入`hugo server -D`啟動hugo伺服器，可以在本地測試。接下來會出現一個網址，點下去可以預覽。

6.覺得沒問題，輸入`hugo -D`建立靜態網頁，預設會儲存在`./public/`中

# 上傳到github page 上

github原本是軟體代碼的託管網站，不過他也有讓人免費建立小型網站的功能，本次就是要利用這個功能。

1.註冊一個github帳號

2.新增一個repo(類似一個存檔案的空間，主要來存程式碼) 名字叫做`[USERNAME].github.io` ，這樣才會啟用`github page`功能。

3.上傳靜態網頁內容到剛剛建立的repo

>github上傳說明
>
>1.先將目錄切換到`./public/` 輸入指令`git init`建立git儲存庫
>
>2.輸入`git add .`將所有檔案加入追蹤
>
>3.輸入`git commit `提交到儲存庫
>
>這邊有個坑，git的預設編輯是`vim` ，操作方式有點奇怪，方式如下
>
>> 首先先按方向鍵的下鍵，把游標移動到最下面，按下`I`進入插入模式，編輯完之後按下`esc`鍵退出插入模式，最後輸入`:wq`存檔並離開。

> 4.最後輸入`git push [URL]` 把文件推送到`github`上

4.上傳完後，沒有意外的話`github action`會自動部屬，過不久就可以打開瀏覽器，輸入`[your username].github.io`看看自己的網頁了。

若沒有可以看看`githubstatus` 上次碰巧遇到`github page`大當機，原本還以為是自己的問題，過不久就好了