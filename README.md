Git and GitHub Basic Tutorial
===
Git installation
---
### Step1：Download directly from the official website
- 安裝過程中會有一個頁面叫你選擇預設主分支名
  **Adjusting the name of the initial branch in new repoistories**
  選擇下方的**Override the default branch name for new repoistories**
  將預設主分支名改為main，因為現在GitHub上的預設branch命名法已改為main，這樣到時候就不需要再重新命名
- 其餘一直Next即可
### Step2：After the installation is complete, open cmd
### Step3：Enter```git --version``` to confirm whether the installation is successful
### Step4：Enter```git config --global user.name "Your Name"``` to set user name
### Step5：Enter```git config --global user.email "Your email"``` to set user email
### Complete Git installation

***

Project folder initialization
---
### Step1：新增專案資料夾(所有的程式及相關檔案都放在這個資料夾內)，終端機的path切換到當前資料夾，使用git bash終端機或cmd都可以
### Step2：終端機輸入```git init```，做專案資料夾初始化，此時會自動新增git資料夾用來存放版本控制相關檔案
### Step3：自行新增檔案：
- ```README.md```：程式使用說明書
- ```.gitignore```：可在此檔案內輸入檔案名稱/*.副檔名等，讓git不要追蹤這些檔案
### Step4：local連結remote端(GitHub)，寫新Project才要做
- GitHub登入並點選 **"New Repoository"**，Repositories頁面右上角綠色按鈕
- 輸入基本Repo資料
  - Repository name
  - Description (optional)
  - 選擇權限Public or Private
  - 其他不用選
- Click **"Create Repoository"** to create a new repository
### Step5：點選 **"Create Repoository"** 後，滑到最下方會出現三行指令，直接複製貼上到你的資料夾的cmd即可
```
git remote add origin URL.git(此repo的網址)
git branch -M main
git push -u origin main
```
### Compelete Project folder initialization
### Step6：視情況從該repo/settings/Collaborators添加協作者
### Step7：之後如果要更新local端的記錄到remote上的話，只需```git push```
- ***但極度不建議直接這樣做***，後面會說明正確做法

***

Git常用指令(背)
---
- ```git add file1 file2... / git add *.副檔名 / git add .```：
  (1) 將未追蹤的file加入git追蹤/將特定副檔名的file加入git追蹤/全部file納入git追蹤(通常都是直接```git add .```)，有新的file就要用
  (2) 變更納入暫存區(存檔到git)，很常用，時不時就會輸入一次```git add .```來儲存狀態，刪檔案也要
- ```git status```：查看當前file的狀態
  - 文件有以下幾種狀態，在 VS Code 中，這些文件狀態會用不同的代號來表示：
  - U: Untracked 文件（未追蹤的文件），在左側文件資源管理器中會顯示一個 U 圖標
  - M: Modified 文件（已修改的文件），顯示為一個 M 圖標。
  - A: Added 文件（已暫存的文件），會顯示為一個 A 圖標，表示這個文件已經被 git add 暫存。
  - C: Copied 文件，表示這個文件是從另一個文件複製過來的。
  - D: Deleted 文件（已刪除的文件），表示這個文件已被刪除。
  - R: Renamed 文件，表示這個文件已被重命名。
- ```git commit -m "commit msg"```：提交當前節點，並輸入commit message(這個commit做了哪些事，更新了什麼東西？)
- ```git log```：查看log(按q退出log mode)，詳細顯示操作的ID(hash value)，及相關資訊
- ```git log --oneline```：查看簡要log，每個操作1行，只顯示ID和簡要資訊
- ```git relog```：查看更詳細的log，包含reset操作的ID(hash value)
- ```git diff```：顯示工作目錄中尚未暫存的變更與暫存區（索引）之間的差異。
- ```git diff HEAD```：顯示工作目錄中的變更與最近一次提交的差異，最近一次的commit稱為HEAD
- ```git diff <commit-hash>```：這會顯示當前工作目錄與指定提交之間的差異， ``` <commit-hash>```即比較點ID(用```git log```查)
- ```git diff <commit-hash1> -- <Filename>```：同上，但只查特定檔案
- ```git checkout <commit-hash1>```：將全部檔案還原到特定的commit時間點狀態
- ```git checkout <commit-hash1> -- <Filename>```：同上，但只針對特定檔案
- ```git reset --hard <commit-hash>```：謹慎使用，當前分支退回到指定commit，並刪除其後的記錄，如要還原，只能使用```git relog```再```git reset --hard <commit-hash>```
- ```git rm --cached <Filename>```：檔案移除追蹤
- ```git pull```：同步GitHub上的repo
- 還有很多git指令，如git rebase等，在此只列出常用指令，其他自己google

***

The right process for collaborating with Git and GitHub
---
***You should follow this process. Even if you are the only one working on the project.***
- **編輯子分支，再push上去remote，接著經過code review沒問題了，再merge到main分支，合併完後再刪除子分支**
- 使用此方法可確保main branch的穩定性，防止main branch的歷史變得混亂
- Step1：```git clone URL.git(此repo的網址)```
- Step2：```git checkout -b my-feature(custom sub-branch name)``` 從main當前節點延伸出一條子分支，並切換當前分支到子分支
- Step3：modify and update code
- Step4：```git diff``` Check what chages have been made
- Step5：```git add .```
- Step6：```git commit -m "commit msg"``` commit changes and create a node on sub-branch
- Step7：```git push origin my-feature(custom sub-branch name)``` push sub-branch onto GitHub
  - **if failed** => version conflict occurred(main branch updates after you cloned it)
  - Solution：sync latest version main branch to your sub-branch
  (1) ```git checkout main```
  (2) ```git pull origin main```
  (3) ```git checkout my-feature(custom sub-branch name)```
  (4) ```git rebase main```
  (5) Solve version conflict
  (6) ```git add .```
  (7-1) ```git rebase --continue```：Continue to rebase
  (7-2) ```git rebase --abort```：Abort rebase and restore (undo all previous rebase operations)
  (8) After completing rebase operation, ```git push -f origin my-feature(custom sub-branch name)```
  (9) If failed, goto (1)
- Step8：Click **"New Pull Request"** on GitHub and request code reviews and merges from other members.
  - After completing the code review and confirming that there is no problem, the sub-branch can be **"Squash and Merge"***(It will be deleted atomically after merged)* to main branch.
- Step9：
  - **if your sub-branch is merged to main branch on GitHub successfully:**
    (1) ```git checkout main```
    (2) ```git branch -D my-feature(custom sub-branch name)```：delete sub-branch on the local end
    (3) ```git pull origin main```：update main branch to latest version
  - **else if pull request failed(Usually the program is returned due to errors):**
    goto Step3