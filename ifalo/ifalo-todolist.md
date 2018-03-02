##連接資料庫
homestead 的資料庫設定了 MySQL 與 Postgres 兩種資料庫。為了方便使用，Laravel 的 .env 檔案預設會設定框架會使用此資料庫。

如果想要從本機資料庫client端連接你的 MySQL 或 Postgres 資料庫，你應該連接 127.0.0.1 連接埠 33060 (MySQL) 或 54320 (Postgres)。資料庫的帳號及密碼皆為 homestead / secret.

在本機電腦你應該只使用這些非標準的連接埠來連接資料庫。因為當 Laravel 執行於虛擬主機中時，你會在 Laravel 的資料庫設定檔使用預設的 3306 及 5432 連接埠。

##今日測試
####1.確定`.env`的環境變數設定正確
####2.範例如下：

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=33060
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```
####3.commend line下指令`php artisan migrate`
####4. `php artisan make:migrate`
####5. 

##git
1. `git checkout -b "branch name` 建立分支並跳轉到分支
2. `git merge master`
3. `git check`
4. `git reset head^`
5. `php artisan make:migration 2018_03_02_alter_test`


####
1. Check Database
2. Check Api
3. Check Git
4. MVC進階概念了解：Service、Repository、Presenter
5. 