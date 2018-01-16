#利用 Heroku 命令列建立你的應用程式
##安裝 Heroku CLI
我們需要先安裝 Heroku 的命令列。Heroku 提供了一個方便的套件 Herorku Command line Interface (CLI)，也稱作 Heroku Toolbelt，既然大家都不陌生 Command Line Interface，那我們就來安裝吧！



##【 Mac 】
我們將使用 Homebrew 進行 Heroku Command Line Interface 的安裝（如果忘記 Homebrew 是什麼的話，可以點這裡複習）：

```
$ brew install heroku
```

整句指令的意思是這樣的：

* Brew：呼叫 homebrew
* Install：告訴它我們要使用 install（安裝）功能
* heroku：被安裝的程式名稱
輸入指令按下 Enter 以後，你應該會看到類似以下訊息跑出來：

```
$ brew install heroku
==> Downloading https://homebrew.bintray.com/bottles/heroku-6.12.7.sierra.bottle.tar.gz
Already downloaded: /Users/user/Library/Caches/Homebrew/heroku-6.12.7.sierra.bottle.tar.gz
==> Pouring heroku-6.12.7.sierra.bottle.tar.gz
==> Using the sandbox
🍺 /usr/local/Cellar/heroku/6.12.7: 5,973 files, 30.7MB
```
訊息的內容可能每個人都不太一樣，但只要看見酒杯符號🍺，就表示你成功了！

##【 Ubuntu 】
對於 Ubuntu 的話，則是使用下列指令進行安裝：

```
$ wget -qO- https://cli-assets.heroku.com/install-ubuntu.sh | sh
```
Ubuntu 會開始下載 Heroku CLI 並進行安裝，安裝結束後可輸入 heroku -v 指令來確認安裝成功

```
$ heroku -v
heroku-cli/6.13.19 (linux-x64) node-v8.3.0
```
現在，我們就可以在自己的本地主機上使用 Heroku 了！

##登入 Heroku
讓我們用以下這行指令連線到 Heroku：

```
$ heroku login
```

輸入這行指令的效果就像你用瀏覽器連到 https://id.heroku.com/login 一樣，接下來 Heroku 主機會要求你輸入帳號與密碼，我現在輸入我的，你要輸入你的：

```
$ heroku login
Enter your Heroku credentials:
Email: ej.lin@alphacamp.co # 請輸入你的 Heroku 帳號
Password: **************** # 請輸入你的 Heroku 密碼
```

輸入正確就會出現歡迎訊息：

```
Logged in as ej.lin@alphacamp.co
```

##利用 Heroku CLI 建立應用程式
請注意，這一節建立的 Heroku 應用程式與上一章「在 Heroku 上建立帳號與應用程式」是相同的，只是上一章用人性化介面、這裡使用指令列。為幫助你盡快熟悉指令列的操作，我們強烈建議你跟著以下步驟來練習。



讓我們用 `heroku create` 指定一個響噹噹的名稱（album-of-ej-family）：

```
$ heroku create album-of-ej-family
Creating ⬢ album-of-ej-family...done
https://album-of-ej-family.herokuapp.com/ |
https://git.heroku.com/album-of-ej-family.git
```

如果你在 `heroku create` 指令後沒有加上名稱，Heroku 也會幫你自動生成：

```
$ heroku create
Creating app... done, ⬢ morning-basin-94262
https://morning-basin-94262.herokuapp.com/ |
https://git.heroku.com/morning-basin-94262.git
```

比較兩行回傳的訊息，你應該會注意到「⬢」後的應用程式名稱有所不同。
不論是哪一種生成方式，都會自動產生：

####網址（URL）：

* 例如 https://album-of-ej-family.herokuapp.com/ ，這個就是應用程式以後的 URL，等我們佈署上去之後，別人就可以使用。
Git repostory：
* 例如 https://git.heroku.com/album-of-ej-family.git ，這是 Heroku 給我們存放專案程式碼的儲存庫位置，也就是等下我們要把專案「推」上去的目標位置。



接下來就讓我們談談專案的資料庫設定，讓專案可以推上Heroku！

「疑？不是說好推上去就好了嗎，怎麼又有設定冒出來？那我用 Heroku 幹嘛？」

其實要改動的設定只有一個啦，就讓我們繼續看下去⋯⋯

##調整環境設定，為佈署 Heroku 作準備
安裝佈署到 Heroku 所需的 gem，並且將 Production 模式的資料庫設定為 PostgreSQL

佈署，從動作面來看，其實就是把存在你家電腦的程式碼，移到另一台主機上，讓其他人使用。

