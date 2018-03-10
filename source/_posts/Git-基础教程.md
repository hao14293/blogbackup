---
title: Git 基础教程
tags: Git
categories: Git and Github
abbrlink: 46251
date: 2018-03-09 11:39:18
---
![0.jpeg](https://i.loli.net/2018/03/08/5aa0b11b22c6c.jpeg)
Git 和 Github是同学们经常使用的工具，这里简单写个基础操作教程。<br>
#### 安装Git
<b>Ubuntu</b>
<pre><code>sudo apt-get install git-core</code></pre>

<b>Mac OS X</b><br>
如果你正在使用Mac做开发，有两种安装Git的方法。

一是安装homebrew，然后通过homebrew安装Git，具体方法请参考homebrew的文档：http://brew.sh/。
<!--more-->
第二种方法更简单，也是推荐的方法，就是直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。
![1.jpeg](https://i.loli.net/2018/03/08/5aa0b316f0bc0.jpeg)
Xcode是Apple官方IDE，功能非常强大，是开发Mac和iOS App的必选装备，而且是免费的！<br>

<b>Windows</b><br>
在Windows上使用Git，可以从Git官网直接下载安装程序，（网速慢的同学请移步国内镜像），然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！
![2.jpeg](https://i.loli.net/2018/03/08/5aa0b3e5d2c57.jpeg)
安装完成后，还需要最后一步设置，在命令行输入：
<pre><code>$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"</code></pre>
因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

注意<code>git config</code>命令的<code>--global</code>参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。<br>
<b>  创建版本库</b><br>
什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：
<pre><code>mkdir learngit
cd learngit
pwd
/home/ubuntu/learngit</code></pre>
pwd命令用于显示当前目录。<br>
如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。<br>

第二步，通过<code>git init</code>命令把这个目录变成Git可以管理的仓库：
<pre><code>git init
初始化空的 Git 仓库于 /home/ubuntu/learngit/.git/
</code></pre>
瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。<br><br>
如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。<br>
也不一定必须在空目录下创建Git仓库，选择一个已经有东西的目录也是可以的。<br><br>
<b>把文件添加到版本库</b><br>
首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。<br><br>

不幸的是，Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的，前面我们举的例子只是为了演示，如果要真正使用版本控制系统，就要以纯文本方式编写文件。<br>

因为文本是有编码的，比如中文有常用的GBK编码，日文有Shift_JIS编码，如果没有历史遗留问题，强烈建议使用标准的UTF-8编码，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。<br><br>

使用Windows的童鞋要特别注意：<br><br>

千万不要使用Windows自带的记事本编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。建议你下载Notepad++代替记事本，不但功能强大，而且免费！记得把Notepad++的默认编码设置为UTF-8 without BOM即可.<br>
<br>
现在我们编写一个<code>readme.txt</code>文件，内容如下：
<pre><code>Git is a version control system.
Git is free software.</code></pre>
一定要放到<code>learngit</code>目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。

和把大象放到冰箱需要3步相比，把一个文件放到Git仓库只需要两步。

第一步，用命令<code>git add</code>告诉Git，把文件添加到仓库：
<pre><code>git add readme.txt</code></pre>
执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令<code>git commit</code>告诉Git，把文件提交到仓库：
<pre><code>$ git commit -m "wrote a readme file"
[master （根提交） a6ee6ed] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
 </code></pre>
简单解释一下<code>git commit</code>命令，<code>-m</code>后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。<br><br>

嫌麻烦不想输入<code>-m "xxx"</code>行不行？确实有办法可以这么干，但是强烈不建议你这么干，因为输入说明对自己对别人阅读都很重要。实在不想输入说明的童鞋请自行Google，我不告诉你这个参数。<br><br>

<code>git commit</code>命令执行成功后会告诉你，1个文件被改动（我们新添加的readme.txt文件），插入了两行内容（readme.txt有两行内容）。<br><br>

为什么Git添加文件需要<code>add</code>，<code>commit</code>一共两步呢？因为<code>commit</code>可以一次提交很多文件，所以你可以多次add不同的文件，比如：
<pre><code>$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."</code></pre>
<br>
<b> 时光机穿梭</b><br><br>

我们已经成功地添加并提交了一个readme.txt文件，现在，是时候继续工作了，于是，我们继续修改readme.txt文件，改成如下内容：

<pre><code>Git is a distributed version control system.
Git is free software.</code></pre>
现在，运行git status命令看看结果：

<pre><code>$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     readme.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）</code></pre>
<code>git status</code>命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。<br><br>

虽然Git告诉我们readme.txt被修改了，但如果能看看具体修改了什么内容，自然是很好的。比如你休假两周从国外回来，第一天上班时，已经记不清上次怎么修改的readme.txt，所以，需要用<code>git diff</code>这个命令看看：

<pre><code>$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
Git is free software.</code></pre>
<code>git diff</code>顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个“distributed”单词。

知道了对readme.txt作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是<code>git add</code>：

<pre><code>$ git add readme.txt</code></pre>
同样没有任何输出。在执行第二步<code>git commit</code>之前，我们再运行<code>git status</code>看看当前仓库的状态：

<pre><code>$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	修改：     readme.txt
 </code></pre>
<code>git status</code>告诉我们，将要被提交的修改包括readme.txt，下一步，就可以放心地提交了：

<pre><code>$ git commit -m "add distributed"
[master ea34578] add distributed
1 file changed, 1 insertion(+), 1 deletion(-)</code></pre>
提交后，我们再用<code>git status</code>命令看看仓库的当前状态：

<pre><code>$ git status
位于分支 master
无文件要提交，干净的工作区
</code></pre>
Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working directory clean）的。<br><br>
<b> 版本回退</b><br><br>

<code>git log</code>命令可以查看历史记录
<pre><code> $ git log
commit 4e77f2f8aada37a56fda295813a8b40847933dae
Author: hao14293 <hao14293@gmail.com>
Date:   Thu Mar 8 13:07:05 2018 +0800

    add

commit a6ee6ed74739651569179c5aadc961a2b548a2a9
Author: hao14293 <hao14293@gmail.com>
Date:   Thu Mar 8 12:54:56 2018 +0800

    wrote a readme file
  </code></pre>
    如果嫌输出信息太多，看得眼花缭乱的，可以试试加上<code>--pretty=oneline</code>参数：
    <pre><code>$ git log --pretty=oneline
4e77f2f8aada37a56fda295813a8b40847933dae add
a6ee6ed74739651569179c5aadc961a2b548a2a9 wrote a readme file
</code></pre>
需要友情提示的是，你看到的一大串类似<code>3628164...882e1e0</code>的是commit id（版本号），和SVN不一样，Git的commit id不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的commit id和我的肯定不一样，以你自己的为准。为什么commit id需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。<br><br>
现在我们准备退回上一个版本<br><br>
首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是<code>HEAD^</code>，上上一个版本就是<code>HEAD^^</code>，当然往上100个版本写100个<code>^</code>比较容易数不过来，所以写成<code>HEAD~100</code><br><br>
现在我们把当前版本回退到上一版本,就可以使用git reset命令：
<pre><code>$ git reset --hard HEAD^
HEAD 现在位于 a6ee6ed wrote a readme file</code></pre>
<code>--hard</code>参数有啥意义？这个后面再讲，现在你先放心使用.<br><br>
打开readme.txt，已经回到上一个版本了。<br><br>
<b>工作区、暂存区和版本库</b><br><br>
我们先来理解下Git 工作区、暂存区和版本库概念<br><br>

* 工作区：就是你在电脑里能看到的目录。<br>
* 暂存区：英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。<br>
* 版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。<br>
下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系：
![1.jpg](https://i.loli.net/2018/03/08/5aa0d97ba6186.jpg)
分支和HEAD的概念我们以后再讲。<br><br>

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：<br><br>

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；<br><br>

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。<br><br>

因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。<br><br>

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。<br><br>

俗话说，实践出真知。现在，我们再练习一遍，先对readme.txt做个修改，比如加上一行内容：
<pre><code>Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.</code></pre>
然后，在工作区新增一个LICENSE文本文件（内容随便写）。

先用git status查看一下状态：
<pre><code>$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     readme.txt

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	LICENSE

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
</code></pre>

Git非常清楚地告诉我们，readme.txt被修改了，而LICENSE还从来没有被添加过，所以它的状态是Untracked。<br><br>

现在，使用两次命令git add，把readme.txt和LICENSE都添加后，用git status再查看一下：
<pre><code>$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	新文件：   LICENSE
	修改：     readme.txt
  </code></pre>
现在，暂存区的状态就变成这样了：
![1.jpeg](https://i.loli.net/2018/03/08/5aa0db74a0056.jpeg)
所以，git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。
<pre><code>$ git commit -m "understand how stage works"
[master f9c5b54] understand how stage works
 2 files changed, 4 insertions(+), 2 deletions(-)
 create mode 100644 LICENSE
 </code></pre>
 一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：
 <pre><code>$ git status
位于分支 master
无文件要提交，干净的工作区
</code></pre>
现在版本库变成了这样，暂存区就没有任何内容了：
![2.jpeg](https://i.loli.net/2018/03/08/5aa0dbfc271a4.jpeg)

<b> Git 基本操作</b><br><br>
* <b>git init</b>:  用 git init 在目录中创建新的 Git 仓库。 你可以在任何时候、任何目录中这么做，完全是本地化的。在目录中执行 git init，就可以创建一个 Git 仓库了。<br>
*  <b>git clone</b>: 使用 git clone 拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改。 执行命令：<code> git clone [url]</code> [url] 为你想要复制的项目，就可以了。<br>
*  <b>git add</b>:  git add 命令可将该文件添加到缓存<br>
*  <b>git status</b>: git status 以查看在你上次提交之后是否有修改。<br>
*  <b>git diff</b>: 执行 git diff 来查看执行 git status 的结果的详细信息。<br>
git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。git diff 有两个主要的应用场景。<br>
* * 尚未缓存的改动：git diff<br>
* * 查看已缓存的改动： git diff --cached<br>
* * 查看已缓存的与未缓存的所有改动：git diff HEAD<br>
* * 显示摘要而非整个 diff：git diff --stat<br>
* <b>git commit</b>: 使用 git add 命令将想要快照的内容写入缓存区， 而执行 git commit 将缓存区内容添加到仓库中。<br>
* <b>git reset HEAD</b>: git reset HEAD 命令用于取消已缓存的内容。<br>
* <b>git rm</b>: 如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 Changes not staged for commit 的提示。<br>

* * 要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除，然后提交。可以用以下命令完成此项工作<br>
<code>git rm <file></code>
* * 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f
<code>git rm -f <file></code><br><br>
* * 如果把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 --cached 选项即可<br>
<code>git rm --cached <file></code><br>
如我们删除 hello.php文件：

<pre><code>$ git rm hello.php 
rm 'hello.php'
$ ls
README</code></pre>
* * 不从工作区中删除文件：

<pre><code>$ git rm --cached README 
rm 'README'
$ ls
README</code></pre>
* * 可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件：
<code>git rm –r * </code><br>
进入某个目录中，执行此语句，会删除该目录下的所有文件和子目录。<br>
* <b>git mv</b>:  <code>git mv</code> 命令用于移动或重命名一个文件、目录、软连接。
我们先把刚移除的 README 添加回来：<br><br>

<code>$ git add README</code> 
然后对其重名:

<pre><code>$ git mv README  README.md
$ ls
README.md</code></pre>
<b> 远程仓库</b><br><br>
Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。<br>这个世界上有个叫GitHub的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。<br><br>

在继续阅读后续内容前，请自行注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：<br><br>

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：<br><br>

<code>$ ssh-keygen -t rsa -C "youremail@example.com"</code><br><br>

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。<br><br>

如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。<br><br>

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：<br><br>

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：<br><br>

点“Add Key”，你就应该看到已经添加的Key：<br><br>

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。<br><br>

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。<br><br>

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。<br><br>

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。<br><br>

确保你拥有一个GitHub账号后，我们就即将开始远程仓库的学习。<br><br>

<b> 添加远程库</b><br><br>
现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。<br><br>

首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：<br><br>

在Repository name填入learngit，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：<br><br>

目前，在GitHub上的这个learngit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。<br><br>

现在，我们根据GitHub的提示，在本地的learngit仓库下运行命令：<br><br>

<pre><code>$ git remote add origin git@github.com:hao14293/learngit.git</code></pre><br><br>
请千万注意，把上面的hao14293替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。<br><br>

添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。<br><br>

下一步，就可以把本地库的所有内容推送到远程库上：<br><br>

<pre><code>$ git push -u origin master
Counting objects: 19, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (19/19), done.
Writing objects: 100% (19/19), 13.73 KiB, done.
Total 23 (delta 6), reused 0 (delta 0)
To git@github.com:michaelliao/learngit.git
 * [new branch]      master -> master
 Branch master set up to track remote branch master from origin.</code></pre>
把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。<br><br>

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。<br><br>

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：<br><br>


从现在起，只要本地作了提交，就可以通过命令：

<code>$ git push origin master</code>
把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

<b>SSH警告</b><br><br>
当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：

<pre><code>The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?</code></pre>
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。<br><br>

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：<br><br>

<pre><code>Warning: Permanently added 'github.com' (RSA) to the list of known hosts.</code></pre>
这个警告只会出现一次，后面的操作就不会有任何警告了。<br><br>

如果你实在担心有人冒充GitHub服务器，输入yes前可以对照GitHub的RSA Key的指纹信息是否与SSH连接给出的一致。<br><br>

<b> 从远程库克隆</b><br><br>
上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。<br><br>

现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。<br><br>

首先，登陆GitHub，创建一个新的仓库，名字叫gitskills：<br><br>

我们勾选<code>Initialize this repository with a README</code>，这样GitHub会自动为我们创建一个README.md文件。创建完毕后，可以看到README.md文件：<br><br>


现在，远程库已经准备好了，下一步是用命令git clone克隆一个本地库：<br><br>

<pre><code>$ git clone git@github.com:hao14293/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.

$ cd gitskills
$ ls
README.md</code></pre>
注意把Git库的地址换成你自己的，然后进入gitskills目录看看，已经有README.md文件了。<br><br>

 如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。<br><br>

你也许还注意到，GitHub给出的地址不止一个，还可以用<code>https://github.com/hao14293/gitskills.git</code>这样的地址。实际上，Git支持多种协议，默认的<code>git://</code>使用ssh，但也可以使用https等其他协议。<br><br>

使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。<br><br>
<b>分支管理</b><br><br>
几乎每一种版本控制系统都以某种形式支持分支。使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作。<br><br>

有人把 Git 的分支模型称为"必杀技特性"，而正是因为它，将 Git 从版本控制系统家族里区分出来。<br><br>

创建分支命令：<br><br>

<code>git branch (branchname)</code><br><br>
切换分支命令:<br><br>

<code>git checkout (branchname)</code><br><br>

当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。<br><br>

合并分支命令:<br><br>

<code>git merge</code> <br><br>
你可以多次合并到统一分支， 也可以选择在合并之后直接删除被并入的分支。<br><br>

<b> 列出分支</b><br><br>
列出分支基本命令：<br><br>

<code>git branch</code><br><br>
没有参数时，git branch 会列出你在本地的分支。<br><br>

<pre><code>$ git branch
* master</code></pre><br><br>
此例的意思就是，我们有一个叫做"master"的分支，并且该分支是当前分支。<br><br>

当你执行 git init 的时候，缺省情况下 Git 就会为你创建"master"分支。<br><br>

如果我们要手动创建一个分支。执行 git branch (branchname) 即可。

<pre><code>$ git branch testing
$ git branch
* master
testing</code></pre>
现在我们可以看到，有了一个新分支 testing。<br><br>

当你以此方式在上次提交更新之后创建了新分支，如果后来又有更新提交， 然后又切换到了"testing"分支，Git 将还原你的工作目录到你创建分支时候的样子<br><br>

接下来我们将演示如何切换分支，我们用 git checkout (branch) 切换到我们要修改的分支。<br><br>

<pre><code>$ ls
README
$ echo 'runoob.com' > test.txt
$ git add .
$ git commit -m 'add test.txt'
[master 048598f] add test.txt
 2 files changed, 1 insertion(+), 3 deletions(-)
 delete mode 100644 hello.php
 create mode 100644 test.txt
$ ls
README        test.txt
$ git checkout testing
Switched to branch 'testing'
$ ls
README        hello.php</code></pre><br><br>
当我们切换到"testing"分支的时候，我们添加的新文件test.txt被移除了, 原来被删除的文件hello.php文件又出现了。切换回"master"分支的时候，它们有重新出现了。

<pre><code>$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt</code></pre><br><br>
我们也可以使用 git checkout -b (branchname) 命令来创建新分支并立即切换到该分支下，从而在该分支中操作。<br><br>

<pre><code>$ git checkout -b newtest
Switched to a new branch 'newtest'
$ git rm test2.txt 
rm 'test2.txt'
$ ls
README        test.txt
$ git commit -am 'removed test2.txt'
[newtest 556f0a0] removed test2.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test2.txt
$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt    test2.txt</code></pre><br><br>
如你所见，我们创建了一个分支，在该分支的上下文中移除了一些文件，然后切换回我们的主分支，那些文件又回来了。<br><br>

使用分支将工作切分开来，从而让我们能够在不同上下文中做事，并来回切换。<br><br>

<b> 删除分支</b><br><br>
删除分支命令：<br><br>

<code>git branch -d (branchname)</code><br><br>

例如我们要删除"testing"分支：<br><br>

<pre>$ git branch
* master
  testing
$ git branch -d testing
Deleted branch testing (was 85fc7e7).
$ git branch
* master</pre><br><br>
<b>分支合并</b><br><br>
一旦某分支有了独立内容，你终究会希望将它合并回到你的主分支。 你可以使用以下命令将任何分支合并到当前分支中去：<br><br>

<code>git merge</code>
<pre><code>$ git branch
* master
  newtest
$ ls
README        test.txt    test2.txt
$ git merge newtest
Updating 2e082b7..556f0a0
Fast-forward
 test2.txt | 1 -
 1 file changed, 1 deletion(-)
 delete mode 100644 test2.txt
$ ls
README        test.txt</code></pre><br><br>
以上实例中我们将 newtest 分支合并到主分支去，test2.txt 文件被删除。<br><br>

<b> 合并冲突</b><br><br>
合并并不仅仅是简单的文件添加、移除的操作，Git 也会合并修改。<br><br>

<pre>$ git branch
* master
$ cat test.txt
runoob.com</pre><br><br>
首先，我们创建一个叫做"change_site"的分支，切换过去，我们将内容改为 www.runoob.com 。<br><br>

<pre>$ git checkout -b change_site
Switched to a new branch 'change_site'
$ vim test.txt 
$ head -1 test.txt 
www.runoob.com
$ git commit -am 'changed the site'
[change_site d7e7346] changed the site
1 file changed, 1 insertion(+), 1 deletion(-)</pre><br><br>
 
将修改的内容提交到 "change_site" 分支中。 现在，假如切换回 "master" 分支我们可以看内容恢复到我们修改前的，我们再次修改test.txt文件。<br><br>

<pre>$ git checkout master
Switched to branch 'master'
$ head -1 test.txt 
runoob.com
$ vim test.txt 
$ cat test.txt
runoob.com
新增加一行
$ git diff
diff --git a/test.txt b/test.txt
index 704cce7..f84c2a4 100644
--- a/test.txt
+++ b/test.txt
@@ -1 +1,2 @@
 runoob.com
+新增加一行
$ git commit -am '新增加一行'
[master 14b4dca] 新增加一行
1 file changed, 1 insertion(+)</pre>
 
现在这些改变已经记录到我的 "master" 分支了。接下来我们将 "change_site" 分支合并过来。<br><br>


我们将前一个分支合并到 "master" 分支，一个合并冲突就出现了，接下来我们需要手动去修改它。<br><br>

<pre>$ vim test.txt 
$ cat test.txt 
www.runoob.com
新增加一行
$ git diff
diff --cc test.txt
index f84c2a4,bccb7c2..0000000
--- a/test.txt
+++ b/test.txt
@@@ -1,2 -1,1 +1,2 @@@
- runoob.com
+ www.runoob.com
+新增加一行</pre><br><br>
在 Git 中，我们可以用 git add 要告诉 Git 文件冲突已经解决<br><br>

<pre>$ git status -s
UU test.txt
$ git add test.txt 
$ git status -s
M  test.txt
$ git commit
[master 88afe0e] Merge branch 'change_site'</pre>
现在我们成功解决了合并中的冲突，并提交了结果。<br><br>

<b> 标签管理</b><br><br>
如果你达到一个重要的阶段，并希望永远记住那个特别的提交快照，你可以使用 git tag 给它打上标签。<br><br>

比如说，我们想为我们的 runoob 项目发布一个"1.0"版本。 我们可以用 git tag -a v1.0 命令给最新一次提交打上（HEAD）"v1.0"的标签。<br><br>

-a 选项意为"创建一个带注解的标签"。 不用 -a 选项也可以执行的，但它不会记录这标签是啥时候打的，谁打的，也不会让你添加个标签的注解。 我推荐一直创建带注解的标签。<br><br>

<code>$ git tag -a v1.0</code><br><br>

当你执行 git tag -a 命令时，Git 会打开你的编辑器，让你写一句标签注解，就像你给提交写注解一样。<br><br>

现在，注意当我们执行 git log --decorate 时，我们可以看到我们的标签了：<br><br>

<pre>$ git log --oneline --decorate --graph
*   88afe0e (HEAD, tag: v1.0, master) Merge branch 'change_site'
|\  
| * d7e7346 (change_site) changed the site
* | 14b4dca 新增加一行
|/  
* 556f0a0 removed test2.txt
* 2e082b7 add test2.txt
* 048598f add test.txt
* 85fc7e7 test comment from runoob.com</pre><br><br>
如果我们忘了给某个提交打标签，又将它发布了，我们可以给它追加标签。<br><br>

例如，假设我们发布了提交 85fc7e7(上面实例最后一行)，但是那时候忘了给它打标签。 我们现在也可以：<br><br>

<pre>$ git tag -a v0.9 85fc7e7
$ git log --oneline --decorate --graph
*   88afe0e (HEAD, tag: v1.0, master) Merge branch 'change_site'
|\  
| * d7e7346 (change_site) changed the site
* | 14b4dca 新增加一行
|/  
* 556f0a0 removed test2.txt
* 2e082b7 add test2.txt
* 048598f add test.txt
* 85fc7e7 (tag: v0.9) test comment from runoob.com</pre><br><br>
如果我们要查看所有标签可以使用以下命令：<br><br>

<pre>$ git tag
v0.9
v1.0</pre>
指定标签信息命令：<br><br>

<pre>git tag -a <tagname> -m "runoob.com标签"</pre>
PGP签名标签命令：

<pre>git tag -s <tagname> -m "runoob.com标签"</pre>

<b>Github</b><br><br>

<pre>…or create a new repository on the command line
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/hao14293/test.git
git push -u origin master</pre><br><br>
<pre>…or push an existing repository from the command line
git remote add origin https://github.com/hao14293/test.git
git push -u origin master</pre><br><br>
<b>git将本地仓库推送到github</b><br><br>
1. 首先在本地创建ssh key;<br><br>
<pre>$ ssh-keygen -t rsa -C "your_email@youremail.com"  </pre><br><br>
2. 回到github，进入Account Settings，左边选择SSH Keys，Add SSH Key,title随便填，粘贴key。为了验证是否成功，在Git bash下输入：<br><br>

<pre>$ ssh -T git@github.com </pre> 

如果是第一次的会提示是否continue，输入yes就会看到：<code>You’ve successfully authenticated, but GitHub does not provide shell access 。</code>这就表示已成功连上github。<br><br>

3. 接下来我们要做的就是把本地仓库传到github上去，在此之前还需要设置username和email，因为github每次commit都会记录他们。<br><br>


<pre>git config --global user.name "your name"  
git config --global user.email "your_email@youremail.com"  </pre>
进入要上传的仓库，添加远程地址：<br><br>

<pre>$ git remote add origin git@github.com:yourName/yourRepo.git  
(可以去git上复制仓库的地址)</pre>
4.提交、上传 <br><br>

接下来在本地仓库里添加一些文件，比如README，<br><br>

<pre> git add README  
 git commit -m "first commit" </pre>
上传到github：<br><br>

<pre>$ git push origin master</pre>  
<code>git push</code>命令会将本地仓库推送到远程服务器。

<code>git pull</code>命令则相反。