#Git logbook

##步驟
1. 確認 Git 版本正確
2. 設定 Git 使用者的名稱和信箱
3. 找到並進入專案資料夾
4. 使用 git init 登記專案資料夾
5. 刪除 .git 資料夾解除 Git 版本控制 => `rm -rf .git`
6. 用圖形化介面軟體（SourceTree／GitKraken）重複一次操作

##確認你的 Git 版本
請使用以下指令確認 Git 的版本為 2.14 以上，結果應類似下圖：

```
Terminal  
$ git --version
git version 2.14.1
```

如果你是 macOS 的使用者，你可以使用以下指令進行 Git 的版本更新：

```
Terminal
$ brew update                                           # 更新 homebrew 軟體管理工具
$ brew upgrade git                                      # 更新 Git 版本
```

如果你是 Ubuntu 的使用者，你可以使用以下指令進行 Git 的版本更新：

```
Terminal
$ sudo add-apt-repository ppa:git-core/ppa              # 註冊 PPA 獲得下載 git 位置
$ sudo apt-get update                                   # 更新所有可下載的軟體版本
$ sudo apt-get install git                              # 安裝新版本的 git
```

設定你的 Git 使用者名稱和信箱
請使用以下指令在電腦的 Git 上設定使用者名稱和電子郵件：

```
Terminal
$ git config --global user.name "your name"             # 請輸入你的 使用者名稱
$ git config --global user.email "your email"           # 請輸入你的 電子郵件
```

未來在進行程式碼的版本更新時，Git 會把修改者的使用者名稱和電子郵件附加上去，方便我們追蹤程式碼的修改。

請使用以下指令確認輸入的資料是否正確：

```
Terminal
$ git config --list
```

指令結果應有類似下圖的資訊：

```
Terminal
user.name= your_username                                # 你輸入的 使用者名稱
user.email= your_email@email.com                        # 你輸入的 電子郵件
```

如果您覺得剛設定的使用者名稱與電子郵件不滿意，或輸入錯了，請再次進行一次設定，新的名稱和信箱會覆蓋掉舊的。

找到後，請在該專案資料夾內，使用以下指令：

```
Terminal
$ git init
```

請使用以下指令確認資料夾內有一個名為 .git 的資料夾，表示登記成功：

```
Terminal
$ ls -al
```

Git 會使用這個 .git 資料夾來紀錄該專案的內容更動。

如果你發現登記錯了，請在該錯誤的資料夾下，使用以下指令：

```
Terminal
$ rm -rf .git
```

這個指令會刪除 .git 資料夾，這樣就會解除該專案資料夾的版本控制，請再去正確的資料夾內使用 git init 來進行登記專案的動作。

設定好環境和登記好專案後，接下來要開始建立專案的版本紀錄了

##操作示範：提交內容更動，建立版本節點
###本單元的操作示範有五：
* 使用 git status 查詢更動過的內容
* 使用 git add 把有更動過的檔案加入暫存區
* 使用 git commit 建立版本紀錄節點
* 使用 git status 確認所有檔案都建立版本紀錄
* 用圖形化介面軟體（SourceTree／GitKraken）重複一次操作

##檢查檔案目前的狀態，提交內容更動，建立版本紀錄
###請使用以下指令檢查目前資料夾內的檔案是否有更動過：
```
Terminal
$ git status
```

請使用以下指令把檔案內容更動加入暫存區：

```
Terminal
$ git add filename
```

你也可以使用以下指令，把「所有」更動過但尚未建立版本紀錄的檔案都加入暫存區：

```
Terminal
$ git add .
```

請使用以下指令把暫存區的內容更動加入儲存庫，建立新的版本紀錄節點，別忘了寫上這個版本紀錄的更動訊息（如：這次更動的內容和目的）：

```
Terminal
$ git commit -m "請描述你這次更新的目的和內容"
```
注：更新訊息也可以使用中文字！

透過影片，我們知道還原版本有兩個要素：

還原的版本——特定的 commit 節點
還原的指令—— git revert、git reset


##操作示範：還原你的專案版本
本單元的操作示範有五：