由於你家電腦和遠端主機不同，佈署前需要調整程式設定、佈署後需要去遠端主機裡去啟動應用程式，Heroku 佈署的基本動作也是如此。



##變更資料庫到 PostgreSQL
Heroku 主機上使用的資料庫是 PostgreSQL，但 Rails 內建的資料庫是 SQLite。SQLite 和 PostgreSQL 同為關聯式資料庫，但 SQLite 是一種單檔式資料庫，較適合單機開發，而相比之下，PostgreSQL 更適合佈署使用。

接下來，我們要修改資料庫的設定，讓你可以繼續保持本地開發用 SQLite，但佈署時改用 PostgreSQL。

## 將資料庫的 gem 移到對應的資料庫環境
Rails 預設有三種環境，在不同環境中，通常有不同的執行模式：

* development（開發模式）：你的本機開發環境
* test（測試模式）：執行測試使用
* production（正式上線模式）：也就是佈署之後的實際運作環境
請你打開專案資料夾底下的 Gemfile，細看 gem 的清單，你會發現 gem 分類在不同環境使用。現在，我們就要把本機開發用的 sqlite3 移到開發與測試群組，然後在正式的上線後用的 production 群組裡加上 pg（PostgreSQL）。
請修改你的 Gemfile，確保兩個 gem 出現在對的位置：

```
group :production do
  gem 'pg'
end
group :development, :test do
  gem 'sqlite3'
end
```

接著習慣上，我們會把佈署時用到的 gem 放在 group :production 中，避免在開發模式（development）或測試模式（test）下去用到這些 gem；如果沒有在 Gemfile 發現 group :production，就自己手動加上去吧！

修改完 Gemfile 後，請確認 gem 沒有重複，尤其 **sqlite3**，這會造成下一步出錯。

接著請根據自己的作業系統，安裝 postgresql 開發者套件，某些 gem 只負責與套件溝通，而不直接處理工作內容，所以必須先安裝套件，才能透過 gem 把工作交給套件去處理。



使用【 Ubuntu 】的人請使用以下安裝 postgresql：

```
Photo_album $ sudo apt-get install libpq-dev
```

使用【 Mac 】的人請使用以下安裝 postgresql ：

```
Photo_album $ brew install postgresql
```

安裝好 postgresql 後，請執行 bundle install 指令安裝 postgresql 的 gem：

```
Photo_album $ bundle install
```

如同剛才的動作，你可以試著在一大串訊息中找找 pg：

```
Installing pg 0.21.0 with native extensions # 有這行的話表示套件安裝了
Using pg 0.21.0                             # 有這行的話表示套件運行中
```

##修改資料庫設定檔
安裝完 gem 之後要記得告訴 Rails 他本人說 database 改成使用 PostgreSQL 了，這種設定被定義在 ~/config/database.yml 裡，這個文件負責控制專案資料庫使用的檔案。

請你打開這份文件， 4 種環境設定已經在裡面了：

* default
* development
* test
* production
我們要去 production 裡面加上上線用的參數：

```
production:
  adapter: postgresql
  encoding: Unicode
```

####請特別注意 yml 檔案內縮排有嚴格限制，用兩個空白鍵縮排，參數要在冒號後面空一格再填入。

這樣一來，Rails 應用程式就知道在線上模式中，要使用 PostgreSQL 資料庫了！

到此為止我們已經完成了所有佈署需要的環境設定，是時候該做個存檔，免得以後忘記當初到底是怎設定 Heroku 佈署的了，因此我們先不談「推」上去，那個太容易了，我們來聊聊怎麼存檔吧！


#利用 Git 將專案佈署到 Heroku 上
使用 Git 將程式碼推到 Heroku 的儲存庫之後執行資料庫遷移，完成應用程式佈署

##Heroku 與版本控制
一般的佈署過程，通常是將自己的程式碼用檔案形式傳到伺服器上，或是在伺服器上輸入指令，到你的代碼庫抓程式碼，再送進特定機器。

Heroku 更進一步把版本控制的精神結合在一起，我們只需要確認版本（在特定 commit 後的節點上），告訴 Heroku 說我要用的版本是現在這一個，然後從本地直接推到 Heroku 伺服器，就大功告成了！

##將程式碼更動加到版本控制裡
為了使用不同的資料庫，我們已經更動了不少檔案，因此我們先用 git 來建立新的版本紀錄（如果各位忘了 Git 是什麼，可以點這裡複習）。

過去我們在 Git 單元時都是使用 git add 把檔案逐個加入到更新列表（Staging Area），但今時今日的我們所寫的專案已經慢慢變得龐大，要逐個檔案加入更新列表會是一件十分麻煩的事；所以今天教大家一個新的指令，可以把所有的檔案一次過加入 Staging Area。

