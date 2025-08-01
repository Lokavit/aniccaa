---
title: Git
date: '2013-03-11'
tags: ['code']
draft: false
---

### 語義化 commit
Added 新新增的功能。
Changed 對現有功能的變更。
Deprecated 已經不建議使用，即將移除的功能。
Removed 已經移除的功能。
Fixed 對 bug 的修復。
Security 對安全性的改進

## 刪除歷史提交記錄
```
git reset --soft HEAD~1

```

## 操作遠端分支

```bash
git branch -a #列出本地及遠端分支
#刪除遠端指定分支
git push origin -d 分支名

git branch 分支名 #建立分支
git checkout 分支名 #切換至分支
git branch -D 分支名 #刪除分支
git push origin 分支名 #推送到遠端倉庫
# 將本地master推送到遠端指定分支
git push -f url master:遠端分支名

# 本地倉庫分支關聯遠端倉庫分支
git push -set-upstream origin 分支名
```
## 本地推遠端倉庫
```bash
# 遠端倉庫建同名空倉庫
# 初始化本地專案git
$ git init
# 關聯遠端倉庫
$ git remote add origin https://……/倉庫名.git
# 關聯後，先pull
$ git pull origin master 
# 此處會丟擲錯誤[fatal: refusing to merge unrelated histories]
$ git pull origin main --allow-unrelated-histories
# 將遠端庫裡的檔案pull到本地後，正常push操作即可。
```
## 拉取指定commit的程式碼
```bash
# 找到commit的值(完整值)
$ git checkout <commit的長值>
```

## CentOS 8 & Git

- - 安裝 GIT 及依賴 git version 2.27.0

```bash
mkdir .ssh
建立檔案 authorized_key
把每個允許的機器的公鑰放進來
```

## 解除安裝並裝載最新 GIT

```bash
yum remove git -y # 解除安裝git
# 裝載基本依賴包
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel nss  gcc perl-ExtUtils-MakeMaker -y
# 解壓編譯及裝載
cd /usr/local/src
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.xz
tar xf git-2.9.5.tar.xz
cd git-2.9.5
make prefix=/usr/local/git all
make prefix=/usr/local/git install

# 配置環境變數
echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
source /etc/bashrc
git version # 檢視版本號
```

## 搭建 GIT 伺服器

```bash
# 新建git倉庫主目錄 可放在/var下，用於存放所有倉庫的.git(bare裸庫)
mkdir gitrepos
# 檢視使用者列表
cat /etc/passwd
# 新建訪問使用者 生成於/home/zen家目錄
useradd zen
# 給使用者設定密碼，用於拉取和提交程式碼
passwd zen

# 變更git倉庫的所有者到任何人 ，賦予nobody就是把許可權給了所有人
chown -R nobody:nobody /var/gitrepos
```

## 驗證 git 伺服器

```bash
# 在/var/gitrepos下建立裸庫
git init --bare /var/gitrepos/專案名.git
# 修改讀寫許可權，否則會報錯拒絕提交
chmod -R 777 /var/gitrepos/專案名.git

# 客戶端拉取，也可以在伺服器找對應地方拉取該倉庫
git clone http://114.96.107.178:88/var/gitrepos/test.git
# 拉取下來是空倉庫，新增內容，按照常用操作git形式提交
git push
# 在伺服器上的www/wwwroot/下拉取倉庫
git clone http://114.96.107.178:88/var/gitrepos/test.git
# 後續持續推拉倉庫操作
## 在伺服器上找不到push的程式碼。因其為倉庫，而非檔案系統。

git checkout master # 是否使用？

# 伺服器端存放的叫裸庫(bare)，倉庫的.git/目錄下面的那些東西，不包含工作區(working space)。
# 直接git clone --bare也可以直接拿到裸庫做服務端，從裸庫還原出工作區只需要git clone一次即可。

# 報錯
bash: git-upload-pack: command not found
fatal: Could not read from remote repository.
fatal: Could not read from remote repository.
fatal: Could not read from remote repository.
# 在linux命令列輸入如下命令即可解決
ln -s /usr/local/git/bin/git-upload-pack /usr/bin/git-upload-pack
```

