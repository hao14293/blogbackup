---
title: Python Click 学习笔记
categories: Python
abbrlink: 36314
date: 2018-08-01 23:03:24
tags:
---
> [(原文)Python Click 学习笔记](https://isudox.com/2016/09/03/learning-python-package-click/)

<!--more-->

[Click](https://pypi.org/project/click/) 是 Flask 的团队 pallets 开发的优秀开源项目，它为命令行工具的开发封装了大量方法，使开发者只需要专注于功能实现。恰好我最近在开发的一个小工具需要在命令行环境下操作，就写个学习笔记。

国际惯例，先来一段 “Hello World” 程序（假定已经安装了 Click 包）。
```sh
# hello.py
import click

@click.command()
@click.option('--count', default=1, help='Number of greetings.')
@click.option('--name', prompt='Your name',
              help='The person to greet.')
def hello(count, name):
    """Simple program that greets NAME for a total of COUNT times."""
    for x in range(count):
        click.echo('Hello %s!' % name)

if __name__ == '__main__':
    hello()
    ```
执行 <code>python hello.py --count=3</code>，不难猜到控制台的输出结果。除此之外，Click 还悄悄地做了其他的工作，比如帮助选项：

```sh
$ python hello.py --help
Usage: hello.py [OPTIONS]

  Simple program that greets NAME for a total of COUNT times.

Options:
  --count INTEGER  Number of greetings.
  --name TEXT      The person to greet.
  --help           Show this message and exit.
  ```
#### 函数秒变 CLI
从上面的 “Hello World” 演示中可以看出，Click 是通过装饰器来把一个函数方法装饰成命令行接口的，这个装饰器方法就是 <code>@click.command()</code>。

```sh
import click

@click.command()
def hello():
    click.echo('Hello World!')
    ```
<code>@click.command()</code> 装饰器把 <code>hello()</code> 方法变成了 <code>Command</code> 对象，当它被调用时，就会执行该实例内的行为。而<code> --help</code> 参数就是<code> Command </code>对象内置的参数。

不同的<code> Command </code> 实例可以关联到<code> group</code> 中。<code>group </code>下绑定的命令就成为了它的子命令，参考下面的代码：

```sh
@click.group()
def cli():
    pass

@click.command()
def initdb():
    click.echo('Initialized the database')

@click.command()
def dropdb():
    click.echo('Dropped the database')

cli.add_command(initdb)
cli.add_command(dropdb)
```
<code>@click.group </code>装饰器把方法装饰为可以拥有多个子命令的 <code>Group </code>对象。由<code> Group.add_command() </code>方法把<code> Command </code>对象关联到<code> Group </code>对象。
也可以直接用<code> @Group.command </code>装饰方法，会自动把方法关联到该 <code>Group </code>对象下。

```sh
@click.group()
def cli():
    pass

@cli.command()
def initdb():
    click.echo('Initialized the database')

@cli.command()
def dropdb():
    click.echo('Dropped the database')
    ```
命令行的参数是不可或缺的，<code>Click </code>支持对 <code>command </code>方法添加自定义的参数，由<code> option()</code> 和<code> argument() </code>装饰器实现。

```sh
@click.command()
@click.option('--count', default=1, help='number of greetings')
@click.argument('name')
def hello(count, name):
    for x in range(count):
        click.echo('Hello %s!' % name)
        ```
#### 打包跨平台可执行程序
通过 Click 编写了简单的命令行方法后，还需要把<code> .py</code> 文件转换成可以在控制台里运行的命令行程序。最简单的办法就是在文件末尾加上如下代码：

```sh
if __name__ == '__main__':
    command()
    ```
Click 支持使用<code> setuptools </code>来更好的实现命令行程序打包，把源码文件打包成系统中的可执行程序，并且不限平台。一般我们会在源码根目录下创建<code> setup.py</code> 脚本，先看一段简单的打包代码：

```sh
from setuptools import setup

setup(
    name='hello',
    version='0.1',
    py_modules=['hello'],
    install_requires=[
        'Click',
    ],
    entry_points='''
        [console_scripts]
        hello=hello:cli
    ''',
)
```
留意<code> entry_points </code>字段，在<code> console_scripts </code>下，每一行都是一个控制台脚本，等号左边的的是脚本的名称，右边的是 Click 命令的导入路径。

#### 详解命令行参数
上面提到了自定义命令行参数的两个装饰器：<code>@click.option() </code>和<code> @click.argument()</code>，两者有些许区别，使用场景也有所不同。

总体而言，<code>argument()</code> 装饰器比<code> option() </code>功能简单些，后者支持下面的特性：

* 自动提示缺失的输入；
* option 参数可以从环境变量中获取，argument 参数则不行；
* option 参数在 help 输出中有完整的文档，argument 则没有；


而 argument 参数可以接受可变个数的参数值，而 option 参数只能接收固定个数的参数值（默认是 1 个）。

Click 可以设置不同的参数类型，简单类型如<code> click.STRING</code>，<code>click.INT</code>，<code>click.FLOAT</code>，<code>click.BOOL</code>。

命令行的参数名由 “-short_name” 和 “–long_name” 声明，如果参数名既没有以 “-“ 开头，也没有以 “–” 开头，那么这边变量名会成为被装饰方法的内部变量，而非方法参数。

#### Option 参数
option 最基础的用法就是简单值变量，option 接收一个变量值，下面是一段示例代码：

```sh
@click.command()
@click.option('--n', default=1)
def dots(n):
    click.echo('.' * n)
    ```
如果在命令行后面跟随参数<code> --n=2 </code>就会输出两个点，如果传参数，默认输出一个点。上面的代码中，参数类型没有显示给出，但解释器会认为是 <code>INT</code> 型，因为默认值 1 是 int 值。
有些时候需要传入多个值，可以理解为一个 list，option 只支持固定长度的参数值，即设置后必须传入，个数由 <code>nargs</code> 确定。

```sh
@click.command()
@click.option('--pos', nargs=2, type=float)
def findme(pos):
    click.echo('%s / %s' % pos)
    ```
<code>findme --pos 2.0 3.0 </code> 输出结果就是 2.0 / 3.0

既然可以传入 list，那么 tuple 呢？Click 也是支持的：

```sh
@click.command()
@click.option('--item', type=(unicode, int))
def putitem(item):
    click.echo('name=%s id=%d' % item)
    ```
这样就传入了一个 tuple 变量，<code>putitem --item peter 1338</code> 得到的输出就是 <code>name=peter id=1338</code>
上面没有设置 nargs，因为 nargs 会自动取 tuple 的长度值。因此上面的代码实际上等同于：

```sh
@click.command()
@click.option('--item', nargs=2, type=click.Tuple([unicode, int]))
def putitem(item):
    click.echo('name=%s id=%d' % item)
    ```
option 还支持同一个参数多次使用，类似 <code>git commit -m aa -m bb</code> 中 <code>-m</code> 参数就传入了 2 次。option 通过 <code>multiple</code> 标识位来支持这一特性：

```sh
@click.command()
@click.option('--message', '-m', multiple=True)
def commit(message):
    click.echo('\n'.join(message))
    ```
有时候，命令行参数是固定的几个值，这时就可以用到 <code>Click.choice</code> 类型来限定传参的潜在值：

```
# choice
@click.command()
@click.option('--hash-type', type=click.Choice(['md5', 'sha1']))
def digest(hash_type):
    click.echo(hash_type)
    ```
当上面的命令行程序参数 <code>--hash-type</code> 不是 md5 或 sha1，就会输出错误提示，并且在 <code>--help</code> 提示中也会对 choice 选项有显示。

如果希望命令行程序能在我们错误输入或漏掉输入的情况下，友好的提示用户，就需要用到 Click 的 <code>prompt</code> 功能，看代码：

```sh
# prompt
@click.command()
@click.option('--name', prompt=True)
def hello(name):
    click.echo('Hello %s!' % name)
    ```
如果在执行 hello 时没有提供 –name 参数，控制台会提示用户输入该参数。也可以自定义控制台的提示输出，把 <code>prompt</code> 改为自定义内容即可。

对于类似账户密码等参数的输入，就要进行隐藏显示。option 的 <code>hide_input</code> 和 <code>confirmation_promt</code> 标识就是用来控制密码参数的输入：

```sh
# password
@click.command()
@click.option('--password', prompt=True, hide_input=True,
              confirmation_prompt=True)
def encrypt(password):
    click.echo('Encrypting password to %s' % password.encode('rot13'))
    ```
Click 把上面的操作进一步封装成装饰器 <code>click.password_option()</code>，因此上面的代码也可以简化成：

```sh password
@click.command()
@click.password_option()
def encrypt(password):
    click.echo('Encrypting password to %s' % password.encode('rot13'))
    ```
有的参数会改变命令行程序的执行，比如 <code>node</code> 是进入 Node 控制台，而 <code>node --verion</code> 是输出 node 的版本号。Click 提供 eager 标识对参数名进行标记，拦截既定的命令行执行流程，而是调用一个回调方法，执行后直接退出。下面模拟 <code>click.version_option()</code> 的功能，实现 <code>--version</code> 参数名输出版本号：

```sh
# eager
def print_version(ctx, param, value):
    if not value or ctx.resilient_parsing:
        return
    click.echo('Version 1.0')
    ctx.exit()

@click.command()
@click.option('--version', is_flag=True, callback=print_version,
              expose_value=False, is_eager=True)
def hello():
    click.echo('Hello World!')
    ```
对于类似删除数据库表这样的危险操作，Click 支持弹出确认提示，<code>--yes</code> 标识位置为 True 时会让用户再次确认：

```sh
# yes parameters
def abort_if_false(ctx, param, value):
    if not value:
        ctx.abort()

@click.command()
@click.option('--yes', is_flag=True, callback=abort_if_false,
              expose_value=False,
              prompt='Are you sure you want to drop the db?')
def dropdb():
    click.echo('Dropped all tables!')
    ```
测试运行下：

```sh
$ dropdb
Are you sure you want to drop the db? [y/N]: n
Aborted!
$ dropdb --yes
Dropped all tables!
```
同样的，Click 对次进行了封装，<code>click.confirmation_option()</code> 装饰器实现了上述功能：

```sh
@click.command()
@click.confirmation_option(prompt='Are you sure you want to drop the db?')
def dropdb():
    click.echo('Dropped all tables!')
    ```
前面只讲了默认的参数前缀 <code>--</code> 和 <code>-</code>，Click 允许开发者自定义参数前缀（虽然严重不推荐）。

```sh
# other prefix
@click.command()
@click.option('+w/-w')
def chmod(w):
    click.echo('writable=%s' % w)

if __name__ == '__main__':
    chmod()
    ```
如果想要用 <code>/</code> 作为前缀，而且要像上面一样采用布尔标识，会产生冲突，因为布尔标识也是用 <code>/</code>，这种情况下可以用 ; 代替布尔标识的 <code>/</code>：

```sh
@click.command()
@click.option('/debug;/no-debug')
def log(debug):
    click.echo('debug=%s' % debug)

if __name__ == '__main__':
    log()
    ```
既然支持 Choice，不难联想到 Range，先看代码：

```sh
# range
@click.command()
@click.option('--count', type=click.IntRange(0, 20, clamp=True))
@click.option('--digit', type=click.IntRange(0, 10))
def repeat(count, digit):
    click.echo(str(digit) * count)

if __name__ == '__main__':
    repeat()
    ```
#### Argument 参数
Argument 的作用类似 Option，但没有 Option 那么全面的功能。

和 Option 一样，Argument 最基础的应用就是传递一个简单变量值：

```sh
@click.command()
@click.argument('filename')
def touch(filename):
    click.echo(filename)
    ```
命令行后跟的参数值被赋值给参数名 <code>filename</code>。

另一个用的比较广泛的是可变参数，也是由 nargs 来确定参数个数，变量值会以 tuple 的形式传入函数：

```sh
@click.command()
@click.argument('src', nargs=-1)
@click.argument('dst', nargs=1)
def copy(src, dst):
    for fn in src:
        click.echo('move %s to folder %s' % (fn, dst))
        ```
运行程序：

```sh
$ copy foo.txt bar.txt my_folder
move foo.txt to folder my_folder
move bar.txt to folder my_folder
```
Click 支持通过文件名参数对文件进行操作，<code>click.File()</code> 装饰器就是处理这种操作的，尤其是在类 Unix 系统下，它支持以 <code>-</code> 符号作为标准输入/输出。

```sh
# File
@click.command()
@click.argument('input', type=click.File('rb'))
@click.argument('output', type=click.File('wb'))
def inout(input, output):
    while True:
        chunk = input.read(1024)
        if not chunk:
            break
        output.write(chunk)
        ```
运行程序，先将文本写进文件，再读取

```sh
$ inout - hello.txt
hello
^D
$ inout hello.txt -
hello
```
如果参数值只是想做为文件名而已呢，很简单，将 <code>type</code> 指定为 <code>click.Path()</code>：

```sh
@click.command()
@click.argument('f', type=click.Path(exists=True))
def touch(f):
    click.echo(click.format_filename(f))
    ```
```sh
$ touch hello.txt
hello.txt

$ touch missing.txt
Usage: touch [OPTIONS] F

Error: Invalid value for "f": Path "missing.txt" does not exist.
```
