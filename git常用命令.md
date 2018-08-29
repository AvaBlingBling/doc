## SSH配置

```
git config --global user.name "zhouye"

git config --global user.email "zhouye2@kingsoft.com"

ssh-keygen -t rsa -C "zhouye2@kingsoft.com"

// 仓库服务器添加生成的pub密钥
```

## 

```
// 生成本地分支，并将远程分支映射到本地分支
git checkout -b local-branchname origin/remote_branchname
```

### 新建分支，合并分支

1. 你在某个分支上工作，突然接到bug需要紧急修补
2. 提交当前分支代码

    git add -a -> git commit
3. 切换到你的线上分支

    git checkout <span style="color:grey" title="分支名">online-branch</span>
4. 为这个紧急任务新建一个分支，并在其中修复它。-b代表以当前分支为基础。

    git checkout -b <span style="color:grey" title="分支名">fix-branch</span>

    git commit
5. 测试通过之后，切换回线上分支

    git checkout <span style="color:grey" title="分支名">online-branch</span>
6. 合并这个修补分支

    git merge <span style="color:grey" title="分支名">fix-branch</span>

    冲突解决：git add -> git status -> git commit

    git push 
7. 删除紧急修补分支

    git branch -d <span style="color:grey" title="分支名">fix-branch</span>
8. 切换回你正在工作的分支继续你的工作
