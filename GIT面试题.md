# GIT面试题

## 1. 列举工作中常用的几个git命令?

新增文件的命令：git add file或者git add .
提交文件的命令：git commit –m或者git commit –a
查看工作区状况：git status –s
拉取合并远程分支的操作：git fetch/git merge或者git pull
查看提交记录命令：git reflog

## 2. 提交时发生冲突,你能解释冲突是如何产生的吗?你是如何解决的?

​	开发过程中，我们都有自己的特性分支，所以冲突发生的并不多，但也碰到过。诸如公共类的公共方法，我和别人同时修改同一个文件，他提交后我再提交就会报冲突的错误.
​	
​	发生冲突，在IDE里面一般都是对比本地文件和远程分支的文件，然后把远程分支上文件的内容手工修改到本地文件，然后再提交冲突的文件使其保证与远程分支的文件一致，这样才会消除冲突，然后再提交自己修改的部分。特别要注意下，修改本地冲突文件使其与远程仓库的文件保持一致后，需要提交后才能消除冲突，否则无法继续提交。必要时可与同事交流，消除冲突.
​	
​	发生冲突，也可以使用命令:
​		通过git stash命令，把工作区的修改提交到栈区，目的是保存工作区的修改；
​		通过git pull命令，拉取远程分支上的代码并合并到本地分支，目的是消除冲突；
​		通过git stash pop命令，把保存在栈区的修改部分合并到最新的工作空间中；

## 3. 如果本次提交误操作,如何撤销?

如果想撤销提交到索引区的文件，可以通过git reset HEAD file；如果想撤销提交到本地仓库的文件，可以通过git reset –soft HEAD^n恢复当前分支的版本库至上一次提交的状态，索引区和工作空间不变更；可以通过git reset –mixed HEAD^n恢复当前分支的版本库和索引区至上一次提交的状态，工作区不变更；可以通过git reset –hard HEAD^n恢复当前分支的版本库、索引区和工作空间至上一次提交的状态。

## 4. 如果我想修改提交的历史信息，应该用什么命令？



## 5. 你使用过git stash命令吗？你一般什么情况下会使用它？

命令git stash是把工作区修改的内容存储在栈区。

​	以下几种情况会使用到它：

​		解决冲突文件时，会先执行git stash，然后解决冲突；
​		遇到紧急开发任务但目前任务不能提交时，会先执行git stash，然后进行紧急任务的开发，然后通过git stash pop取出栈区的内容继续开发；
切换分支时，当前工作空间内容不能提交时，会先执行git stash再进行分支切换；

## 6. 如何查看分支提交的历史记录？查看某个文件的历史记录呢？

查看分支的提交历史记录：

​		命令git log –number：表示查看当前分支前number个详细的提交历史记录；

​		命令git log –number –pretty=oneline：在上个命令的基础上进行简化，只显示sha-1码和提交信息；

​		命令git reflog –number: 表示查看所有分支前number个简化的提交历史记录；

​		命令git reflog –number –pretty=oneline：显示简化的信息历史信息；
如果要查看某文件的提交历史记录，直接在上面命令后面加上文件名即可。

​		注意：如果没有number则显示全部提交次数。

## 7.  能不能说一下git fetch和git pull命令之间的区别？

​		简单来说：git fetch branch是把名为branch的远程分支拉取到本地；

​		而git pull branch是在fetch的基础上，把branch分支与当前分支进行merge；因此pull = fetch + merge。

​		**或者这么回答：**

		git pull 命令从中央存储库中提取特定分支的新更改或提交，并更新本地存储库中的目标分支。

​		git fetch 也用于相同的目的，但它的工作方式略有不同。当你执行 git fetch 时，它会从所需的分支中提取所有新提交，并将其存储在本地存储库中的新分支中。如果要在目标分支中反映这些更改，必须在 git fetch 之后执行git merge。只有在对目标分支和获取的分支进行合并后才会更新目标分支。为了方便起见，请记住以下等式：

<center><h5>git pull = git fetch + git merge</h5></center>

## 8. 使用过git merge和git rebase吗？它们之间有什么区别？

​		简单的说，git merge和git rebase都是合并分支的命令。

​		git merge branch会把branch分支的差异内容pull到本地，然后与本地分支的内容一并形成一个committer对象提交到主分支上，合并后的分支与主分支一致；

​		git rebase branch会把branch分支优先合并到主分支，然后把本地分支的commit放到主分支后面，合并后的分支就好像从合并后主分支又拉了一个分支一样，本地分支本身不会保留提交历史。

## 9. 能说一下git系统中HEAD、工作树和索引之间的区别吗？

​		**HEAD文件**包含当前分支的引用（指针）；

​		**工作树**是把当前分支检出到工作空间后形成的目录树，一般的开发工作都会基于工作树进行；