```bash
git init # 初始化 使用當前目錄作為Git倉庫
git init qqn # 初始化 使用指定目錄作為Git倉庫
git commit -m '提交說明' # Linux
git commit -m "提交說明" # Windows
```

## VSCode 設定同步

- Settings sync on

```bash
$ git branch modal # 新建分支
$ git checkout modal # 切換分支
$ git status
$ git add .
$ git status
$ git checkout master # 切換到住分支
$ git merge modal # 提交分支

$ git push origin modal # 提交分支到遠端

$ git fetch origin master # 下載最新的版本到origin/master分支上
$ git log -p master..origin/master # 比較本地的master分支和origin/master分支的差別
$ git merge origin/master # 最後進行合併

## 解決衝突之後
$ git status
$ git add .
$ git status
$ git commit -m "++"

## 合併到 master
$ git checkout master
$ git merge modal
$ git push origin

## 刪除本地及遠端分支
$ git branch -a # 檢視所有分支
$ git branch -d router # 刪除本地 router分支
$ git push origin --delete router # 刪除遠端 router分支
```

## Git 多倉庫同步

```bash
# GitHub與區域網GitLab共享一個專案
# 在兩個倉庫裡分別建立【空】專案[不含READMME.md檔案]
# 本地建立 shiperp ,並初始化 git
$ cd git init
# 關聯github遠端倉庫
$ git remote add lokavit https://github.com/Lokavit/shiperp.git
# 關聯區域網gitlab遠端倉庫
$ git remote add satya http://區域網地址/frontend/shiperp.git
$ git remote -v # 檢視遠端倉庫
# 新建一個 README.md檔案
$ git status  git add .   git status
$ git commit -m "Add README.md"
$ git push lokavit master # 推送到 github的同名專案中
$ git push satya master # 推送到 gitlab的同名專案中
```

- 以下為其他情況，或許可以排錯

```bash
# 在README.md裡編輯一下
$ git status # 檢視檔案狀態
$ git add . # 新增檔案
$ git status # 檢視暫存狀態
$ git commit -m "Add Content 2" # 提交msg
$ git pull origin master # 拉去 origin 倉庫主分支
$ git push origin master # 提交 origin 倉庫主分支
$ git pull mirror master # 拉去 mirror 倉庫主分支
$ git push mirror master # 提交 mirror 倉庫主分支

## 其他錯誤處理
$ git pull mirror master --allow-unrelated-histories # 確定合併
```

```bash
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[remote "lokavit"]
	url = https://github.com/Lokavit/shiperp.git
	fetch = +refs/heads/*:refs/remotes/lokavit/*
[remote "satya"]
	url = http://區域網地址/frontend/shiperp.git
	fetch = +refs/heads/*:refs/remotes/satya/*
```

## gitBash 開啟 VSCode

```Bash
# C:\Users\username/.bash_profile檔案
# generated by Git for Windows
test -f ~/.profile && . ~/.profile
test -f ~/.bashrc && . ~/.bashrc

# OPEN Program 注意空格轉義及斜線轉義。
alias vscode="D:/\Program\ Files/\Microsoft\ VS\ Code/Code.exe"
# VUE
alias vue='winpty vue.cmd'
```

## 本地建立專案，連線遠端倉庫

在 Github 上建立一個空倉庫，亦可見到如下提示

```Bash
# GitHub上建立專案(空)倉庫,本地建立專案
$ cd 專案根目錄 # 進入專案根目錄
$ git init # 初始化
$ git status # 檢視檔案狀態
$ git add .  # 新增到暫存區
$ git status # 檢視檔案狀態
$ git remote add origin 倉庫地址 # 連線到遠端倉庫
$ git remote -v # 檢視是否連線成功
$ git push -u origin master # 當前分支關聯到遠端分支
```

## git 多人協作

```Bash
$ git branch modal # 新建分支
$ git checkout modal # 切換分支
$ git status
$ git add .
$ git status
$ git checkout master # 切換到住分支
$ git merge modal # 提交分支

$ git push origin modal # 提交分支到遠端

$ git fetch origin master # 下載最新的版本到origin/master分支上
$ git log -p master..origin/master # 比較本地的master分支和origin/master分支的差別
$ git merge origin/master # 最後進行合併

## 解決衝突之後
$ git status
$ git add .
$ git status
$ git commit -m "++"

## 合併到 master
$ git checkout master
$ git merge modal
$ git push origin

```

