# 03\_Git分支管理

\[TOC]

## 3.1 分支的简介

Git最重要的运用场景是多人协同开发，但是如何能保证每个人之间的开发不影响其他人的开发进程，Git 分支使得每个人之间的开发是独立的，互不影响的。

Git 鼓励在工作流程中频繁地使用**分支与合并**，哪怕一天之内进行许多次。 理解和精通这一特性，从此真正改变你的开发方式。

分支的本质：指向提交对象的一个可变指针

## 3.2 分支的相关操作

### 3.2.1 分支的增删改查

```bash
# 1. 查看分支 默认是 master 分支; -r(列出远程分支) -a(列出本地分支和远程分支)
git branch

# 2. 创建分支
git branch 分支名称

# 3.1 切换分支 checkout
git checkout 分支名称

# 3.2 切换到新创建的分支上 -b
git checkout -b 分支名称

# 4.1 删除本地分支 -d  当前所在的分支不能删除
git branch -d 分支名称

# 4.2 删除远程分支 origin主机
git push origin --delete 分支名称

# 5.1 重命名本地分支，-m 如果新分支名字已存在，则需要使用-M 强制重命名
git branch -m 旧分支名称 新分支名称
git branch -M 旧分支名称 新分支名称

# 5.2 重命名远程分支 即改名后的分支推送到远程
git branch -m 旧分支名称 新分支名称       # 将本地的分支进行重命名
git push origin 新分支名称               # 将新的分支推送到远程        
git push --delete origin 旧分支名称      # 删除远程的旧的分支
```

### 3.2.2 分支的合并

```bash
# 切换回主分支
git checkout master

# 使用git merge 进行合并
git merge issue102

# 根据需要看是否删除分支

# git branch --no-merged
# 查看所有未合并工作的分支
```

视频教程：[git强大的分支管理能力](https://www.bilibili.com/video/BV1k3411s7GV?spm\_id\_from=333.337.search-card.all.click)

有时候分支的合并不会一番顺利，当我们在两个分支中**对同一个文件的同一个部分**进行了不同的修改，Git就没有办法顺利的合并他们，会在合并的时候产生**合并冲突**。

eg：比如我们在`issue102`分支和`master`分支下对`issue102.md`文件进行了修改，当我们将`issue102`分支融合到主分支上时就会发生冲突。

当出现了矛盾时，我们需要进行手动解决或者放弃合并。手动合并，选择我们要保留的代码，然后再把>>>>>, ======, <<<<<<这些提示行给去掉。最后重新进行`add，commit，push`的操作即可。

### 3.2.3 分支推送到远程

```bash
# 查看远程库的详细信息
git remote -v
origin  git@github.com:ProjectOwner/ProjectName.git (fetch)
origin  git@github.com:ProjectOwner/ProjectName.git (push)

# 推送本地的master分支到远程
git push origin master

# 推送本地的issue102分支到远程
git push origin issue102
```

## 3.3 分支开发工作流

当我们已经了解了分支的操作后，我们应该考虑使用一种怎样的方式使我们最大化的使用分支操作的优点。在接下来的这部分中，我们将会介绍一些常见的分支开发工作流程。而正是由于分支管理的便捷，才衍生出这些典型的工作模式，我们以后可以根据项目实际情况进行使用。

[Git Flow开发流](https://www.bilibili.com/video/BV1dT4y1r7ug?spm\_id\_from=333.880.my\_history.page.click)

![](../Git/images/git\_flow.jpg)

### 3.3.1 长期分支

在整个项目开发周期的不同阶段，我们可以同时拥有多个分支；

然后我们可以定期地把某些主题分支合并入其他分支中。

许多使用 Git 的开发者都喜欢使用这种方式来工作，

比如只在 master 分支上保留完全稳定的代码——有可能仅仅是已经发布或即将发布的代码。

他们还有一些名为 develop 或者 next 的平行分支，被用来做后续开发或者测试稳定性——这些分支不必保持绝对稳定，但是一旦达到稳定状态，它们就可以被合并入 master 分支了。

这样，在确保这些已完成的主题分支（短期分支，比如之前的 issue102 分支）能够通过所有测试，并且不会引入更多 bug 之后，就可以合并入主干分支中，等待下一次的发布。

### 3.3.2 短期分支

短期分支也可以叫做主题分支，它的作用是用来实现某一种特性或者相关工作（修复bug，开发产品新特性）。

比如当我们的产品出现了bug时，我们应该新建一个分支并起名为bug分支，并在该分支上进行bug的修复，等我们的代码确定不会引起其他bug时，我们就可以合并到主分支上进行修复。

当我们看见issue时，我们也可以使用同样的方式来解决issue的问题。

常见的短期分支还有上面提到的develop，topic分支。

### 3.3.3 基本原则

1. master分支应该是最稳定的，也就是仅用来发布新版本，平时不能直接在上面进行操作，应该保存在远程。
2. 短期分支是我们干活的分支，短期分支可以不用上传到远程，当我们完成了bug的修复，新功能的开发时才需要合并到主分支上。
3. 多使用分支来进行开发工作 eg：master，dev，self，test 四个分支。
