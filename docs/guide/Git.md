
# Git

## init

创建新仓库

```shell
git init
```

## clone
```shell
git clone username@host:/path/to/repository

```

## add

```shell
// 第一次拉去代码
git pull origin master

// 新增代码
git add .

// 提交
git commit -m "第一次提交"

// 提交到远程
git push origin master
```


## Rebase 合并

该命令可以让和 `merge` 命令得到的结果基本是一致的。

通常使用 `merge` 操作将分支上的代码合并到 `master` 中，分支样子如下所示

![](https://user-gold-cdn.xitu.io/2018/4/23/162f109db27be054?w=505&h=461&f=png&s=22796)

使用 `rebase` 后，会将 `develop` 上的 `commit` 按顺序移到 `master` 的第三个 `commit` 后面，分支样子如下所示

![](https://user-gold-cdn.xitu.io/2018/4/23/162f11cc2cb8b332?w=505&h=563&f=png&s=26514)

Rebase 对比 merge，优势在于合并后的结果很清晰，只有一条线，劣势在于如果一旦出现冲突，解决冲突很麻烦，可能要解决多个冲突，但是 merge 出现冲突只需要解决一次。

使用 rebase 应该在需要被 rebase 的分支上操作，并且该分支是本地分支。如果 `develop` 分支需要 rebase 到 `master` 上去，那么应该如下操作

```shell
## branch develop
git rebase master
git checkout master
## 用于将 `master` 上的 HEAD 移动到最新的 commit
git merge develop
```

## stash

`stash` 用于临时报错工作目录的改动。开发中可能会遇到代码写一半需要切分支打包的问题，如果这时候你不想 `commit` 的话，就可以使用该命令。

```shell
git stash
```

使用该命令可以暂存你的工作目录，后面想恢复工作目录，只需要使用

```shell
git stash pop
```

这样你之前临时保存的代码又回来了

## reflog

`reflog` 可以看到 HEAD 的移动记录，假如之前误删了一个分支，可以通过 `git reflog` 看到移动 HEAD 的哈希值

![](https://user-gold-cdn.xitu.io/2018/4/23/162f14df98ce3d83?w=950&h=118&f=png&s=77151)

从图中可以看出，HEAD 的最后一次移动行为是 `merge` 后，接下来分支 `new` 就被删除了，那么我们可以通过以下命令找回 `new` 分支

```shell
git checkout 37d9aca
git checkout -b new
```

PS：`reflog` 记录是时效的，只会保存一段时间内的记录。

## Reset

如果你想删除刚写的 commit，就可以通过以下命令实现

```shell
git reset --hard HEAD^
```

但是 `reset` 的本质并不是删除了 commit，而是重新设置了 HEAD 和它指向的 branch。

## 远程协作流程

### 开源项目

- fork dev 分支
- 克隆代码
```javascript
git clone 你本地.git

// 查看远程分支
git remote -v

// 添加远程分支别名
git remote add devmaster 远程地址
```

- 修改本地代码第一次提交
```javascript
git add .
git commit -m "feat: 提交"
git push origin master
```

- 第二次修改前先更新代码
```javascript
git fetch devmaster
git merge devmaster/dev
```

- 提交pr，请提交到主干**dev**分支上，提交请遵循基本规范
```javascript
<type>: 提交描述

type

fix：修复 xxx Bug
feat：新增 xxx 功能
test：调试 xxx 功能
style：变更 xxx 代码格式或注释
docs：变更 xxx 文档
refactor：重构 xxx 功能或方法
```

<a name="5f739c16"></a>
### 私有项目

- 克隆开发的分支
```javascript
git clone -b 分支名称 分支地址
```

- 提交代码
- 合并分支

## 清除历史敏感文件

```shell
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch 敏感文件路径" --prune-empty --tag-name-filter cat -- --all
git push origin master --force
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now
git gc --aggressive --prune=now
```

## 错误汇总

* `npm install` 出现 `Unexpected end of JSON input while parsing near`的错误

我们可以先清除缓存 
```
npm cache clean --force
```
再次执行 `npm install`，如仍然出现原错误，可执行下句代码：
```
npm config set registry http://registry.cnpmjs.org
```

将`npm`代理重置。