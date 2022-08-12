### 1. git status -s

--查看仓库状态

### 2. git add +文件名

--添加需要提交的文件名（加路径--参考git status 打印出来的文件路径）

### 3. git stash -u -k

--忽略其他文件，把现修改的隐藏起来，这样提交的时候就不会提交未被add的文件

### 4. git commit -m "xxx"

### 5. git pull

### 6. git push

--推送到远程仓库

### 7. git stash pop

--恢复之前忽略的文件（非常重要的一步）

### 8. git reset HEAD

-- 回退暂存区里的文件 (取消git add操作)

### 9. git reset HEAD "xxx"

-- 回退暂存区里的指定文件(取消git add操作)

### 10. git rm file_path

-- 删除暂存区和分支上的文件，同时工作区也不需要

### 11. git rm --cached file_path
