#創造與設定
##STEP 1：請打開你的 iTerm2，可以透過 Mac 的搜索工具 Spotlight 去做。


##STEP 2：如果你在這個時候輸入 subl test.rb
```
Terminal
$ subl test.rb
```
你應該會得到一個錯誤的訊息，這是我們所期待的，請再輸入 clear 把視窗清除。

##STEP 3：進入 $HOME 資料夾
由於 .bash_profile 必須放在 $HOME 的路徑下（也就是 MacOS 系統預設給使用者的資料夾），我們需要先確保自己在對的路徑下，請使用 cd 指令移動到對的位置：

```
Terminal
$ cd $HOME
```

cd 是「切換目錄」(change directory) 的意思。

你可以使用 pwd 指令確認當前的路徑，應該會看到類似以下的過程，當然裡面顯示的使用者是你的名字：

##STEP 4：輸入 vi .bash_profile

```
Terminal
$ vi .bash_profile
```

執行指令後，畫面會切入一個新打開的編輯器。這個編輯器的名字為"vi"。跟一般的編輯器不太一樣，它是一個以指令操作的編輯器。


##STEP 5：基本的 vi 操作
從這裡開始，我們就直接操作 vi。

首先輸入 i 進入 insert 模式
你會看到 INSERT 這個字出現在 vi 視窗的左下角裡，表示 vi 已經進入「編輯模式」準備可以給您在上面寫新的內容了。

接下來，請複製(command-c)下面這段程式碼，貼在 vi 視窗 (command-v) 裡：

```
alias stree='/Applications/SourceTree.app/Contents/Resources/stree'
alias subl="'/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl'"

function parse_git_dirty {
  if [[ $(git status 2> /dev/null | tail -n1) == "nothing to commit, working tree clean" ]]; then
    echo "✔ "
  else
   echo "✘ "
  fi
}

function git_branch {
    ref=$(git symbolic-ref HEAD 2> /dev/null) || return;
    echo "("$(parse_git_dirty)${ref#refs/heads/}") ";
}

export PATH=$HOME/bin:/usr/local/bin:$PATH
export PS1="[\[\033[1;32m\]\w\[\033[0m\]] \[\033[0m\]\[\033[1;36m\]\$(git_branch)\[\033[0;33m\]\\[\033[0m\]$ "
export EDITOR="subl"
```

然後請按 esc 鍵，再輸入 :x。
esc 鍵是讓 vi 回到「指令模式」，然後 :x 是 vi 的指令，意思是「儲存並離開」(save-and-quit)。注意，x 一定要小寫。


操作 vi 對新手來說比較複雜，如果中間有任何錯誤，建議按 esc 鍵，輸入 :q! 離開，再回到 STEP 5 重新開始。



##STEP 6：載入 .bash_profile 裡的設定
現在您應該回到 Terminal 了，接下來我們需要讓剛才貼入的那段程式碼生效。請輸入source .bash_profile：

```
Terminal
$ source .bash_profile
```

source 這個指令是將設定檔的內容 (也就是 .bash_profile 的內容)，讀進來目前系統環境中。