## git bash 使用 tree

```Bash
# 將tree.exe放入git.usr/bin中
$ tree -d # 顯示目錄名稱而非內容
$ tree -L 3 # 指定層級
# 命令集，詳見末尾
```

> warning: LF will be replaced by CRLF in ……………….

```Bash
$ git config --global core.autocrlf true # 當簽出程式碼時，LF會被轉換成CRLF (Win)
$ git config --global core.autocrlf input # 提交時把CRLF轉換成LF(Mac Linux)
$ git config --global core.autocrlf false # 僅執行在Windows上的專案
```

## Git 拉取遠端分支

```Bash
$ git init # 本地初始化
$ git remote add origin 倉庫地址  # 連線遠端倉庫
$ git fetch origin dev # dev為遠端倉庫的分支名
# 在本地建立分支dev並切換到該分支
$ git checkout -b dev origin/dev
$ git pull origin dev # 拉取 git上dev分支上的內容
```

### Git 術語及名詞解釋

**Workspace**：工作區

- 透過 *git init*建立程式碼庫的所有檔案,不包含*.git*(版本庫檔案)

**Stage/Index**：暫存區/索引

- 透過*git add ./(地址)*新增修改;
- 透過*git status* 可以看到修改的狀態.

**Repository**：倉庫/本地儲存庫

**Remote**：遠端倉庫

**git status -s** 檢視檔案狀態

- _??_：表示新新增未跟蹤的檔案
- _A _：表示新新增到暫存區中的檔案
- _M _：表示修改過的檔案
- _右 M_：表示該檔案被修改但未放入暫存區;
- _左 M_：表示該檔案被修改並放入暫存區;
- _雙 M_：表示工作區修改並提交到暫存區後,又在工作區中被修改,所以左右皆有.
- _R _：表示該檔案被重新命名

### 基本操作流程

**常規提交-推送流程**

```Bash
$ git status  # 檢視檔案狀態,確定是否全部以放入暫存區
$ git add .  # 將未放入暫存區的檔案放入暫存區
$ git commit # 提交
$ git push  # 推送到遠端倉庫
```

**跳過暫存區提交流程**

```Bash
$ git status # 檢視檔案狀態,確定是否全部以放入暫存區
$ git commit -a -m '更新說明'
```

**檢視被打分支下的檔案**

```Bash
$ ls # 檢視當前目錄下的所有資料夾及檔案;
$ cd dir # 開啟某資料夾,繼續*ls*檢視;
```

**刪除檔案**

```Bash
$ git rm FileName  # 刪除指定檔名的檔案
$ git rm FileName -r -f # 刪除指定資料夾下所有檔案
$ git status -s  # 確認檔案狀態
$ git commit -m 'DeleteFIle' # 提交版本的說明
$ git push  #  推送到遠端倉庫
```

**Git 操作目錄**

```Bash
$ cd C:/ # 切換磁碟,
## === 進入帶空格的目錄=== ##
$ cd "Program Files" # 第一種：將檔名加上""
$ cd Program" "Files # 第二種：將空格加上""
```

### Git 常用命令