* 使用 git reset 還原到之前的版本
* 使用 git diff 觀察檔案差異
* 刪除一些程式碼造成錯誤，並建立版本紀錄
* 使用 git revert 把上一個錯誤移除
* 用圖形化介面軟體（SourceTree／GitKraken）重複一次操作

##找出 commit 節點代碼，還原專案，找出差異
【macOS】請先進行這個設定，讓 git 在進行 git revert 指令時可以啟動 Sublime：

```
Terminal
$ git config --global core.editor "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl -n -w"
```

使用以下指令找出要還原的 Commit 節點的 SHA1 代碼：

```
Terminal
$ git log
```

##在這個單元裡有兩種還原專案的方法：
* git reset：刪除版本紀錄，但保留內容。
* git revert：刪除該版本紀錄的內容更動，並新增一個版本紀錄。

兩種還原方式各有不同的好處，如果你很清楚錯誤出自哪個版本紀錄的內容更動，請使用 git revert；如果你不確定，或該版本紀錄檔案十分多，git revert 會把很多正確的更動也刪除，那就使用 git reset，然後用 git diff 進行比較。

以下是 git reset 的指令，後面要加上 SHA1 代碼：

```
Terminal
$ git reset <SHA1>
```

以下是 git revert 的指令，後面要加上 SHA1 代碼：

```
Terminal
$ git revert <SHA1>
```

注意：【Ubuntu】使用 git revert 後會開啟 nano 編輯器，自動生成新的版本紀錄的更新訊息，請使用 CTRL + X 確認和退出，這樣就完成 revert 了。

以下是使用 git reset 後，用來比較編輯中的內容和版本紀錄的差異的指令：

```
Terminal
$ git diff
```
注意：如果 git diff 的結果太長，會進入一個遊覽模式，可以按 q 鍵退出。

git 以一行為單位進行內容更動的紀錄，綠色是新增的內容，紅色是刪除的內容，如果你修改過某行的內容的話，原本的那行會變成紅色，修改後的結果會變成綠色新增上去。

兩種方式，各有好壞，如果你想要多使用 `git revert`，盡可能每次建立版本紀錄時，只 commit 單一檔案，這樣 `git revert` 使用起來會比較方便；當然 commit 越少檔案，`git reset` 也更好找出錯誤。

關於版本控制的良好習慣，我們會在下一單元裡面和大家說明。


##透過影片，我們知道還原版本有兩個要素：

* 還原的版本——特定的 commit 節點
* 還原的指令—— `git revert`、`git reset`


##操作示範：還原你的專案版本
###本單元的操作示範有五：

* 使用 `git reset` 還原到之前的版本
* 使用 `git diff` 觀察檔案差異
* 刪除一些程式碼造成錯誤，並建立版本紀錄
* 使用 `git revert` 把上一個錯誤移除
* 用圖形化介面軟體（SourceTree／GitKraken）重複一次操作

##找出 commit 節點代碼，還原專案，找出差異
###【macOS】請先進行這個設定，讓 git 在進行 git revert 指令時可以啟動 Sublime：
```
$ git config --global core.editor "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl -n -w"
```

使用以下指令找出要還原的 Commit 節點的 SHA1 代碼：
```
$ git log
```

在這個單元裡有兩種還原專案的方法：
* git reset：刪除版本紀錄，但保留內容。
* git revert：刪除該版本紀錄的內容更動，並新增一個版本紀錄。

兩種還原方式各有不同的好處，如果你很清楚錯誤出自哪個版本紀錄的內容更動，請使用 `git revert`；如果你不確定，或該版本紀錄檔案十分多，`git revert` 會把很多正確的更動也刪除，那就使用 `git reset`，然後用 `git diff` 進行比較。

以下是 `git reset` 的指令，後面要加上 SHA1 代碼：

```
$ git reset <SHA1>
```

以下是 git revert 的指令，後面要加上 SHA1 代碼：

```
$ git revert <SHA1>
```

注意：【Ubuntu】使用 `git revert` 後會開啟 nano 編輯器，自動生成新的版本紀錄的更新訊息，請使用 CTRL + X 確認和退出，這樣就完成 revert 了。

以下是使用 `git reset` 後，用來比較編輯中的內容和版本紀錄的差異的指令：

```
$ git diff
```

注意：如果 `git diff` 的結果太長，會進入一個遊覽模式，可以按 q 鍵退出。

