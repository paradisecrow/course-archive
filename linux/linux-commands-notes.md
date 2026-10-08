# Linux 常用命令学习笔记

> 约定：`$` 普通用户提示符，`#` root 提示符；`~` 表示当前用户主目录（如 `/home/zhangsan`）。

---

## 目录

1. [了解 Linux 及用户信息](#1了解-linux-及用户信息)
2. [cd —— 切换目录（change directory）](#2cd--切换目录change-directory)
3. [ls —— 查看文件与目录（list）](#3ls--查看文件与目录list)
4. [mkdir —— 新建目录（make directory）](#4mkdir--新建目录make-directory)
5. [rmdir —— 删除空目录（remove directory）](#5rmdir--删除空目录remove-directory)
6. [cp —— 复制文件或目录（copy）](#6cp--复制文件或目录copy)
7. [mv —— 移动文件与目录，或更名（move）](#7mv--移动文件与目录或更名move)
8. [cat —— 查看文件内容（concatenate）](#8cat--查看文件内容concatenate)
9. [tac —— 反向列示（cat backwards）](#9tac--反向列示cat-backwards)
10. [more —— 一页一页翻动查看](#10more--一页一页翻动查看)
11. [head —— 取出前面几行](#11head--取出前面几行)
12. [tail —— 取出后面几行](#12tail--取出后面几行)
13. [touch —— 修改文件时间或创建新文件](#13touch--修改文件时间或创建新文件)
14. [chown —— 修改文件所有者（change owner）](#14chown--修改文件所有者change-owner)
15. [find —— 文件查找](#15find--文件查找)
16. [tar —— 打包压缩（tape archive）](#16tar--打包压缩tape-archive)
17. [grep —— 查找字符串（global regular expression print）](#17grep--查找字符串global-regular-expression-print)
18. [echo —— 回显与查看变量](#18echo--回显与查看变量)
19. [chmod —— 文件访问权限（change mode）](#19chmod--文件访问权限change-mode)
20. [> >> —— 输出重定向（redirect）](#20----输出重定向redirect)
21. [| —— 管道操作（pipe）](#21--管道操作pipe)
22. [demo.sh —— 通过脚本运行一组命令](#22demosh--通过脚本运行一组命令)
23. [work.sh —— 脚本中的重定向、管道与后台运行](#23worksh--脚本中的重定向管道与后台运行)

**附录**：[命令速查表](#附录命令速查表) ｜ [易错点提醒](#易错点提醒)

---

## 1、了解 Linux 及用户信息

### 涉及命令

`ls`、`tree`、`pwd`、`whoami`、`cat`、`head`、`id`

### 语法

```bash
ls -l /              # 查看根目录下一级内容（目录结构）
tree -L 1 /          # 树形查看目录结构，需安装 tree
ls -lR /             # 递归列出全部内容
pwd                  # 查看当前文件路径（print working directory）
whoami               # 查看当前用户名
cat /etc/passwd      # 查看 passwd 文件中的用户信息
head -n 5 /etc/group # 只看 group 文件前 5 行
```

### 功能与示例

**查看 Linux 操作系统的目录结构**

```bash
ls -l /            # 或 tree -L 1 /  ；递归查看： ls -lR /
```

```
$ ls -l /
total 68
lrwxrwxrwx.   1 root root     7 8月   8 2022 bin -> usr/bin
dr-xr-xr-x.   5 root root  4096 8月   8 2022 boot
drwxr-xr-x.  20 root root  3360 3月  12 09:15 dev
drwxr-xr-x.  86 root root  8192 3月  12 09:10 etc
drwxr-xr-x.   3 root root    22 8月   8 2022 home
...
```

Linux 目录结构（FHS 标准）：

| 目录 | 作用 |
| --- | --- |
| `/` | 根目录，所有目录的起点 |
| `/bin`、`/sbin` | 可执行命令（`/sbin` 多为管理员命令） |
| `/boot` | 启动内核与引导程序 |
| `/dev` | **设备文件**（硬盘、终端等，即“外部设备文件”） |
| `/etc` | **配置文件**（`passwd`、`group`、`sshd_config` 等） |
| `/home` | **普通用户家目录** `/home/用户名` |
| `/root` | root 用户的家目录 |
| `/lib` | 共享库文件 |
| `/tmp` | **临时文件**目录（重启可能被清空） |
| `/usr` | 用户程序与资源，`/usr/local` 常放手工安装软件 |
| `/var` | 经常变化的文件，如日志 `/var/log` |

> 补充：Linux 由 **Linus Torvalds** 于 **1991 年**首次发布；其最大特点是**开源且免费、源代码公开可自由修改**，支持多用户多任务、可运行在多种硬件平台。

**查看当前文件路径**

```bash
pwd
```

```
$ pwd
/home/zhangsan
```

**查看当前用户名**

```bash
whoami             # 或 id -un
```

```
$ whoami
zhangsan
```

**查看 passwd 文件中的用户相关信息**

```bash
cat /etc/passwd    # 只看前几行： head -5 /etc/passwd
```

```
$ head -5 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/sbin/nologin
```

字段含义：`用户名 : 密码占位符(x) : UID : GID : 用户说明 : 主目录 : 登录Shell`。
其中 UID=0 的是超级用户 **root**；密码密文另存于 `/etc/shadow`，所以 `/etc/passwd` 里只是 `x`。

**查看 group 文件中的分组相关信息**

```bash
cat /etc/group     # 只看前几行： head -5 /etc/group
```

```
$ head -5 /etc/group
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
```

字段含义：`组名:密码占位符:GID:组内成员列表`。把多个用户放进同一个组，就能一次性给这一组用户分配相同权限，便于批量管理、增强安全性。

---

## 2、cd —— 切换目录（change directory）

### 语法

```bash
cd [目录路径]
```

| 写法 | 含义 |
| --- | --- |
| `cd /usr/local` | 切换到绝对路径目录 |
| `cd ..` | 去到目前的上层目录 |
| `cd .` | 当前目录（不变） |
| `cd -` | 回到上一次所在目录 |
| `cd` 或 `cd ~` | 回到自己的主文件夹 |
| `cd ~用户名` | 进入指定用户的主目录 |

### 功能与示例

**切换到目录 /usr/local**

```bash
cd /usr/local
```

```
$ cd /usr/local && pwd
/usr/local
```

**去到目前的上层目录**

```bash
cd ..
```

```
$ cd .. && pwd
/usr
```

**回到自己的主文件夹**

```bash
cd ~
```

```
$ cd ~ && pwd
/home/zhangsan

$ cd - && pwd          # 补充：回到上一次所在目录
/usr
```

```bash
cd /usr/local
cd ..
cd ~
```

---

## 3、ls —— 查看文件与目录（list）

### 语法

```bash
ls [选项] [目录或文件]
```

| 选项 | 作用 |
| --- | --- |
| `-l` | 长格式显示（权限、属主、大小、时间） |
| `-a` | 显示全部文件，**含隐藏文件**（以 `.` 开头） |
| `-A` | 显示隐藏文件但不含 `.` 与 `..` |
| `-h` | 与 `-l` 连用，文件大小人性化（KB/MB） |
| `-R` | 递归显示子目录内容 |
| `-d` | 只显示目录本身，不展开 |
| `-i` | 显示 inode 号 |
| `-t` | 按修改时间排序 |

### 功能与示例

**查看目录 /usr 下所有的文件**

```bash
ls /usr               # 基本写法
ls -l /usr            # 带详细信息
ls -al /usr           # 含隐藏文件
ls -R /usr            # 递归查看所有子目录文件
```

```
$ ls -lh /usr
total 40K
drwxr-xr-x.  2 root root 4.0K 8月   8 2022 bin
drwxr-xr-x.  2 root root 4.0K 8月   8 2022 etc
drwxr-xr-x.  2 root root 4.0K 8月   8 2022 games
drwxr-xr-x. 58 root root 4.0K 8月   8 2022 lib
drwxr-xr-x. 10 root root 4.0K 8月   8 2022 local
...
```

---

## 4、mkdir —— 新建目录（make directory）

### 语法

```bash
mkdir [选项] 目录名...
```

| 选项 | 作用 |
| --- | --- |
| `-p` | 递归创建多级目录；父目录已存在也不报错 |
| `-m 权限` | 创建时直接指定权限，如 `-m 777` |

### 功能与示例

**进入 /tmp 目录，创建一个名为 a 的目录**

```bash
cd /tmp
mkdir a
```

```
$ cd /tmp && mkdir a && ls -ld /tmp/a
drwxrwxr-x. 2 zhangsan zhangsan 6 3月 12 10:15 /tmp/a
```

**在 /tmp 中创建目录 a1/a2/a3/a4**

```bash
mkdir -p /tmp/a1/a2/a3/a4     # 多级目录必须加 -p
```

```
$ mkdir -p /tmp/a1/a2/a3/a4 && ls -R /tmp/a1
/tmp/a1:
a2

/tmp/a1/a2:
a3

/tmp/a1/a2/a3:
a4

/tmp/a1/a2/a3/a4:
```

---

## 5、rmdir —— 删除空目录（remove directory）

### 语法

```bash
rmdir [选项] 目录名...
```

| 选项 | 作用 |
| --- | --- |
| `-p` | 连同上层“空目录”一起删除（递归删空链） |
| 注意 | rmdir **只能删空目录**；目录非空会报 `Directory not empty`，此时用 `rm -r` |

### 功能与示例

**将上例创建的目录 a（/tmp 下面）删除**

```bash
rmdir /tmp/a
```

```
$ rmdir /tmp/a && ls -ld /tmp/a
ls: 无法访问'/tmp/a': 没有那个文件或目录
```

**删除 /tmp 下的目录 a1/a2/a3/a4**

```bash
rmdir -p /tmp/a1/a2/a3/a4
```

```
$ rmdir -p /tmp/a1/a2/a3/a4
$ ls /tmp | grep a1        # 无输出，说明 a1 整条链都被删除
```

**配套命令 rm —— 删除文件或目录（remove）**

```bash
rm -r /tmp/test          # 递归删除目录及内容（目录非空时用）
rm -rf /tmp/test         # 强制删除，慎用
rm -rf /tmp/*            # 删除 /tmp 下所有文件和子目录
```

> 注意区分：`rm -rf /tmp/*` 删的是“/tmp 下的内容”；`rm -rf /tmp` 会把 `/tmp` 目录本身也删掉，更危险。

---

## 6、cp —— 复制文件或目录（copy）

### 语法

```bash
cp [选项] 源文件 目标文件
cp [选项] 源文件... 目标目录
```

| 选项 | 作用 |
| --- | --- |
| `-r` / `-R` | 递归复制整个目录 |
| `-p` | 连同权限、属主、时间戳一起保留 |
| `-a` | 等价 `-dpr`，归档式复制（最常用） |
| `-i` | 覆盖前询问 |
| `-f` | 强制覆盖 |
| `-v` | 显示复制过程 |

### 功能与示例

**将主文件夹下的 .bashrc 复制到 /usr 下，命名为 bashrc1**

```bash
cp ~/.bashrc /usr/bashrc1
# 没有权限时先提权： sudo cp ~/.bashrc /usr/bashrc1
```

```
$ sudo cp ~/.bashrc /usr/bashrc1
$ ls -l /usr/bashrc1
-rw-r--r--. 1 root root 231 3月 12 10:18 /usr/bashrc1
```

**在 /tmp 下新建目录 test，再复制这个目录内容到 /usr**

```bash
mkdir /tmp/test
cp -r /tmp/test /usr          # 或： cp -a /tmp/test /usr
```

```
$ cp -r /tmp/test /usr
$ ls -ld /usr/test
drwxr-xr-x. 2 zhangsan zhangsan 6 3月 12 10:20 /usr/test
```

---

## 7、mv —— 移动文件与目录，或更名（move）

### 语法

```bash
mv [选项] 源文件 目标文件      # 目标不存在 ⇒ 重命名
mv [选项] 源文件... 目标目录   # 移动
```

常用选项：`-i` 覆盖前询问、`-f` 强制覆盖、`-v` 显示过程、`-b` 覆盖前备份。

### 功能与示例

**将上例文件 bashrc1 移动到目录 /usr/test**

```bash
mv /usr/bashrc1 /usr/test
```

```
$ mv /usr/bashrc1 /usr/test && ls /usr/test
bashrc1
```

**将上例 test 目录重命名为 test2**

```bash
mv /usr/test /usr/test2
```

```
$ mv /usr/test /usr/test2 && ls -ld /usr/test2
drwxr-xr-x. 2 zhangsan zhangsan 19 3月 12 10:22 /usr/test2
```

> `cp` 是“复制”，源文件仍在；`mv` 是“剪切/改名”，源文件消失。

---

## 8、cat —— 查看文件内容（concatenate）

### 语法

```bash
cat [选项] 文件名
```

| 选项 | 作用 |
| --- | --- |
| `-n` | 显示行号 |
| `-b` | 非空行显示行号 |
| `-A` | 显示不可见字符（制表符、行尾 `$`） |
| `>` / `>>` | 配合重定向合并文件、写文件 |

### 功能与示例

**查看主文件夹下的 .bashrc 文件内容**

```bash
cat ~/.bashrc
# 带行号： cat -n ~/.bashrc
```

```
$ cat -n ~/.bashrc | head -5
     1  # .bashrc
     2
     3  # User specific aliases and functions
     4
     5  alias rm='rm -i'
```

**合并两个文件**

```bash
cat a.txt b.txt > c.txt
```

---

## 9、tac —— 反向列示（cat backwards）

### 语法

```bash
tac [选项] 文件名      # 注意是 cat 倒过来写
```

### 功能与示例

**反向查看主文件夹下 .bashrc 文件内容**（从最后一行到第一行输出）

```bash
tac ~/.bashrc
```

用小文件演示效果更直观：

```
$ echo -e "1\n2\n3" > /tmp/n.txt
$ cat /tmp/n.txt
1
2
3
$ tac /tmp/n.txt
3
2
1
```

---

## 10、more —— 一页一页翻动查看

### 语法

```bash
more [选项] 文件名
```

| 按键 | 作用 |
| --- | --- |
| `空格` / `PageDown` | 向下翻一页 |
| `Enter` | 向下滚动一行 |
| `b` | 向上翻一页（`more` 支持有限） |
| `/字符串` | 向下搜索字符串 |
| `q` | 退出 |

常用选项：`-num` 每页显示多少行（如 `more -20 file`）、`+num` 从第几行开始显示。

### 功能与示例

**翻页查看主文件夹下 .bashrc 文件内容**

```bash
more ~/.bashrc
# 每页 20 行： more -20 ~/.bashrc
```

```
$ more /etc/passwd
root:x:0:0:root:/root:/bin/bash
...
--More--(15%)
```

**配套命令 less —— 可上下移动光标查看**

```bash
less ~/.bashrc        # 方向键上下滚动，q 退出
```

> 查看文件内容时可以用光标上下移动的是 `less`；`more` 只能向下翻页，`menu` 不是 Linux 命令。

---

## 11、head —— 取出前面几行

### 语法

```bash
head [选项] 文件名
```

| 选项 | 作用 |
| --- | --- |
| `-n 数字` 或 `-数字` | 显示前 N 行，默认 10 行 |
| `-c 字节数` | 显示前 N 个字节 |

### 功能与示例

**查看主文件夹下 .bashrc 文件内容前 20 行**

```bash
head -20 ~/.bashrc      # 等价写法： head -n 20 ~/.bashrc
```

```
$ head -3 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

---

## 12、tail —— 取出后面几行

### 语法

```bash
tail [选项] 文件名
```

| 选项 | 作用 |
| --- | --- |
| `-n 数字` 或 `-数字` | 显示最后 N 行，默认 10 行 |
| `-f` | 动态跟踪文件新增内容（看日志常用） |
| `-c 字节数` | 显示最后 N 个字节 |

### 功能与示例

**查看主文件夹下 .bashrc 文件内容最后 20 行**

```bash
tail -20 ~/.bashrc      # 等价写法： tail -n 20 ~/.bashrc
```

```
$ tail -3 /etc/passwd
tcpdump:x:72:72::/:/sbin/nologin
zhangsan:x:1000:1000:zhangsan:/home/zhangsan:/bin/bash
mysql:x:27:27:MySQL Server:/var/lib/mysql:/bin/false
```

**动态跟踪日志**（Ctrl+C 退出）

```bash
tail -f /var/log/messages
```

---

## 13、touch —— 修改文件时间或创建新文件

### 语法

```bash
touch [选项] 文件名
```

| 选项 | 作用 |
| --- | --- |
| 无选项 | 文件不存在则创建空文件；已存在则更新为当前时间 |
| `-a` | 只改访问时间 atime |
| `-m` | 只改修改时间 mtime |
| `-t [[CC]YY]MMDDhhmm[.ss]` | 指定时间 |
| `-d "字符串"` | 用可读字符串指定时间，如 `-d "5 days ago"` |
| `-r 参考文件` | 以参考文件的时间为准 |

### 功能与示例

**在 /tmp 下创建一个空文件 hello 并查看时间**

```bash
cd /tmp
touch hello
ls -l hello          # 或 stat hello，查看时间戳
```

```
$ touch hello && ls -l hello
-rw-r--r--. 1 zhangsan zhangsan 0 3月 12 10:30 hello
```

**修改 hello 文件，将日期调整为 5 天前**

```bash
touch -d "5 days ago" /tmp/hello
ls -l /tmp/hello
```

```
$ touch -d "5 days ago" hello && ls -l hello
-rw-r--r--. 1 zhangsan zhangsan 0 3月  7 10:30 hello

$ stat hello
  File: hello
  Size: 0          Blocks: 0     IO Block: 4096  regular empty file
Access: 2024-03-07 10:30:12 +0800
Modify: 2024-03-07 10:30:12 +0800
Change: 2024-03-12 10:31:05 +0800
```

**指定具体日期时间**

```bash
touch -t 202403070930 hello
```

---

## 14、chown —— 修改文件所有者（change owner）

### 语法

```bash
chown [选项] 用户[:组] 文件或目录
```

| 选项/写法 | 作用 |
| --- | --- |
| `chown 用户 文件` | 只改属主 |
| `chown 用户:组 文件` | 同时改属主与属组 |
| `chown :组 文件` | 只改属组（等价 `chgrp`） |
| `-R` | 递归处理目录下所有文件 |
| `-v` | 显示处理过程 |
| 权限 | **仅超级用户（root）可用**，普通用户需 `sudo` |

### 功能与示例

**将 hello 文件所有者改为 root 帐号，并查看属性**

```bash
sudo chown root /tmp/hello       # root 用户直接： chown root /tmp/hello
ls -l /tmp/hello                 # 查看属性（属主已变为 root）
```

```
$ sudo chown root /tmp/hello
$ ls -l /tmp/hello
-rw-r--r--. 1 root zhangsan 0 3月  7 10:30 /tmp/hello
                    ↑ 属主已变为 root
```

**同时改组、递归修改目录**

```bash
sudo chown root:root /tmp/hello
sudo chown -R zhangsan:zhangsan /home/zhangsan
```

**配套命令**

```bash
su -                  # 普通用户转换为超级用户（需 root 密码）
sudo chown root /tmp/hello    # 权限提升：以 root 身份执行单条命令
whoami / id -un               # 查看当前账户；超级用户账户名为 root，UID=0
```

> `su` 才是“将普通用户转换为超级用户”；`sudo` 只是单条命令提权；`super` 不是 Linux 命令。

---

## 15、find —— 文件查找

### 语法

```bash
find [搜索路径] [条件] [动作]
```

| 条件 | 含义 |
| --- | --- |
| `-name "文件名"` | 按名字查找（支持通配符 `*` `?`） |
| `-iname` | 名字忽略大小写 |
| `-type f/d/l` | 按类型：普通文件/目录/链接 |
| `-size +10M` | 按大小（+大于，-小于） |
| `-mtime -7` | 按修改时间（-7 表示 7 天内） |
| `-user 用户名` | 按属主 |
| `-perm 777` | 按权限 |
| `-exec 命令 {} \;` | 对结果执行命令 |

### 功能与示例

**找出主文件夹下文件名为 .bashrc 的文件**

```bash
find ~ -name ".bashrc"
# 若报权限错误可加： 2>/dev/null
# find ~ -name ".bashrc" 2>/dev/null
```

```
$ find ~ -name ".bashrc"
/home/zhangsan/.bashrc
```

**其他常用写法**

```bash
find /tmp -type d                      # 只找目录
find / -name "*.conf" 2>/dev/null      # 全盘找配置文件
find ~ -name "*.log" -size +1M         # 找大于1M的日志
find /tmp -name "*.tmp" -exec rm {} \; # 找到并删除
```

> 区分：`find` 按**文件名/属性**找文件；在**文件内容中查特定字符串**要用 `grep`。

---

## 16、tar —— 打包压缩（tape archive）

### 语法

```bash
tar [选项] 包文件名 [要打包的文件或目录...]
```

| 选项 | 作用 |
| --- | --- |
| `-c` | 创建打包文件（create） |
| `-x` | 解开打包文件（extract） |
| `-t` | 查看包内文件列表 |
| `-v` | 显示过程 |
| `-f 文件名` | 指定包文件名（必须紧跟文件名，放在最后） |
| `-z` | 用 gzip 压缩/解压（`.tar.gz`） |
| `-j` | 用 bzip2（`.tar.bz2`） |
| `-C 目录` | 解压到指定目录 |

> 记忆：**打包用 `czvf`，解包用 `xzvf`，查看用 `tzvf`。**

### 功能与示例

**在 / 目录下新建文件夹 test，然后在 / 目录下打包成 test.tar.gz**

```bash
sudo mkdir /test                      # root 直接： mkdir /test
cd /                                  # 先切到 / 目录
sudo tar -czvf /test.tar.gz test      # 打包 / 下的 test 目录
```

```
$ tar -czvf /test.tar.gz test
test/
test/file1.txt

$ tar -tzvf /test.tar.gz              # 只看包内列表，不解压
drwxr-xr-x root/root 0 2024-03-12 10:40 test/
-rw-r--r-- root/root 0 2024-03-12 10:40 test/file1.txt
```

**解压缩到 /tmp 目录**

```bash
sudo tar -xzvf /test.tar.gz -C /tmp   # -C 指定解压目标目录
ls -l /tmp/test                       # 验证
```

```
$ tar -xzvf /test.tar.gz -C /tmp
test/
test/file1.txt
```

---

## 17、grep —— 查找字符串（global regular expression print）

### 语法

```bash
grep [选项] "字符串" 文件名
```

| 选项 | 作用 |
| --- | --- |
| `-i` | 忽略大小写 |
| `-n` | 显示匹配行的行号 |
| `-v` | 反向选择（显示不匹配的行） |
| `-c` | 只统计匹配行数 |
| `-w` | 匹配整个单词 |
| `-r` | 递归查找目录 |
| `-E` | 支持扩展正则（等价 `egrep`） |
| `-A n` / `-B n` | 显示匹配行后/前 n 行 |

### 功能与示例

**从 ~/.bashrc 文件中查找字符串 'examples'**

```bash
grep "examples" ~/.bashrc
# 带行号忽略大小写： grep -in "examples" ~/.bashrc
```

```
$ grep -n "alias" ~/.bashrc
5:alias rm='rm -i'
6:alias ll='ls -l'
```

**其他常用写法**

```bash
grep -c "a" /etc/passwd               # 统计含 a 的行数
grep -v "^#" /etc/ssh/sshd_config     # 过滤掉注释行
ps -ef | grep bash                    # 配合管道过滤进程（见第 21、23 节）
```

> 区分：`which` 查命令路径，`locate` 按名字快速查文件，`find` 按属性查文件，只有 `grep` 查文件**内容**。

---

## 18、echo —— 回显与查看变量

### 语法

```bash
echo [选项] 字符串或变量
echo $变量名          # 输出变量的值
```

| 用法 | 说明 |
| --- | --- |
| `echo "hello"` | 输出字符串 |
| `echo $JAVA_HOME` | 输出 JAVA_HOME 环境变量的值 |
| `echo ${JAVA_HOME}` | 同上，加花括号更规范 |
| `echo $PATH` | 查看命令搜索路径 |
| `echo $?` | 上一个命令的退出状态（0 表示成功） |
| `echo $$` | 当前 Shell 的 PID |
| `echo $!` | 上一个后台进程的 PID |
| `echo -n` | 输出后不换行 |
| `echo -e "a\tb"` | 解析转义字符 |

### 功能与示例

**查看 JAVA_HOME 变量的值**

```bash
echo $JAVA_HOME
```

```
$ echo $JAVA_HOME
/usr/local/jdk1.8.0_181

$ echo "JAVA_HOME = ${JAVA_HOME}"
JAVA_HOME = /usr/local/jdk1.8.0_181
```

**若输出为空说明未配置**，可临时设置：

```bash
export JAVA_HOME=/usr/local/jdk1.8.0_181
# 永久生效：写入 /etc/profile 后 source /etc/profile
```

**相关命令**

```bash
env                        # 查看所有环境变量
set                        # 查看所有变量（含局部变量）
printenv JAVA_HOME
```

---

## 19、chmod —— 文件访问权限（change mode）

### 语法

```bash
chmod [选项] 权限 文件        # 数字法
chmod [选项] [ugoa][+-=][rwx] 文件   # 符号法
```

`ls -l` 第一列共 10 位，分 **4 段**：

```
类型  属主权限   属组权限   其他人权限
 -      rwx        r-x        r--
第1段   第2段      第3段       第4段
```

- 第 1 段：文件类型（`-` 普通文件、`d` 目录、`l` 链接）
- 第 2 段：**文件所有者权限**
- 第 3 段：**文件所有者所在组权限**
- 第 4 段：其他用户权限

权限字母与数字：

| 字母 | 英文 | 含义 | 数字 |
| --- | --- | --- | --- |
| `r` | read | 读 | 4 |
| `w` | write | 写 | 2 |
| `x` | execute | 执行 | 1 |
| `-` | — | 无权限 | 0 |

组合口诀：`rwx`=7、`rw-`=6、`r-x`=5、`r--`=4、`---`=0。
例如 `chmod 746`：7=`rwx`、4=`r--`、6=`rw-` ⇒ **`rwxrw-r--`**。

| 选项 | 作用 |
| --- | --- |
| `-R` | 递归修改目录内所有文件 |
| `-v` | 显示修改过程 |
| 对象 | `u`属主、`g`属组、`o`其他人、`a`所有人 |
| 操作 | `+`增加、`-`取消、`=`设为 |

### 功能与示例

**用户主目录下创建文件 a.txt，访问权限修改为 777**

```bash
cd ~
touch a.txt
chmod 777 a.txt
ls -l a.txt
```

```
$ chmod 777 a.txt && ls -l a.txt
-rwxrwxrwx. 1 zhangsan zhangsan 0 3月 12 10:50 a.txt
```

**验证 746**

```
$ chmod 746 file.txt && ls -l file.txt
-rwxrw-r--. 1 zhangsan zhangsan 0 3月 12 11:00 file.txt
```

**符号法常见写法**

```bash
chmod u+x demo.sh        # 给属主增加执行权限（等价 chmod +x demo.sh）
chmod go-r a.txt         # 取消属组和其他人的读权限
chmod -R 755 /tmp/test   # 递归设置目录权限
chown zhangsan:zhangsan a.txt && chmod 644 a.txt   # 属主可写、其他人只读
```

> `touch` 创建的普通文件默认权限一般是 `664`，目录默认是 `775`（受 `umask` 影响）。

---

## 20、> >> —— 输出重定向（redirect）

### 语法

| 符号 | 作用 |
| --- | --- |
| `>` | 标准输出重定向到文件，**覆盖**原内容 |
| `>>` | 标准输出**追加**到文件末尾 |
| `2>` | 错误输出重定向 |
| `2>&1` 或 `&>` | 正确与错误输出一起重定向 |
| `<` | 输入重定向 |
| `tee` | 既输出屏幕又写文件 |

### 功能与示例

**在 a.txt 末尾追加“最后一行”**

```bash
cd ~
echo "最后一行" >> a.txt      # 追加，不覆盖原内容
cat a.txt                    # 查看结果
```

```
$ echo "第一行" > a.txt        # 覆盖写入
$ cat a.txt
第一行
$ echo "最后一行" >> a.txt     # 追加写入
$ cat a.txt
第一行
最后一行
```

```bash
echo "新内容" > a.txt
cat a.txt          # 此时 a.txt 只剩“新内容”
```

---

## 21、| —— 管道操作（pipe）

### 语法

```bash
命令A | 命令B          # 把命令 A 的标准输出当作命令 B 的标准输入
wc -l 文件             # 统计行数（-l 行数，-w 单词数，-c 字节数）
```

可多级串联：`cat /etc/passwd | grep "/bin/bash" | wc -l`

### 功能与示例

**查看系统有多少普通用户**

```bash
cat /etc/passwd | grep "/bin/bash" | wc -l
```

```
$ cat /etc/passwd | grep "/bin/bash" | wc -l
2

$ cat /etc/passwd | grep "/bin/bash"
root:x:0:0:root:/root:/bin/bash
zhangsan:x:1000:1000:zhangsan:/home/zhangsan:/bin/bash
```

原理拆解：

1. `cat /etc/passwd`：输出所有用户行；
2. `grep "/bin/bash"`：只保留登录 Shell 为 `/bin/bash` 的行（通常是可登录的普通用户，root 也会被算进去）；
3. `wc -l`：统计行数，即用户个数。

**配套命令 wc —— 统计行数、单词数、字符数（word count）**

```bash
wc -l /etc/passwd      # 只统计行数
wc /etc/passwd         # 行数 单词数 字节数
```

> 注意：`wc –l` 里的 `–` 若是全角/长破折号，必须写成半角短横线 **`-l`**，否则命令报错。
> `stat` 是查看文件属性（大小、时间等），不能统计行数/单词数。

**其他管道写法**

```bash
grep -c "/bin/bash" /etc/passwd                          # 等价写法
awk -F: '$3>=1000 && $3<65534' /etc/passwd | wc -l       # 只算 UID>=1000 的普通用户
grep "/bin/bash" /etc/passwd | cut -d: -f1               # 只列用户名
```

---

## 22、demo.sh —— 通过脚本运行一组命令

### 语法

```bash
#!/bin/bash          # shebang：指明解释器，必须放在脚本第一行
# 注释以 # 开头
命令1
命令2
```

| 运行方式 | 说明 |
| --- | --- |
| `./demo.sh` | 需要执行权限（`chmod +x`），用脚本首行指定的解释器 |
| `bash demo.sh` | 不需要执行权限，显式指定用 bash 解释执行 |
| `source demo.sh` / `. demo.sh` | 在当前 Shell 中执行，脚本里的变量会留在当前终端 |

### 功能与示例

**创建脚本 demo.sh**（`vim demo.sh` 输入以下内容）

```bash
#!/bin/bash          # 这是注释，# 开头
echo "=====开始执行脚本===="
pwd
ls -l /home
ps -ef | grep ssh
echo "脚本执行结束"
```

也可用一条命令直接生成：

```bash
cat > demo.sh << 'EOF'
#!/bin/bash
echo "=====开始执行脚本===="
pwd
ls -l /home
ps -ef | grep ssh
echo "脚本执行结束"
EOF
```

**修改权限**

```bash
chmod +x demo.sh
ls -l demo.sh          # 权限中出现 x，如 -rwxr-xr-x
```

**运行脚本**

```bash
./demo.sh              # 方式1：需要执行权限
bash demo.sh           # 方式2：不需要执行权限
```

```
=====开始执行脚本====
/home/zhangsan
total 4
drwx------. 3 zhangsan zhangsan 78 3月 12 09:00 zhangsan
root       1234      1  0 09:00 ?        00:00:00 /usr/sbin/sshd -D
root       5678   1234  0 09:10 ?        00:00:00 sshd: zhangsan [priv]
脚本执行结束
```

> `ps -ef | grep ssh` 中最后可能出现 `grep ssh` 自己那一行，属正常现象；只匹配进程本身可用 `ps -ef | grep [s]sh` 或 `pgrep -a ssh`。

---

## 23、work.sh —— 脚本中的重定向、管道与后台运行

### 语法

| 语法 | 含义 |
| --- | --- |
| `$(date)` 或 `` `date` `` | 命令替换（command substitution），把命令输出嵌入字符串 |
| `>` | 重定向（redirect）：把命令结果写入文件（覆盖） |
| `\|` | 管道（pipe）：上一条命令的输出作为下一条命令的输入 |
| `&` | 把命令放到后台执行（background） |
| `$!` | **上一个后台进程的 PID**（Shell 内置变量） |
| `$?` | 上一条命令的退出状态 |
| `$$` | 当前脚本/Shell 的 PID |
| `jobs` / `fg` / `bg` | 查看、切到前台、放到后台运行作业 |

### 功能与示例

**脚本 work.sh 内容**

```bash
#!/bin/bash

echo "脚本开始时间：$(date)"

# 重定向，把ls结果保存文件
ls /etc > etc_list.txt

# 管道过滤进程
ps -ef | grep bash > bash_proc.txt

# 后台运行sleep命令
sleep 20 &

echo "后台进程PID: $!"
echo "脚本结束"
```

**运行脚本**

```bash
chmod +x work.sh
./work.sh
```

```
脚本开始时间：2024年 03月 12日 星期二 10:58:31 CST
后台进程PID: 3421
脚本结束
```

**检查生成的文件**

```bash
ls -l etc_list.txt bash_proc.txt
head etc_list.txt
head bash_proc.txt
```

```
$ head -3 etc_list.txt
adjtime
aliases
aliases.db

$ head -3 bash_proc.txt
root       1101      1  0 09:00 ?  00:00:00 /bin/bash /usr/sbin/...
zhangsan   3402   3380  0 10:58 pts/0  00:00:00 /bin/bash ./work.sh
...
```

逐行解释：

1. `echo "脚本开始时间：$(date)"`：打印当前日期时间；
2. `ls /etc > etc_list.txt`：把 `/etc` 的目录列表写入当前目录下的 `etc_list.txt`（已存在则**覆盖**）；
3. `ps -ef | grep bash > bash_proc.txt`：把所有进程中含 `bash` 的行保存到 `bash_proc.txt`；
4. `sleep 20 &`：后台睡眠 20 秒，`&` 让脚本不必等它结束；
5. `echo "后台进程PID: $!"`：输出刚才那个后台进程的 PID。

> `$!` 代表上一个后台进程的 PID（Shell 内置变量）；脚本重复执行时 `>` 会覆盖旧文件，想保留历史可改用 `>>` 追加。
> 后台 `sleep 20` 会在 20 秒后自动结束，可用 `ps -ef | grep sleep` 或 `jobs` 查看，必要时 `kill <PID>` 结束它。

---

## 附录：命令速查表

| 分类 | 命令（完整单词） | 示例 |
| --- | --- | --- |
| 目录 | `pwd`（print working directory） | `pwd` |
| 目录 | `cd`（change directory） | `cd /usr/local`、`cd ..`、`cd ~`、`cd -` |
| 目录 | `ls`（list） | `ls -al`、`ls -lh`、`ls -R /usr` |
| 目录 | `mkdir`（make directory） | `mkdir a`、`mkdir -p a1/a2/a3/a4` |
| 目录 | `rmdir`（remove directory） | `rmdir a`、`rmdir -p a1/a2/a3/a4` |
| 文件 | `cp`（copy） | `cp ~/.bashrc /usr/bashrc1`、`cp -r /tmp/test /usr` |
| 文件 | `mv`（move） | `mv /usr/bashrc1 /usr/test`、`mv /usr/test /usr/test2` |
| 文件 | `rm`（remove） | `rm -r /tmp/test`、`rm -rf /tmp/*` |
| 查看 | `cat`（concatenate） | `cat -n ~/.bashrc` |
| 查看 | `tac`（cat backwards） | `tac ~/.bashrc` |
| 查看 | `more`（more） / `less`（less） | `more -20 ~/.bashrc`、`less ~/.bashrc` |
| 查看 | `head`（head） | `head -20 ~/.bashrc` |
| 查看 | `tail`（tail） | `tail -20 ~/.bashrc`、`tail -f /var/log/messages` |
| 文件属性 | `touch`（touch） | `touch hello`、`touch -d "5 days ago" hello` |
| 文件属性 | `chmod`（change mode） | `chmod 777 a.txt`、`chmod 746 file.txt` |
| 文件属性 | `chown`（change owner） | `chown root /tmp/hello`、`chown -R user:group dir` |
| 文件属性 | `chgrp`（change group） | `chgrp root /tmp/hello` |
| 查找 | `find`（find） | `find ~ -name ".bashrc"` |
| 查找 | `grep`（global regular expression print） | `grep -in "examples" ~/.bashrc` |
| 压缩 | `tar`（tape archive） | `tar -czvf /test.tar.gz test`、`tar -xzvf /test.tar.gz -C /tmp` |
| 输出 | `echo`（echo） | `echo $JAVA_HOME`、`echo "x" >> a.txt` |
| 统计 | `wc`（word count） | `wc -l`、`wc` |
| 进程 | `ps`（process status） | `ps -ef \| grep bash` |
| 用户 | `whoami`（who am I） / `id`（identity） | `whoami`、`id` |
| 权限 | `su`（substitute user） / `sudo`（superuser do） | `su -`、`sudo chown root /tmp/hello` |
| 帮助 | `man`（manual） / `--help` | `man ls`、`ls --help` |

## 易错点提醒

1. `wc –l` 中的 `–` 必须写成半角 `-l`，否则报 “invalid option”。
2. `mkdir a1/a2/a3/a4` 必须加 `-p`，否则父目录不存在会报错。
3. `rmdir` 只能删**空目录**，非空目录要用 `rm -r`；删 `/tmp` 下全部内容用 `rm -rf /tmp/*`，不要写成 `rm -rf /tmp`。
4. `>` 是覆盖，`>>` 才是追加，追加“最后一行”必须用 `>>`。
5. `cp` 复制目录必须加 `-r`/`-a`；`cp` 是复制（源文件保留），`mv` 是移动/改名（源文件消失）。
6. 直接操作 `/usr`、`/` 等系统目录前要确认权限，普通用户需 `sudo`。
7. 脚本第一行 `#!/bin/bash` 不能省；`./脚本名` 需要执行权限，`bash 脚本名` 不需要。
8. `$!` 是上一个**后台**进程 PID，`$?` 是上一条命令退出状态，不要混用。
9. 权限数字口诀：7=rwx、6=rw-、5=r-x、4=r--、0=---；`chmod 746` = `rwxrw-r--`。
10. 分页查看要能**上下移动光标**用 `less`，只能向下翻页的是 `more`；查文件内容中的字符串用 `grep`。
