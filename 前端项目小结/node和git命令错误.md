# node和git命令错误

## git对源代码进行push到其他分支时可能会出错,错误信息如下: error: failed to push some refs to 'xxx(远程库)'

 ![CSDN图标](https://luohaoyu.oss-cn-beijing.aliyuncs.com/dictionary/icon-information/ba7e192d582147f19e2dc0b472bef427.png "这是CSDN的图标")
- 解决方式：git pull --rebase githubRepo master 这里面的githubRepo是远程库的名字。

## windows和linux换行符不同导致的错误
```markdown
Windows下Git使用报错：
warning：LF will be replaced by CRLF in ××××.××(文件名)
The file will have its original line ending in your working directory.
原因是：
windows中的换行符为CRLF，而Linux下的换行符为LF（使用Git命令行Git Bash，实际上就是相当于linux环境），
所以在执行git add xxx.xx操作时，会出现这个错误提示！

解决方法：
1、如果版本库/项目还没被创建，执行以下操作：
git config --global core.autocrlf false     //在全局禁用自动转换
2、如果版本库/项目已经创建，使用非全局禁用自动转换
git config core.autocrlf false                    //在当前版本库中禁用自动转换
3、如果版本库/项目已经创建，在全局禁用自动转换则需要先删除之前创建的.git 文件后添加上面的设置。 
此操作很坑！是删除整个.git，会把跟远程的链接都断掉，整个git的提交历史/版本库都会被删除！
rm -rf .git
git config --global core.autocrlf false     //在全局禁用自动转换
完成后再重新执行git创建版本库操作：
git init
git add ...
git remote add ***
这样设置git的配置后在执行add操作就没有问题了。

```

## 本地分支与远程分支断开连接
```markdown
当 git push origin branch_name时遇到报错如下：
fatal:'origin' does not appear to be a git repository
fatal:Could not read from remote repository

解决方法：
cd 本地分支里
1、git branch 
               ——*master   只显示master

查看是否从上游仓库
2、git remote –v
             ——若什么都没有，则和上游已断联系，拉不了代码也推不了代码 


加关联
3、git remote add origin ssh://git@gitlab*********************************.git(地址)


然后
4、git fetch origin

              ——会显示下拉的branch情况

                    格式为From ssh://gitlab.********************************

                  *  [new branch]         XXXXX        ->origin/XXXXX
 
再次检查远程仓库，显示对应的clone地址
git remote –v
——origin  git://github.com/schacon/ticgit.git (fetch)
origin  git://github.com/schacon/ticgit.git (push)
 
然后再查分支
git branch –a

            ——* mater

                   remotes/origin/XXXXXX         **********
## git对源代码进行push到其他分支时可能会出错,错误信息如下: error: failed to push some refs to 'xxx(远程库)'

 ![CSDN图标](https://luohaoyu.oss-cn-beijing.aliyuncs.com/dictionary/icon-information/ba7e192d582147f19e2dc0b472bef427.png "这是CSDN的图标")
- 解决方式：git pull --rebase githubRepo master 这里面的githubRepo是远程库的名字。

## git push 出错
error: failed to push some refs to 'https://github.com/lvchunyan/electron-demo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```markdown
1.使用强制push的方法：

$ git push -u origin master -f 

这样会使远程修改丢失，一般是不可取的，尤其是多人协作开发的时候。

2.push前先将远程repository修改pull下来

$ git pull origin master

$ git push -u origin master

3.若不想merge远程和本地修改，可以先创建新的分支：

$ git branch [name]

然后push

$ git push -u origin [name]

```

## git提交到远程藏库冲突解决

```markdown
hint: Updates were rejected because the remote contains work that you do 
hint: not have locally. This is usually caused by another repository pushing 
hint: to the same ref. You may want to first integrate the remote changes 
hint: (e.g., 'git pull ...') before pushing again. 
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

原因: 两人同时fetch了一个分支。 第一个人修改后提交，第二个人提交就失败。

解决方法:
1.强制推送
$ git push -f 
可以提交，会将remote上第一个人的改动冲掉，比较暴力，不太好。

2.正常解决
先 git fetch origin 然后git merge origin/master, 和本地分支合并, 之后再push。
```
## npm audit fix
```markdown
1. 执行npm install 出现如下提醒
 
added 253 packages from 162 contributors and audited 1117 packages in 42.157s
found 5 vulnerabilities (1 low, 4 high)
run `npm audit fix` to fix them, or `npm audit` for details html

2. 按照控制台提示的命令，输入‘npm audit fix’后，控制台提示：

1 package update for 5 vulns involved breaking changes
(use `npm audit fix --force` to install breaking changes; or do it by hand)

3. 输入：‘npm audit fix --force’后，控制台提示：

added 199 packages from 111 contributors, removed 64 packages and updated 23 packages in 42.194sfixed 5 of 5 vulnerabilities in 1117 scanned packages
1 package update for 5 vulns involved breaking changes
(installed due to `--force` option)

4. 重新输入‘npm audit’：

=== npm audit security report ===
found 0 vulnerabilities
in 4598 scanned packages

终于一切正常。
```

## node踩坑之This is probably not a problem with npm. There is likely additional logging output above.错误
```markdown
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! my-vvp@1.0.0 dev: `webpack-dev-server --inline --progress --config build/webpack.dev.conf.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the my-vvp@1.0.0 dev script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\chunyan\AppData\Roaming\npm-cache\_logs\2019-06-17T01_21_34_748Z-debug.log

可能由于种种版本更新的原因需要执行
npm install
重新安装一次，如果还是不可以的话，在把之前装的都清空

cnpm install rimraf -g
rimraf node_modules
rimraf package-lock.json
npm cache clear --force
npm install

```