git 以一行為單位進行內容更動的紀錄，綠色是新增的內容，紅色是刪除的內容，如果你修改過某行的內容的話，原本的那行會變成紅色，修改後的結果會變成綠色新增上去。

兩種方式，各有好壞，如果你想要多使用 `git revert`，盡可能每次建立版本紀錄時，只 `commit` 單一檔案，這樣 `git revert` 使用起來會比較方便；當然 `commit` 越少檔案，`git reset` 也更好找出錯誤。
關於版本控制的良好習慣，我們會在下一單元裡面和大家說明。



#把專案「推」上 GitHub Lab Last updated: 2017-12-04 17:21
・能使用 `git remote add` 註冊遠端 GitHub 網址
・用 `git push` 把專案推上 GitHub

開發到一個段落之後，通常會將已完成 commit 的新版本專案上傳到遠端平台，讓其他夥伴可以下載最新版本繼續開發，保持專案的更新。

##從 Local Repository 上傳至 Remote Repository
Git 會使用 `git push` 把專案上傳到 GitHub 上去，使用 `git pull` 和 `git clone` 把專案下載到電腦裡，而本單元會專注在 `git push` 上，不會提及 `git pull` 和 `git clone` 的部分。

在 Git 中，每一個可以進行版本控制的單位就是 Commit，因此在進行 `git push` 之前，要記得先檢查本地的專案是不是已經 Commit 了，然後才把專案推上去遠端平台。

##操作示範：將專案「推」上 GitHub
本單元的操作示範有七：          

* 確認專案資料夾都已經 commit 好了
* 在 GitHub 創建一個新的專案，複製其 HTTPS 網址
* 使用 `git remote add origin path` 註冊該 GitHub 網址
* 使用 `git remote -v` 確認註冊了的 GitHub 網址
* 使用 `git remote remove origin` 重設遠端網址
* 使用 `git push -u origin master` 把專案上傳到 GitHub-n
* 用圖形化介面軟體（SourceTree／GitKraken）重複一次操作


macOS 操作示範：iTerm2 + SourceTree
Ubuntu 操作示範：Terminal + GitKraken

在開始 `git push` 之前，記得要先確認你是否已經 `commit` 好最新版本的專案。
請前往 GitHub 創建一個新的專案，創建好的專案頁面應類似下圖：

紅色方形內的第一行：設定 GitHub 雲端網址用的指令。
紅色方形內的第二行：把專案「推」上 GitHub 要用的指令。

在專案資料夾內，以下指令可以設定好 GitHub 雲端平台的網址：

```git
My-personal-site $ git remote add origin path              # 斜體為專案網址
```

在專案資料夾內，以下指令可以檢查遠端的 Git Repository（設定好後結果應類似下圖）：

```git
My-personal-site $ git remote -v
origin https://github.com/Archimondes/my-personal-site.git (fetch)
origin https://github.com/Archimondes/my-personal-site.git (push)
```

如果你不小心輸入錯的網址，可以使用以下指令移除遠端設定：

```git
My-personal-site $ git remote remove name                  # 斜體為網址名稱，教案裡名 origin
```

在專案資料夾內，以下指令可以把該專案最新建立的版本「推」上遠端 Git Repository，若是第一次使用 `git push` 會需要你輸入帳號和密碼：

```git
My-personal-site $ git push -u origin master
Username for 'https://github.com': Username                # GitHub 暱稱
Password for 'https://username@github.com': Password       # 密碼不會出現
```

請注意！使用 `git push origin master` 時，有些情況會無法成功，需要再補上一個參數 -u ，詳情可以參考這篇說明。

`git push` 之後，可以再次利用 git status 檢查版本控制的狀態：

```git
My-personal-site $ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean
```

`git status` 除了「On Branch Master」和「nothing to commit, working tree clean」外，在使用 `git remote` 和 `git push` 指令後，就會多出現「 Your branch is up-to-date with 'origin/master'. 」的字眼，表示本機端與遠端皆是最新版本。

你也可以前往 GitHub 的專案頁面，結果應類似下圖：

你已經成功把個人網站上傳到 GitHub 了。

接下來，你將學習使用 GitHub Pages 展示自己的網頁。