​		**索引index文件**是对工作树进行代码修改后，通过add命令更新索引文件；GIT系统通过索引index文件生成tree对象

## 10. git跟其他版本控制器有啥区别？

​		GIT是分布式版本控制系统，其他类似于SVN是集中式版本控制系统。

​		分布式区别于集中式在于：每个节点的地位都是平等，拥有自己的版本库，在没有网络的情况下，对工作空间内代码的修改可以提交到本地仓库，此时的本地仓库相当于集中式的远程仓库，可以基于本地仓库进行提交、撤销等常规操作，从而方便日常开发。

## 11. 我们在本地工程常会修改一些配置文件，这些文件不需要被提交，而我们又不想每次执行git status时都让这些文件显示出来，我们该如何操作？

首先利用命令touch .gitignore新建文件：

```
touch .gitignore
```

然后往文件中添加需要忽略哪些文件夹下的什么类型的文件：

```
/target/class
.settings
.imp
*.ini
```

注意：忽略/target/class文件夹下所有后缀名为.settings，.imp的文件，忽略所有后缀名为.ini的文件。

## 12. 如何把本地仓库的内容推向一个空的远程仓库？

首先确保本地仓库与远程之间是连同的。如果提交失败，则需要进行下面的命令进行连通：

```
git remote add origin XXXX
```

注意：XXXX是你的远程仓库地址。
如果是第一次推送，则进行下面命令：

```
git push -u origin master
```

注意：-u 是指定origin为默认主分支
之后的提交，只需要下面的命令：

```
git push origin master
```

## 13. 什么是SubGit？

​		SubGit 是将 SVN 到 Git迁移的工具。它创建了一个可写的本地或远程 Subversion 存储库的 Git 镜像，并且只要你愿意，可以随意使用 Subversion 和 Git。

​		这样做有很多优点，比如你可以从 Subversion 快速一次性导入到 Git 或者在 Atlassian Bitbucket Server 中使用SubGit。我们可以用 SubGit 创建现有 Subversion 存储库的双向 Git-SVN 镜像。你可以在方便时 push 到 Git 或提交 Subversion。同步由 SubGit 完成。

## 14. 如果分支是否已合并为master，你可以通过什么手段知道？

答案很直接。

要知道某个分支是否已合并为master，你可以使用以下命令：

`git branch –merged` 它列出了已合并到当前分支的分支。

`git branch –no-merged` 它列出了尚未合并的分支。

## 15. 描述一下你所使用的分支策略？

​		这个问题被要求用Git来测试你的分支经验，告诉他们你在以前的工作中如何使用分支以及它的用途是什么，你可以参考以下提到的要点：

功能分支（Feature branching）
		要素分支模型将特定要素的所有更改保留在分支内。当通过自动化测试对功能进行全面测试和验证时，该分支将合并到主服务器中。

任务分支（Task branching）
		在此模型中，每个任务都在其自己的分支上实现，任务键包含在分支名称中。很容易看出哪个代码实现了哪个任务，只需在分支名称中查找任务键。

发布分支（Release branching）
		一旦开发分支获得了足够的发布功能，你就可以克隆该分支来形成发布分支。创建该分支将会启动下一个发布周期，所以在此之后不能再添加任何新功能，只有错误修复，文档生成和其他面向发布的任务应该包含在此分支中。一旦准备好发布，该版本将合并到主服务器并标记版本号。此外，它还应该再将自发布以来已经取得的进展合并回开发分支。

​		最后告诉他们分支策略因团队而异，所以我知道基本的分支操作，如删除、合并、检查分支等。

## 16. 如何在Git中创建存储库？

这可能是最常见的问题，答案很简单。

要创建存储库，先为项目创建一个目录（如果该目录不存在），然后运行命令 **git init**。通过运行此命令，将在项目的目录中创建 .git 目录。

## 17. 提交对象包含什么？

Commit 对象包含以下组件，你应该提到以下这三点：

- 一组文件，表示给定时间点的项目状态
- 引用父提交对象
- SHAI 名称，一个40个字符的字符串，提交对象的唯一标识。

## 18. git config 的功能是什么？

首先说明为什么我们需要 git config。

git 使用你的用户名将提交与身份相关联。 git config 命令可用来更改你的 git 配置，包括你的用户名。

下面用一个例子来解释。

假设你要提供用户名和电子邮件 ID 用来将提交与身份相关联，以便你可以知道是谁进行了特定提交。为此，我将使用：

git config –global user.name "Your Name": 此命令将添加用户名。

git config –global user.email "Your E-mail Address": 此命令将添加电子邮件ID。

## 19. 什么是 git stash?

首先应该解释 git stash 的必要性。

​		通常情况下，当你一直在处理项目的某一部分时，如果你想要在某个时候切换分支去处理其他事情，事情会处于混乱的状态。问题是，你不想把完成了一半的工作的提交，以便你以后就可以回到当前的工作。解决这个问题的答案是 git stash。

