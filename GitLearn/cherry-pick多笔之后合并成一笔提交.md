### git cherry-pick commit-idA commit-idB commit-idC
https://blog.csdn.net/qq_41084438/article/details/123783673

### 合并到多笔commit修改 git rebase -i HEAD~3
https://blog.csdn.net/SmileToLin/article/details/125630862

1.合并到多笔commit修改
在使用git时经常会遇到多笔commit修改同一个问题的情况，在提交时，往往只希望将这些commit作为一笔commit提交，可以通过commit指令达到这个目的。假设我们希望将最近的N笔commit合并成一笔

首先执行命令 git rebase -i HEAD~N

例如，我们希望合并最近的3笔commit，则命令为 git rebase -i HEAD~3

接下来，系统会打开一个编辑工具，内容如下
```
 pick 16212a5 main change
 pick 1343e22 remove log
 pick 2a21096 remove empty line

 # Rebase XXXXXXXXX
 # XXXXXXXXXXXXXXX
 # ......
```

如果不希望branch的commit记录中有2，3笔commit的记录，我们可以将上面的文本修改成下面的样子，然后保存
 ```
 pick 16212a5 main change
 squash 1343e22 remove log
 squash 2a21096 remove empty line

 # Rebase XXXXXXXXX
 # XXXXXXXXXXXXXXX
 # ......
```

接下来，git会帮我们进行rebase的动作，rebase执行完后，git会启动一个编辑器让我们填写合并后的新的commit的记录，填写完成后保存。
最后使用git log命令查看一下，可以看到，原先三笔commit中的改动都被合并到一笔新的commit中提交了。
