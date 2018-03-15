---
title: vim精简版教程
categories: Linux
abbrlink: 12935
date: 2018-03-13 08:40:08
tags:
---
![1.jpg](https://i.loli.net/2018/03/13/5aa71e0326d7e.jpg)
<!--more-->


### 编辑器的分类
* 文本编辑器，ASCII码
* 字处理器：word
### 全称
* vi：Visual interface
* vim: Visual interface improved
### 分类
* 全屏编辑器,
> vim
> Emacs
> notepad++...
* 模式编辑器
> grep
> sed
> awk
### vim模式：
1.编辑模式（命令模式）<br>
2.输入模式<br>
3.末行模式<br>
4.可视化模式（块）<br>
默认处于编辑模式<br>
### 模式转换
<code>编辑模式（命令模式） --->>输入模式</code><br>
<pre>i：在当前光标所在字符的的前面，转换为输入
    a：在当前光标所在字符的的后面，转换为输入
    o:在当前光标所在字符的行下方，新建一行，并转为输入模式。
    I:在当前光标所在行的行首，转为输入模式
    A:在当前光标所在行的行尾，转为输入模式
    O:在当前光标所在行的上方，新建一行，并转为输入模式。</pre>
<code>输入模式--->编辑模式（命令模式)</code><br>
<pre>ESC键</pre>
<code>编辑模式（命令模式）---->>末行模式</code><br>
<pre>:
10d
10,20d
set nu
!ls /etc</pre>
<code>末行模式---->> 编辑模式（命令模式）</code><br>
<pre>ESC  ESC键</pre>
#### 一.打开文件：vim filename
<pre>vim /path/to/somefile
vim +12 file ：打开文件，光标在12行
vim  +# file :打开文件，光标在N行
vim  +   file：打开文件，光标在最后一行。
vim  +/pattern file ：打开文件，光标在第一个匹配的行首</pre>
#### 二.关闭文件：
<pre>1.末行模式关闭文件
:q　　退出
:q!
:wq   保存退出
；w  保存 
：w!  强制保存
:wq   --> :x

2.编辑模式（命令模式）
ZZ:保存退出</pre>
#### 三.移动光标（编辑模式）
<pre>1.逐字符移动：
 h:向右
 j:向下
 k:向上
 l:向左
 数字h  
 5h：向右移动5个字符
 
2.逐个单词移动
 w:移动到下一个单词词首
 e：跳到当前单词或下一单词的词尾
 b：跳到当前单词或前一单词的词首
 #w：一次跳n个单词。
 4b:


3. 行内跳转
  0：跳到行首(绝对行首)
  ^:行首的第一个非空白字符
  $:绝对行尾
4.行间跳转
   #G：跳转到n行
   G：最后一行
   GG：第一行

  末行模式   ：#  移动到n行</pre>
#### 四、翻屏
<pre>编辑模式（命令模式）
f： 向后翻一屏、
CTRL+b:向上翻一屏

Ctrl+d： 向下翻半屏
Ctrl+u:向上翻半屏</pre>
#### 五、删除单个字符
<pre>x：删除光标所在处的单个字符
#x:删除光标所在处及向后n个字符</pre>
#### 六、删除命令：d
<pre>d命令跟跳转命令组合使用
dw：
3dw：
#de，#db
dd：删除当前光标所在行
#dd：删除当前光标所在行及下面共#行 


末行模式下
startadd，Endaddd
1,8d
.,5+d
.:表示当前行
$:表示最后一行
+#：向下#行
1,$-3d:

最后一次删除的内容，可以粘贴到别处</pre>
#### 七、粘贴命令p
<pre>p:如果删除或复制为整行内容，则粘贴至光标所在行的下方，如果复制或删除的内容为非整行，则粘贴至光标所在字符的后面；
P:如果删除或复制为整行内容，则粘贴至光标所在行的上方，如果复制或删除的内容为非整行，则粘贴至光标所在字符的前面；</pre>
#### 八、复制命令 y
<pre>yy：一行
#y：</pre>
#### 九、先删除内容，在转换为输入模式(修改)
<pre>c：同d命令
c$:
cc:
5C:</pre>
#### 十、替换
<pre>r:替换单个字符
R：进入替换模式</pre>
#### 十一、撤销编辑操作：
<pre>u:撤销前一次的操作：
  连续u，撤销此前n次操作
3u
#u：撤销最近#次操作</pre>
#### 十二、撤销上一次的撤销
<pre>Ctrl+r
恢复</pre>
#### 十三、重复前一次编辑操作
<pre>.</pre>
#### 十四、可视化模式
<pre>v：按字符选取
V：按矩形选取
Ctrl+v:</pre>
#### 十五、查找
<pre>/pattern
?pattern
n
N</pre>
#### 十六、查找并替换
<pre>在末行模式下

用法和sed一样

address1，address2s/pattern/string/gi

1，$

%:表示全文。</pre>
#### 十七、打开多个文件
<pre>vim file1 file2
;next 切换至下一个文件
：prev 切换至前一个文件
：last 切换至最后一个文件
：first 切换最前面的一个文件
退出
：qall 全部退出</pre>
#### 十八：分屏显示一个文件
<pre>ctrl+w ,s:水平分割窗口
ctrl+w,v:垂直分割窗口

在窗口间切换光标
Ctrl+w，ARRON

：qa 关闭所有窗口</pre>
#### 十九、分窗口多个文件
<pre>vim -o file1 file2  file3 ..水平分割窗口
vim -O  file1 file3 ....    垂直分割窗口</pre>
#### 二十、将当前文件部分内容另存为另一文件
<pre>末行模式下使用w命令
：w
：add1,addr2w /path/to/somewhere</pre>
#### 二十一、将另一个文件的内容填充在当前文件中
<pre>：r /path/to/somefile</pre>
#### 二十二、跟shell交互
<pre>：!command</pre>
#### 二十三、高级话题
<pre>1.显示或取消行号
：set number
set nu
：set nonu
2、显示忽略大小写或区分大小写
set ignorecase
set ic
:set noignorecase
:set noic
3.设定自动缩进
：set autoindent
:set noai

4.查找的文本高亮显示或取消
：set hlsearch
:set nohlsearch

5.语法高亮
:syntax on
:syntax off</pre>
#### 二十四、配置文件
<pre>/etc/vimrc
~/.vimrc(家目录下)</pre>
#### 二十五、练习vim的小游戏
<pre>vimtutor 
vim -r file</pre>
总结以上都是我大学的时候，学习的笔记，无意间看到了，发现很多东西都忘记了，现在准备复习一下，分享在这里。下面赠送一个安装vim插件的命令。<br>
神器：<code> wget -qO- https://raw.github.com/ma6174/vim/master/setup.sh | sh -x</code>