## 常见命令

[详细地址](http://www.cheat-sheets.org/saved-copy/git-cheat-sheet.pdf)

[git入门和进阶](http://git.mydoc.io/?t=154712#category_19221)

[Mac 一个实用工具小站](https://www.jianshu.com/p/5965095d7835)

[Mac 更新node版本](https://www.jianshu.com/p/71c82fc63522)

- git push 需要输入用户名和密码
```markdown
git config credential.helper store
```


- git 添加gitignore文件，忽略某些文件，避免其提交
```markdown
一.先在项目路径下添加.gitignore文件
在Git Bash应用中输入 touch .gitignore 指令
二.编辑.gitignore文件，其实就是输入一些相对路径或者通配符来避免文件提交。


# 此为注释 – 将被 Git 忽略

      *.a       # 忽略所有 .a 结尾的文件
      !lib.a    # 但 lib.a 除外
       /TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
       build/    # 忽略 build/ 目录下的所有文件

       doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```

- 删除项目文件
```markdown
1. 方法一
cnpm install rimraf -g
rimraf node_modules

2. 方法二
cnpm install -g dlf 
dlf  C:\Users\92809\Desktop\12

```

## git常用指令
- git --version   //查看git的版本信息
- git config --global user.name   //获取当前登录的用户
- git config --global user.email  //获取当前登录用户的邮箱
- git config --global user.name 'userName'    //设置git账户，userName为你的git账号，
- git config --global user.email 'email'

- git status   //查看当前代码状态  红色未提交  绿色代表代码在缓存区  蓝色新建的未提交的
- git remote -v   //查看远程仓库
- git remote rm origin    //删除远程仓库

- git add index.html  //增加到暂存区中
- git add -A      //全部添加到缓存区  或  git add .   //把所有修改的代码放到缓存区

- git commit -m '备注信息'  //增加到版本库中
- git log --oneline   //查看版本
- git push origin (当前分支名)   //提交到远程仓库里面

## 撤销

- 撤销工作区的修改：git checkout -- 文件名
- 撤销暂存区的修改：git reset HEAD 文件名
- 回退到历史版本：git reset --hard 该版本ID
- 回退到上个版本：git reset --hard HEAD^
- 上上版本是HEAD^^，也可用HEAD~2表示，以此类推

## tag命令

- 为当前版本打标签：git tag 标签名
- 为历史版本打标签：git tag 标签名 该版本ID
- 指定标签说明：git tag –a 标签名 –m "标签说明" [可选：版本ID]
- 查看所有标签：git tag
- 查看某一标签：git show 标签名
- 删除本地标签：git tag –d 标签名
- 删除远程标签：git push origin --delete tag 标签名
- 提交指定标签：git push origin 标签名
- 提交本地所有标签：git push origin --tags

## 上传命令
- git add .  //增加到暂存区中
- git commit -m '备注信息'  //增加到版本库中
- git push origin (当前分支名)   //提交到远程仓库里面
- git remote add (接收分支名) （上传分支的git地址）


- git pull的作用是将远程库中的更改代码合并到当前分支中，默认为：git fetch + git merge
- git fetch 的作用就相当于是从远程库中获取最新版本到本地分支，不会自动进行git merge
- git pull –rebase 加上–rebase参数的原因是，在多人开发中，有多个merge commit，如果不加该参数，则有多个历史提交线，而它的作用，就相当于把分叉的提交线中的一条，每一次提交都捡选出来， 在另一条提交线上提交。最后也形成一条单一的提交线。
eg:git pull --rebase (接收分支) master