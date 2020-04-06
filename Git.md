# Git & Gitlab
###### tags: `Gitlab` `Github`

- Git 相關教學網站
    - http://gogojimmy.net/2012/01/17/how-to-use-git-1-git-basic/
    - https://windsuzu.github.io/learn-git/
---
## 開始使用 Git (init, clone)
### 建立自己要控管的 Repository (init)
若有一個專案資料夾叫 **Feature_maps_visulize**，要用 git 管理此專案，首先進入該專案資料夾下，透過 `git init`對此資料夾建立Repository。

```
$ git init
```
![](https://i.imgur.com/3kHbj7l.png)

此時顯示於專案資料夾內新增了一個隱藏資料夾`.git`，已成功建立好一個新的 Git Repository。
### 複製別人的 Respoditory (clone)
於 Github 上看到別人的程式碼要抓下來自己修改，或團隊中別人的程式碼，在 Git 中可以看到一個網址存放該 Repository 
![](https://i.imgur.com/Ikj0lug.png)

複製起來後，透過 `git clone` 將該 Repository 複製到本地資料夾，同時也可以對該 Repository 重新命名，只要在網址後面加上新的名字即可，為 **HackMD_Docker**。
```
$ git clone https://github.com/changpowei/HackMD.git HackMD_Docker
```
---
## Git的基本功 (status, add, commit, log, .gitignore)
Git分為在**本地端數據庫**操作以及**遠端數據庫**的同步與共享，我們可以在本地端使用像是還原更改等所有Git版本控制功能，但若想要公開本地端的修改內容，或與他人共同修改內容，就必須要透過遠端數據庫的幫忙。

<font size=4 color=red>  **本地端數據庫**</font>
要將檔案進行控管，仍有一些步驟，分別是**工作目錄(Working Tree)**、**索引(Index)**、**數據庫(Repository)**。
- **工作目錄(Working Tree)** : 
    處理專案的==本地端目錄==，Git的操作指令都在這裡完成。
- **索引(Index)** : 
    將專案上傳到數據庫而準備的==暫存區==，索引的存在可以==排除工作目錄裡不必要的檔案提交==。
- **數據庫(Repository)** : 
    記錄==檔案或目錄狀態==的地方，儲存專案的==修改歷史記錄==，還可以==追蹤內容的狀態和版本==。
    
![](https://i.imgur.com/811xiKr.png)

- **提交流成** ：
修改檔案 :arrow_right: 加入 stage (git add) :arrow_right: 提交( git commit ):arrow_right: 繼續修改其他檔案


<font size=4 color=red>  **遠端數據庫**</font>
![](https://i.imgur.com/ZvbG1vp.png)

### 查看 Git 的狀態 (status)
可以輸入`$ git status`來檢查目前 Git 的狀態

![](https://i.imgur.com/eMZtF9e.png)

表示目前的工作目錄(working tree)是乾淨的，On branch master 表示你目前正在名為 master 的 branch 上。
此時新增 test.txt，再次查看整個狀態 `$ git status`

![](https://i.imgur.com/WWTLguh.png)

剛剛新增的 test.txt 檔案變成 Untracked files (未被追蹤的檔案)，此狀態稱為**unstage**。

### 將檔案加入追蹤 (add)
使用 `git add` 這個指令來把它加入追蹤：
```
$ git add test.txt
```
![](https://i.imgur.com/Hdi5DJl.png)

此時看到 test.txt 已經被加入追蹤成為 **stage** 狀態，已經待命被提交到數據庫。

<font color=red>*Tips* :</font> `$ git add . `一次將所有修改過得檔案，加入 **stage** 狀態

### 提交 stage 狀態之檔案 (commit)
commit 在 Git 中就是一個節點，這些 commit 的節點就是未來你可以回朔及追蹤的參考，如同電玩遊戲時的存檔，每一個 commit 就是一次存檔，讓我們未來在需要的時候都可以回到這些存檔時的狀態。
```
$ git commit
```
![](https://i.imgur.com/LcQhj5F.png)

視窗中最上面的空行是要給你寫下你這次 commit 的訊息，例如在這裡寫下  “新增test.txt檔案進入repository!!!!!!!” 來表示這一次提交的目的，強烈建議在 commit 的時候要<font color=red>**清楚表達這次 commit 的內容為何**</font>，因為這會讓你未來要回頭看 code 的時候能讓你快速的找到你想要找的內容，也能對團隊中其他成員了解你在做什麼。

在 commit 完後會顯出出你這次 commit 的更動：

![](https://i.imgur.com/mQ1JJ77.png)

也可以在 commit 時加上 -m 的參數來快速提交，同時留下提交的訊息：

```
$ git commit -m “新增test.txt檔案進入repository!!!!!!!”
```

### 查看過去 git 的紀錄 (log)
使用 `$ git log` 的指令查看過去 commit 的紀錄：

![](https://i.imgur.com/SC7AYYD.png)

亂碼是此次 commit 的版號，後面的是 commit 的作者、時間，以及 commit 的訊息。

可以使用 `$ git log --stat` 參數看到更詳盡的訊息:

![](https://i.imgur.com/FCprATA.png)

### 忽略某些檔案不加入追蹤 (.gitignore)
有些檔案我們不希望加入版本控制的追蹤，例如說Database的schema或是一些log檔，這時候你可以將他們加入 .gitignore 中來讓 Git 忽略他們，使用編輯器來打開你的 .gitignore 檔案。
```
$ vim .gitignore
```
假設不追蹤ignore.txt，則在 .gitignore 新增：
![](https://i.imgur.com/782LZSC.png)

---
## Git Branch的操作流程

:notebook: 相關指令大全：
http://blog.gogojimmy.net/2012/02/29/git-scenario/

### 列出所有的 branch (branch)

`$ git branch` 這個指令可以列出所有的 branch 並告訴你目前正在哪個 branch：

![](https://i.imgur.com/SVdf9N9.png)

*此顯示目前為master的branch。

---

### 開新的branch (branch +<font color=green>"new branch name"</font> )

若要新增一條支線叫 new branch ，則可透過 `$ git branch new_branch` 新增

![](https://i.imgur.com/ywraqrV.png)

顯示多了一支 new_branch 的支線，目前仍在master主線上。

可透過在repo資料夾下執行 `$ gitk` ，可以看到 master 和 new branch 在同一個水平線上，代表兩個分之的目前狀態是一樣的。改打指令 `gitk --all &` 來讓 gitk 在背景執行。

![](https://i.imgur.com/Ji7q10B.png)

---
### 切換不同的branch (checkout + <font color=green>"branch name"</font>)

使用 `$ git  checkout ` 來切換不同的支線：

![](https://i.imgur.com/BypG0vX.png)

在 new branch 下新增檔案新增 new_branch_test.txt，並將它 add 到 stage 後再 commit 它。

![](https://i.imgur.com/p4NC14z.png)

可以看到 new_branch 已經比 master 多了一個 commit 的內容！

![](https://i.imgur.com/k2lCcgz.png)

切回 master 主線新增測試檔案，可以發現並沒有 new_branch_test.txt 

![](https://i.imgur.com/1FEXQlR.png)

此時在主線上新增另一個測試檔案 master_test.txt，透過 `$ gitk --all &` 可以看到 master 和 new_branch 已經產生分歧，由於 master 最後 commit 所以節點在上面。

![](https://i.imgur.com/0Jn397d.png)

---
### 整理現在的 branch (rebase)

與 `$ git merge` 不同的是， `$ git rebase` 不單單只是將兩個不同的 branch 合併起來，而是將**某一支 branch 基於另一支 branch 的內容合併起來**，也就是 `$ git rebase` 會基於 master branch 目前最後一次的 commit 內容，再往後把 new branch 上commit 的內容加上去。

在 new branch 輸入 `$ git rebase master` 來將 new branch 基於 master branch 做 rebase。

![](https://i.imgur.com/mHWSRRa.png)

不同於 `$ git merge` 的線圖會把 new_branch 合併到 master， 而是把原本的 new_branch 接到 master 因此只有一條線，**如同用最新的master 更新 new_branch**，當一個專案有很多的 branch 再做開發的時候會避免很多 branch 的線接來接去難以辨認。

---
### 比較不同branch的差異 (diff)

當前的 branch 與其他 branch 有哪些差異，你可以使用 `$ git diff master new_branch` 的指令去觀察，顯示為==後面比前面多了或少了哪些東西==：

![](https://i.imgur.com/nw2b1uK.png)

顯示 new_branch 比 master 多了 new_branch_test.txt，其 new_branch_test.txt 內文為 "在支線新增文檔!"。

---
### 合併兩個branch (merge)

開發完畢時，我們會把開發好的東西合併回 master 很自然的我們通常都會使用 `$ git merge` 這個指令來合併兩個branch。

在 master 下`$ git merge new_branch`  這個指令來告訴 git 要 merge new_branch 到現在所在的 branch 。

![](https://i.imgur.com/dwGg18g.png)

---
### 取消提交，回到之前提交的版本 (reset)
- -\-hard
     完全不保留原始 commit 結點的任何資訊，會連同資料夾中實體檔案內容都進行重置，也就是直接將工作區、暫緩區及 git 目錄都重置成目標Reset結點的資料內容。
     - **具體操作**
     `$ git reset --hard HEAD~ ` ：  回復到最新提交版本 
    `$ git reset --hard HEAD~1`  ： 回復到上一個提交版本 
    `$ git reset --hard HEAD~n`  ： 等於往上第n個提交版本 回復之前指定的提交版本
    or
    `$ git reset --hard commit_id` ： 根據 commit id 回覆到指定版本
    
     - **步驟**
         1. `$ git reflog` ：查看所有訊息版本
         
             可以看到目前所在版本為5f516ae
         
        ![](https://i.imgur.com/AgOhPKq.png)
         
         2. `git reset --hard 045a1d8 `： 回復到指定版本  045a1d8
         
             回復到刪除kernel inspection時所提交的版本， 資料夾內沒有Kernel Inspection
         
         ![](https://i.imgur.com/jOUvtUC.png)
         
- -\-soft
    此模式下會保留工作區資料內容，不會異動到目前所有的實體檔案內容；也會保留暫緩區資料內容，讓暫緩區與 git 目錄資料內容是一致的。

---
### 創建一個新的提交，來回到之前的提交 (revert)
`$  git revert HEAD ` 新增一個 Commit 來反轉（或説取消）另一個 Commit 的內容，原本的 Commit 依舊還是會保留在歷史紀錄中。雖然會因此而增加 Commit 數，但通常比較適用於已經推出去的 Commit，或是不允許使用 Reset 或 Rebase 之修改歷史紀錄的指令的場合。

revert 為提交一個新的commit，其內容==執行的動作為指定commit的相反動作==。例如：上一次commit是**刪除**某個檔案，執行 `$  git revert HEAD~1` 則會**新增**該刪除的檔案，並提交一次commit。

目前資料夾如下：

![](https://i.imgur.com/N6BX6Ah.png)

將 Kernel Inspection.py 刪除並重新commit，資料夾下已沒有該檔案。

![](https://i.imgur.com/6MzY2mp.png)

透過 `$ git revert  HEAD` 執行上次提交的相反動作，並commit新的版本，其資料夾內又出現剛剛刪除的檔案。

![](https://i.imgur.com/FBNpVtY.png)