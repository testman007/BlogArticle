#將專案佈署到 Heroku 上去
專案已經準備好了，那我們現在就來「推」上 Heroku 吧！

1. 首先，請使用以下指令從 Terminal 登入 Heroku

```ruby
Terminal
$ heroku login
```

2. 用以下指令，設定你等下要「推」到 Heroku 上的位置，記得把專案名稱改成你自己的：

```ruby
Terminal
$ heroku git:remote -a album-of-ej-family
set git remote heroku to https://git.heroku.com/album-of-ej-family.git
```

3. 利用 git push 指令把專案「推」上 Heroku：

```ruby
Terminal
$ git push heroku master
```

沒有問題的話最後面通常會顯示這些訊息：

```ruby
Terminal
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

4. 最後別忘了遷移資料庫：

```ruby
Terminal
$ heroku run rake db:migrate
```

為什麼要遷移資料庫呢？在專案中，我們設定了各種資料表欄位、關聯等資料庫設定，因此，專案程式碼上傳之後，我們必須在 Heroku 伺服器上也執行一次 migrate ，這樣一來，我們在專案內設定的那些資料表關聯才能起作用，資料才能會按照我們想要的方式存進資料庫。

5. 完成，開啟瀏覽器！

```ruby
Terminal
$ herokur open