再解释什么是git stash。

​		stash 会将你的工作目录，即修改后的跟踪文件和暂存的更改保存在一堆未完成的更改中，你可以随时重新应用这些更改。

## 20. 在Git中，你如何还原已经 push 并公开的提交？

这个问题可以有两个答案，你回答时也要保包含这两个答案，因为根据具体情况可以使用以下选项：

- 删除或修复新提交中的错误文件，并将其推送到远程存储库。这是修复错误的最自然方式。对文件进行必要的修改后，将其提交到我将使用的远程存储库

  ```
  git commit -m "commit message"
  ```

- 创建一个新的提交，撤消在错误提交中所做的所有更改。可以使用命令：

  ```
  git revert <name of bad commit>
  ```

## 21. Git 是用什么语言编写的？

你需要说明使用它的原因，而不仅仅是说出语言的名称。我建议你这样回答：

Git使用 C 语言编写。 GIT 很快，C 语言通过减少运行时的开销来做到这一点。

## 22. 什么是 Git 中的“裸存储库”？

你应该说明 “工作目录” 和 “裸存储库” 之间的区别。

​		Git 中的 “裸” 存储库只包含版本控制信息而没有工作文件（没有工作树），并且它不包含特殊的 .git 子目录。相反，它直接在主目录本身包含 .git 子目录中的所有内容，其中工作目录包括：

​		一个 .git 子目录，其中包含你的仓库所有相关的 Git 修订历史记录。
​		工作树，或签出的项目文件的副本。

## 23. 在 Git 中提交的命令是什么？

答案非常简单。
用于写入提交的命令是 git commit -a。

现在解释一下 -a 标志， 通过在命令行上加 -a 指示 git 提交已修改的所有被跟踪文件的新内容。还要提一下，如果你是第一次需要提交新文件，可以在在 git commit -a 之前先 git add <file>。

## 24. Git和SVN有什么区别？

![image-20191121110744580](/Users/yangling/Library/Application Support/typora-user-images/image-20191121110744580.png)

## 25. 什么是Git？

我建议你先通过了解 git 的架构再来回答这个问题，如下图所示，试着解释一下这个图：

​		Git 是分布式版本控制系统（DVCS）。它可以跟踪文件的更改，并允许你恢复到任何特定版本的更改。

​		与 SVN 等其他版本控制系统（VCS）相比，其分布式架构具有许多优势，一个主要优点是它不依赖于中央服务器来存储项目文件的所有版本。
每个开发人员都可以“克隆”我在图中用“Local repository”标注的存储库的副本，并且在他的硬盘驱动器上具有项目的完整历史记录，因此当服务器中断时，你需要的所有恢复数据都在你队友的本地 Git 存储库中。

​		还有一个中央云存储库，开发人员可以向其提交更改，并与其他团队成员进行共享，如图所示，所有协作者都在提交更改“远程存储库”。

