
================= 
開始使用 git 之前
=================

在還沒建立任何 git 的工作目錄之前，應該要先建立你自己的使用者名稱及電子信箱地址，這樣 git 便會將這些資訊存在工作目錄的日誌中

::

  git config --global user.name "Your Name"
  git config --global user.email "your@email.address"

============
建立工作目錄
============

假設你要開始一個全新的專案，並使用 git 來管理源碼。

建立一個全新的工作目錄：

::

  mkdir projectname
  cd projectname
  git init

之後開始撰寫程式，於是你可能產生了好幾個源碼檔案，想要將它們交由 git 管理：

::

  git add file1 file2 file3

如果你懶得打字，也可以這麼做：

::

  git add .

========
送交改變
========

你已經告訴 git 有那些檔案想要交由它管理，再來你必須告訴 git 記錄這些改變：

::

  git commit

輸入以上命令後，git 會呼叫預設的編輯器讓你編輯想要輸入的訊息，git 會以這訊息當做改變的記錄

也可以：

::

  git commit -m "Commit Message" # 直接將訊息代入

於是當你想要查看記錄時只要打上：

::

  git log

便可以看到你之前所送交改變的訊息

====
分支
====

當你建立工作目錄並送交第一個改變記錄，git 會為你建立一個名為 *master* 的預設分支，當你不斷地修改程式的時候，你可能會有想要嘗試添加某些新的功能，但並不想影響到目前的工作，這時候便是建立一個分支的時候。

在新的分支裡，你可以恣意的修改並不用害怕會影響到你的其他分支。若在新的分支裡改壞了，你只要將它刪除，一切就像沒發生一樣。若覺得新的分支令人滿意，你可以將這個分支合併到你主要的工作的分支裡。

建立一個分支：

::

  git branch experiment

此時，你便已經建立一個名為 *experiment* 的分支了。

切換至某個分支：

::

  git checkout experiment

於是你便已經從 *master* 分支切換至 *experiment* 分支了。

以上兩個命令可以簡化成一個命令來完成：

::

  git checkout -b experiment

刪除某個分支：

::

  git branch -D experiment

查看目前有那些分支：

::

  git branch

====
標籤
====

當你的專案進行到某種程度，你可能想要將它訂定一個版本號以便更容易辨識與找尋，此時便是標籤登場的時候。

建立一個簡易的標籤：

::

  git tag v1.0

你也可以將標籤加上一些註解：

::

  git tag -m "Some Descriptions" v1.0

顯示目前有那些標籤：

::

  git tag

顯示目前有那些標籤及註解：

::

  git tag -n

====
合併
====

假設你的分支完成了你所需要的新功能的添加，並且結果令人滿意，你想要將它合併到你的主要開發分支中。

先切換到你的主要開發分支中：

::

  git checkout master


將你實驗用的分支合併到當前的分支中：

::

  git merge experiment

====
衝突
====

一般情況下，合併通常令人滿意地順利完成，但是世界並不是一直這麼美好，你可能在合併中看到以下的訊息，告訴你合併失敗因為有衝突：

::

  Automatic merge failed; fix conflicts and then commit the result.


假設有衝突的是名為 file.txt 的檔案，當你用編輯器將它開啟時，你會看到類似以下的衝突標記：

::

  <<<<<<< HEAD:file.txt
  Hello world
  =======
  Goodbye
  >>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt

此時你必須決定該使用哪一部份，修改完成並儲存之後，輸入：

::

  git add file.txt

再次送出改變記錄：

::

  git commit

衝突便順利的解決了。

========
取消改變
========

若你已經在工作目錄中做了一些改變，經過檢視後發現這些改變不如人意，想要回復成前一次改變，可以輸入：

::

  git checkout -f


若你想要回復到某次改變，則可以輸入：

::

  git reset --hard HEAD^ # HEAD^ 表示上上一次的改變

或是：

::

  git reset --hard 9ae878f7f9410bc2f1555276036f52ddca1385c0 # 以某次改變記錄的 SHA1 id 來表示

或是：

::

  git reset --hard v1.0 # 以標籤名來表示

若你想要回復到某次改變，但是想要保留你已經做的修改，則可以輸入：

::

  git reset --soft HEAD^

或是想要恢復到某次改變，並撰寫訊息以說明為何做出如此處理，則可以輸入：

::

  git revert HEAD # 回復到上一次的改變

====
差異
====

比較你所做的修改與前一次的改變記錄的差異：

::

  git diff

比較某個檔案的差異：

::

  git diff file1

比較兩個分支之間的差異：

::

  git diff master experiment

比較兩個分支之間某個檔案的差異：

::

  git diff master experiment -- file1

比較兩個標籤之間的差異：

::

  git diff v1.0 v1.1

比較兩個改變記錄之間的差異：

::

  git diff 79a8 823b # 並不需要將改變記錄的 SHA1 id 全部打上，通常前面幾個數字便已足夠

==========
與他人合作
==========

假設你有位同事已經進行某項專案一段時間，你想要對他的專案做些研究或幫助，而你的這位同事已經將他的專案於網路上公開。首先你必須取得他的專案的副本：

::

  git clone ssh://somehost/someone.git # 其他還有 git，http 等傳輸協定

之後便如同前所述一樣，開始做屬於你自己的改變。當你滿意你所做的改變，並想要更新你的同事的專案，則輸入：

::

  git push origin master

此處的 *origin* 代表了你所取得的遠端來源的別名， *master* 則表示你想將你的 *master* 分支所做的改變更新到遠端的 *master* 分支裡。

若你的同事也持續的做改變，你想要取得你的同事所做的改變：

::

  git pull

====
其他
====

內部物件
--------

git 內部以幾個物件來代表它所管理的資料，分別是 blob，tree，commit，tag。

blob 表示檔案，tree 表示目錄，commit 表示改變記錄，tag 則是標籤。

每一個物件都有一個獨一無二的 SHA1 40 個字元十六進位表示的 id，經由此 id 可以指涉到如前所述的四種物件。

命名法
------

除了以 SHA1 來指涉物件外，git 還提供了一些符號及別名來指涉內部物件：

- HEAD

  當前工作目錄分支的最新 commit 

- origin

  預設的遠端來源的別名

- master

  預設的分支名稱

- ^

  此符號後面若跟隨著數字，則代表著某個 commit 的第幾個父 commit ( 因為 commit 可能經由合併而有好幾個父 commit )，若沒跟隨著數字則代表第一個父 commit。

  ::

    HEAD^ 表示 HEAD 的第一個父 commit
    HEAD^^ 表示 HEAD 的第一個祖父 commit

- ~ 

  此符號後面跟隨著數字，代表著某個 commit 的前幾代的第一個父 commit。

  ::

    HEAD~3 等同於 HEAD^^^
