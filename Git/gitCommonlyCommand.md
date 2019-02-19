## `Git` 配置

可以通过 `git config -l` 查看 Git 的相关配置

## 设置 `Git` 账号

```
// --global为设置全局，不加则设置当前文件的git用户
git config --global user.name 'hzu_zuoxiong'
git config --global user.email 'hzu_zuoxiong@qq.com'
```

## 创建/拷贝 `Git` 仓库

```
// 创建Git仓库
git init

// 拷贝Git仓库
git clone git@github.com:Hzu-zuoxiong/MeetingRoomManagementSystem.git
```

## `Git` 提交

```
// 查看当前分支的内容状态
git status

// 把已跟踪的文件放到暂存区
// 合并时把有冲突的文件标记为已解决状态
git add

// 从暂存区中一处文件
git rm --cached README

// 将暂存区内的内容提交到当前分支
git commit -m '提交描述'
// 撤回已commit过的内容文件，重新进行提交
git commit --amend
// 查看本地已commit，但未push的版本
git cherry -v

// 将当前分支提交到远程分支
git push <远程主机名> <本地分支名>:<远程分支命>
// 将本地分支 test 推送到可以实现重命名
git push origin test:remote-test
// 利用该用法，可以推送空分支到远程，实现远程分支的删除
git push origin :remote-test     ===     git push origin --delete remote-test
```

## `checkout`

```
// 切换到某个历史版本
git checkout <commit id>

// 从当前分支中创建并切换成新分支
git checkout -b new-branch

// checkout 除了切换，还有一个撤销作用（直接把原文件还原，取消之前对该文件的所有修改）
git checkout <filename>
```

## `branch`

```
// 查看本地分支
git branch

// 查看远程分支
git branch -r

// 删除本地分支
git branch -d <分支名>
git branch -D <分支名>（强制删除）

// 新建分支
git branch <分支名>

// 修改分支名称
git branch -m <旧分支名> <新分支名>
```

## 版本回退

```
// 通过 git log 查找要回退的历史版本
git reset --hard <commit id>
```

## 分支合并

```
// 当前分支合并主分支
git merge master
```

## `Git` 日常开发流程

日常工作中，冲突产生的情况通常是在你开发新功能的过程中，同事新上线了一个功能，导致 `master` 主分支更新，而你的分支没有及时更新导致的。所以，当你完成需求之后，一般要先拉取 `master` 分支，将其更新至最新版本，再与 `master` 进行合并。

```
// 首先，更新 master 分支
git pull

// 接着，在 master 最新版本中新建一个 20190128_frontEnd_note_1.0.0_useGit 分支
git checkout -b 20190128_frontEnd_note_1.0.0_useGit

// 在新建的分支里面进行开发，完成开发后合并 master 解决冲突
git pull   // 在 master 分支拉取
git merge master   // 在 20190128_frontEnd_note_1.0.0_useGit 分支合并 master
```