```Bash
$ git config --global user.name "yourname" # 配置全域性使用者資訊
$ git config --global user.email 123@456.com # 配置全域性使用者資訊

$ git config --list  # 檢視配置資訊

$ git config --global core.editor vim # 自定義編輯器

$ git helep # 檢視幫助手冊
$ git --help # 檢視幫助手冊

$ git help config # 檢視config命令手冊

$ git clone url(倉庫地址) # 克隆倉庫
$ git clone url(倉庫地址) MyGit # 克隆倉庫,自定義本地儲存庫的名字

$ git init #  初始化倉庫 (該命令在需管理的本地儲存庫目錄下執行)

$ git add Test.cs # 對檔案跟蹤 ,使檔案處於暫存狀態(允許該命令後再次修改的檔案,需要再次允許該命令)
$ git commit -m 'Update Version'  # 提交,備註資訊自定義

$ git status # 檢視當前檔案處於什麼狀態
$ git status -s # 檢視當前檔案狀態(緊湊版)

$ git diff #  檢視檔案具體修改的地方(只顯示尚未暫存的改動，而非自上次提交以來的所有改動。)
$ git diff --cached # 檢視已經暫存起來的變化

$ git rm # 移除檔案(將檔案從暫存區中移除)
$ git rm log/\*.log # 刪除log/目錄下副檔名為.log的所有檔案
$ git rm \*~   # 刪除以 ~ 結尾的所有檔案。

$ git mv file_from file_to #  檔案重新命名

$ git log  # 檢視提交歷史
$ git log -p # 顯示每次提交的差異
$ git log -p -2 # 僅顯示最近兩次提交
$ git log --stat # 檢視每次提交簡略的統計資訊
$ git log --pretty=online  # 將每個提交放在一行顯示

$git commit --amend # 重新提交(將暫存區的檔案提交,如未作修改,則只變更提交資訊)

# 提交後發現忘記了暫存某些需要的修改
$ git commit -m 'Update commit'
$ git add (忘記的檔案)
$ git commit --amend # 最終只有一個提交(第二次提交代替第一次提交的結果)

$ git reset HEAD my.txt # 取消暫存

$ git checkout -- my.txt # 撤銷之前所做的更改


$ git remote  # 檢視已配置的遠端倉庫伺服器
$ git remote -v  # 顯示需要讀寫遠端倉庫使用的Git儲存的簡寫與其對應的URL

$ git remote add test url(遠端倉庫地址) # 新增新的遠端倉庫，並指定一個可以輕鬆引用的簡寫
# 此時,可以用 test 來代替 url

$ git fetch [remote-name] #  拉取遠端倉庫
$ git fetch test  #  拉取test的遠端倉庫

$ git pull  # 自動抓取後,合併遠端分支到當前分支

$ git push [remote-name] [branch-name]  # 推送到遠端倉庫
$ git push origon master

```

---

## Git Bash 常用命令

### 資料夾操作

```Bash
$ mkdir 資料夾名  # 建立資料夾
$ mkdir 資料夾名 && cd 資料夾名  # 建立並開啟資料夾
$ rm -r 資料夾名  # 刪除資料夾
$ cd  # 切換到某目錄下
$ cd ..  # 退回到上一目錄
$ pwd  # 顯示當前目錄路徑
$ ls  # 列出當前目錄中所有檔案
$ ll  # 列出當前目錄所有檔案，詳細版
```

### 檔案操作

```Bash
$ touch 檔名.字尾名  # 新建檔案
$ rm 檔名.字尾名  # 刪除檔案
$ vi 檔名.字尾名  # 新建檔案並進入編輯狀態。
$ vi index.html encoding:utf8  # 建立檔案指定編碼格式
# 注：vi為linux文字編輯器,該命令實則進入vi程式。
預設為命令模式，按下 i 切換為編輯模式。
輸入內容，按esc退出編輯模式, 按 :x 退出並儲存，回到命令列介面。

$ mv 檔名.字尾名 目標資料夾
$ mv index.js src  # 表示把當前的index.js檔案移動到src資料夾下

```

### 其它操作

```Bash
$ rest  # 清屏
```

## 在 Git Bash 中用某程式開啟某檔案

### 在 Git Bash 中用 Chrome 開啟 index.html 檔案

- 建立一個檔案，命名為需要的命令，如 chrome(無後綴名)

```Bash
#!/bin/sh
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" $1 &

## 註釋：
第一行表示這是一個shell指令碼
第二行是chrome的裝載目錄
$1 取命令之後輸入的引數
& 此命令在後臺開啟，不會阻塞git bash
```

- 將該檔案儲存在 Git

```Bash
D:\Program Files\Git\mingw64\bin
```

- 使用方式

```Bash
$ chrome index.html  # 即可用Chrome瀏覽器開啟html檔案
```

過濾掉不需要提交到遠端倉庫的檔案

```.gitignore
/node_modules
.vscode
```