![clipboard.png](https://segmentfault.com/img/bVbtc0b?w=502&h=252)

## 26. 之前项目中是使用的GitFlow工作流程吗？它有什么好处？

**GitFlow可以用来管理分支。GitFlow工作流中常用的分支有下面几类：**

- **master分支**：最为稳定功能比较完整的随时可发布的代码，即代码开发完成，经过测试，没有明显的bug，才能合并到 master 中。请注意永远不要在 master 分支上直接开发和提交代码，以确保 master 上的代码一直可用；

- **develop分支**；用作平时开发的主分支，并一直存在，永远是功能最新最全的分支，包含所有要发布 到下一个 release 的代码，主要用于合并其他分支，比如 feature 分支； 如果修改代码，新建 feature 分支修改完再合并到 develop 分支。所有的 feature、release 分支都是从 develop 分支上拉的。

- **feature分支**；这个分支主要是用来开发新的功能，一旦开发完成，通过测试没问题（这个测试，测试新功能没问题），我们合并回develop 分支进入下一个 release

- **release分支**；用于发布准备的专门分支。当开发进行到一定程度，或者说快到了既定的发布日，可以发布时，建立一个 release 分支并指定版本号(可以在 finish 的时候添加)。开发人员可以对 release 分支上的代码进行集中测试和修改bug。（这个测试，测试新功能与已有的功能是否有冲突，兼容性）全部完成经过测试没有问题后，将 release 分支上的代码合并到 master 分支和 develop 分支

- **hotfix分支**；用于修复线上代码的bug。**从 master 分支上拉。**完成 hotfix 后，打上 tag 我们合并回 master 和 develop 分支。

  **GitFlow主要工作流程**

- 1.初始化项目为gitflow , 默认创建master分支 , 然后从master拉取第一个develop分支

- 2.从develop拉取feature分支进行编码开发(多个开发人员拉取多个feature同时进行并行开发 , 互不影响)

- 3.feature分支完成后 , 合并到develop(不推送 , feature功能完成还未提测 , 推送后会影响其他功能分支的开发)；合并feature到develop , 可以选择删除当前feature , 也可以不删除。但当前feature就不可更改了，必须从release分支继续编码修改

4.从develop拉取release分支进行提测 , 提测过程中在release分支上修改BUG

5.release分支上线后 , 合并release分支到develop/master并推送；合并之后，可选删除当前release分支，若不删除，则当前release不可修改。线上有问题也必须从master拉取hotfix分支进行修改；

6.上线之后若发现线上BUG , 从master拉取hotfix进行BUG修改；

7.hotfix通过测试上线后，合并hotfix分支到develop/master并推送；合并之后，可选删除当前hotfix ，若不删除，则当前hotfix不可修改，若补丁未修复，需要从master拉取新的hotfix继续修改；

8.当进行一个feature时 , 若develop分支有变动 , 如其他开发人员完成功能并上线 , 则需要将完成的功能合并到自己分支上，即合并develop到当前feature分支；

9.当进行一个release分支时 , 若develop分支有变动 , 如其他开发人员完成功能并上线 , 则需要将完成的功能合并到自己分支上，即合并develop到当前release分支 (!!! 因为当前release分支通过测试后会发布到线上 , 如果不合并最新的develop分支 , 就会发生丢代码的情况)；

**GitFlow的好处**
为不同的分支分配一个明确的角色，并定义分支之间如何交互以及什么时间交互；可以帮助大型项目理清分支之间的关系，简化分支的复杂度。

## 27. 请描述什么是工作区、暂存区和本地仓库？

![img](file:////var/folders/yw/fjx4d7k92cq5zff6y5qmhy8h0000gn/T/com.kingsoft.wpsoffice.mac/wps-yangling/ksohtml/wpsA2NBx1.jpg) 对于任何一个文件，在 Git 内都只有三种区域：工作区，暂存区和本地仓库。

![img](file:////var/folders/yw/fjx4d7k92cq5zff6y5qmhy8h0000gn/T/com.kingsoft.wpsoffice.mac/wps-yangling/ksohtml/wpsUovxDr.jpg) 工作区：表示新增或修改了某个文件，但还没有提交保存；

![img](file:////var/folders/yw/fjx4d7k92cq5zff6y5qmhy8h0000gn/T/com.kingsoft.wpsoffice.mac/wps-yangling/ksohtml/wpsHFpZ79.jpg) 暂存区：表示把已新增或修改的文件，放在下次提交时要保存的清单中; ![img](file:////var/folders/yw/fjx4d7k92cq5zff6y5qmhy8h0000gn/T/com.kingsoft.wpsoffice.mac/wps-yangling/ksohtml/wpsKbNmWa.jpg) 本地仓库：文件已经被安全地保存在本地仓库中了。

![img](file:////var/folders/yw/fjx4d7k92cq5zff6y5qmhy8h0000gn/T/com.kingsoft.wpsoffice.mac/wps-yangling/ksohtml/wpsn9oK4x.png) 

## 28. 请写出查看分支、创建分支、删除分支、切换分支、合并分支的命令以及写出解决冲突的思路？

![image-20191121124053759](/Users/yangling/Library/Application Support/typora-user-images/image-20191121124053759.png)

​	![image-20191121124112570](/Users/yangling/Library/Application Support/typora-user-images/image-20191121124112570.png)

## 29. 请写出将工作区文件推送到远程仓库的思路？（两种情况都写出来）

## 30.请写出团队内部协作开发的流程？

![img](file:////var/folders/yw/fjx4d7k92cq5zff6y5qmhy8h0000gn/T/com.kingsoft.wpsoffice.mac/wps-yangling/ksohtml/wpsVF8FPK.png)

这块应该在描述出分支的创建与切换。

## 31. 请写出远程跨团队协作开发的流程？

![img](file:////var/folders/yw/fjx4d7k92cq5zff6y5qmhy8h0000gn/T/com.kingsoft.wpsoffice.mac/wps-yangling/ksohtml/wpsfAWlqR.png)

## 32. 请写出配置ssh的思路？

## 33. 请写出你们公司团队内部协作开发的流程？

## 34. 请描述什么是GitLab,或者说出你对GitLab的理解？

## 35. 请写出你所参与的多人协同开发时候，项目都有哪些分支，分支名是什么，每个分支代表什么，以及分支是由谁合并？

# 参考文献：

https://blog.csdn.net/weixin_30663391/article/details/97852039

https://blog.csdn.net/weixin_30663391/article/details/97852039

https://blog.csdn.net/qq_31001889/article/details/80316503

https://blog.csdn.net/Hanani_Jia/article/details/77950594

