## Git配置
可以通过 `git config -l` 查看Git的相关配置

## 设置Git账号
```
git config --global user.name 'hzu_zuoxiong'
git config --global user.email 'hzu_zuoxiong@qq.com'
```

## 创建/拷贝Git仓库
```
// 创建Git仓库
git init

// 拷贝Git仓库
git clone git@github.com:Hzu-zuoxiong/MeetingRoomManagementSystem.git
```

## Git提交
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

// 将当前分支提交到远程分支
git push <远程主机名> <本地分支名>:<远程分支命>
// 将本地分支 test 推送到可以实现重命名
git push origin test:remote-test
// 利用该用法，可以推送空分支到远程，实现远程分支的删除
git push origin :remote-test     ===     git push origin --delete remote-test
```

## checkout
```
// 切换到某个历史版本
git checkout <commit id>

// 从当前分支中创建并切换成新分支
git checkout -b new-branch

// checkout 除了切换，还有一个撤销作用（直接把原文件还原，取消之前对该文件的所有修改）
git checkout <filename>
```

## branch
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