1.請在專案文件夾底下使用以下指令把所有檔案更動加入 Staging Area：

```
$ git add .
```

2.請使用 commit 建立新的版本紀錄，附上修改設定的註記：

```
$ git commit –m "add pg, update Gemfile.lock, change production db to pg"
```

將專案佈署到 Heroku 上去
專案已經準備好了，那我們現在就來「推」上 Heroku 吧！

1.首先，請使用以下指令從 Terminal 登入 Heroku

```
$ heroku login
```

2.用以下指令，設定你等下要「推」到 Heroku 上的位置，記得把專案名稱改成你自己的：

```
$ heroku git:remote -a album-of-ej-family
set git remote heroku to https://git.heroku.com/album-of-ej-family.git
```

3.利用 git push 指令把專案「推」上 Heroku：

```
$ git push heroku master
```
沒有問題的話最後面通常會顯示這些訊息：

```
remote: -----> Launching...
remote: Released v5
remote: https://album-of-ej-family.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/album-of-ej-family.git
* [new branch] master -> master
```
小提醒 ！

若沒有部屬成功，而出現錯誤訊息的話，應該是缺少了某一個步驟設定，請回到上一單元 調整環境設定，為佈署 Heroku 作準備仔細檢視部署步驟是否有誤！

4.最後別忘了遷移資料庫：

```
$ heroku run rake db:migrate
```
為什麼要遷移資料庫呢？在專案中，我們設定了各種資料表欄位、關聯等資料庫設定，因此，專案程式碼上傳之後，我們必須在 Heroku 伺服器上也執行一次 migrate ，這樣一來，我們在專案內設定的那些資料表關聯才能起作用，資料才能會按照我們想要的方式存進資料庫。

5.完成，開啟瀏覽器！

```
$ heroku open
```

佈署成功後，任何人都可以用 Heroku 給你的 URL 連到應用程式了！

#進入遠端 Heroku 控制台
運用 Heroku 指令查看 Log 以及進入遠端控制台

無論如何，佈署最容易遇到的挫折是，常常在本機開發時都很正常，只要伺服器與本地的環境有有些許不同，上 production 就是會出錯，所以我們需要能夠直接訪問伺服器的後台，循著 log 裡的線索找出哪裡有問題。
##瀏覽遠端 Log
所有登入伺服器的活動，都會在 log 裡留下紀錄。不急於一時，但隨著 debug 經驗值的提升，你將會愈來愈看懂 log 裡的訊息。

~~~
$ heroku logs --num 5
2017-06-28T08:34:59.969901+00:00 app[web.1]: Place Load (1.7ms) …
2017-06-28T08:34:59.971368+00:00 app[web.1]: Rendering …
2017-06-28T08:34:59.974502+00:00 app[web.1]: Place Load (2.4ms) …
2017-06-28T08:34:59.975160+00:00 app[web.1]: Rendered … (3.6ms)
2017-06-28T08:34:59.975503+00:00 app[web.1]: Completed 200 OK in 11ms …
~~~

查看紀錄的指令就是 logs，直接輸入 heroku logs 也可以看見「所有的」紀錄——然後你的指令列就會塞爆，所以不建議直接使用 logs 查看紀錄。如果你去看 log，通常是要找一些最近發生的事件。

另一種可能，是你想要一直掛在主機上，持續觀察 log 檔案的變化，就像是在本機使用 rails s 一樣，檢查伺服器的運作是否正常，你可以使用以下指令：\

（如果要跳出就按 Ctrl + C，與 linux 系統一樣的強制終止指令。）

~~~
$ heroku logs --tail
~~~

或是利用 --help 找尋心目中的指令：

~~~
$ heroku logs --help
~~~

打開遠端 Rails console
~~~
$ heroku run rails console
Running 3d85c6rails console on ⬢ album-of-ej-family... up, run.7850 (Free)
Loading production environment (Rails 5.0.2)
irb(main):001:0>
~~~

run 也是一個 Heroku 所提供的指令，就像是本地終端機，在 run 後面的指令與本地所需要輸入的指令相同，在本地使用的是 rails console 或是精簡版的 rails c 進入專案控制台，在遠端的話就是再加上 heroku run 進入控制台，所以同理，在 heroku 以下兩行指令的功能也是一樣：

~~~
$ heroku run rails console
$ heroku run rails c
~~~

接下來的路，就靠你了⋯⋯
有沒有注意到指令都是 heroku 開頭？因為這些指令都是我們安裝 Heroku CLI 來得，使用的邏輯類似 linux 系統，需要時，你可以進入官方文件查找，不過一般人不會直接查找官方文件，大家會去這個網站解決所有的問題。