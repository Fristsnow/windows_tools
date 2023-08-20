---
title: 04-解决 Git 不区分大小写导致的文件冲突问题
---

<ArticleTopAd></ArticleTopAd>


## 解决 Git 不区分大小写导致的文件冲突问题

- 文章发布时间：2022-02-17

- 作者：@千古壹号

有些同学在 Git 仓库对文件/文件夹进行命名时，刚开始是小写，后来为了保持团队一致，又改成了大写，然而 Git 不会发现大小写的变化，此时就出了问题：导致仓库里出现了 大小写 同时存在的两个文件。但在 Windows、Mac 的电脑磁盘里，肉眼却能只看到一个文件，实在奇葩。

这个问题的根本原因是，Windows、Mac 的**文件系统**不区分大小写。

Linux的文件系统是区分大小写的。Git 默认是不区分大小写的，也可以通过改配置项，改为区分大小写。

### 问题复现路径

（1）新建一个 test 文件(大小写不敏感的状态下)，并提交。

（2）本地修改 test 变为 Test，文件内容无变更，无法提交。

（3）执行 `git config core.ignorecase false`，设 置Git的规则为 区分大小写（大小写敏感），然后 git push 提交，查看结果，此时远程仓库会存在 大小写 同时存在的文件，但本地仓库却只看到其中一个文件。

（4）甚至可能出现这种异常情况：本地暂存区的文件，怎么删也删不掉。再之后，从 test 尝试改为 Test 时，提示命名冲突。

### 错误的解决办法

```bash
git mv test Test
```

执行上面的命令时，会报错：`fatal: renaming 'Test' failed: Invalid argument`

### 正确的解决办法

```bash
# 将本地的 test、Test 目录都删掉，并生成一个新的目录 Temp
git mv Test Temp

# 将 Temp 目录改成 Test 目录。此时，项目中只会存在 Test 目录，不会存在 test 目录。目标达成。
git mv Temp Test
```

执行完上面的两个命令之后，项目中只会存在 Test 目录，不会存在 test 目录。目标达成。

### 关于 是否区分大小写 的补充说明

我们知道：针对文件/文件夹，Windows 系统和 Mac 系统是不区分大小写的；Linux 系统是区分大小写的；Git 默认是不区分大小写的，也可以通过改配置项，改为区分大小写。

不分区大小写，也有它的好处，比如：文件夹/文件的路径，很多时候就代表了网站地址、页面url的路径。而**网站地址也是不区分大小写的**，这是很关键的原因之一。

总的来说，根本原因是文件系统、url在底层设计上不区分大小写。磁盘路径、页面地址，本质上都是 url 。

### 关于 Git是否区分大小写 的补充

前面讲到，Git 默认是不区分大小写的，可以通过命令`git config --get core.ignorecase`查到，默认值是 true。

我们也可以修改 Git 的这一配置项，改为区分大小写，配置命令为`git config core.ignorecase false`。

但我建议你**保留 Git 的默认配置项**，不要随意自行修改，避免产生其他的麻烦。


### 参考链接

- [Mac 中 git 大小写问题的解决方案](https://shanyue.tech/bug/mac-git-ignorecase.html)

- [git 大小写问题 踩坑笔记](https://blog.csdn.net/u013707249/article/details/79135639)

## 赞赏作者

创作不易，你的赞赏和认可，是我更新的最大动力：

![](https://img.smyhvae.com/20220401_1800.jpg)