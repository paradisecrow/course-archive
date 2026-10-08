# Linux 常用命令学习笔记

基于课程作业个人整理的 Linux 命令行学习笔记，覆盖目录与用户信息、文件与目录操作、权限管理、
查找与压缩、管道与重定向、Shell 脚本等主题。每个命令均给出**语法、常用选项、功能说明与可运行示例**。
仅作为扫盲使用，不适合有一定Linux基础的人使用。

## 目录

- [Linux 常用命令笔记](linux/linux-commands-notes.md)

## 笔记内容

1. 了解 Linux 及用户信息
2. `cd` —— 切换目录（change directory）
3. `ls` —— 查看文件与目录（list）
4. `mkdir` —— 新建目录（make directory）
5. `rmdir` —— 删除空目录（remove directory）
6. `cp` —— 复制文件或目录（copy）
7. `mv` —— 移动文件与目录，或更名（move）
8. `cat` —— 查看文件内容（concatenate）
9. `tac` —— 反向列示（cat backwards）
10. `more` —— 一页一页翻动查看
11. `head` —— 取出前面几行
12. `tail` —— 取出后面几行
13. `touch` —— 修改文件时间或创建新文件
14. `chown` —— 修改文件所有者（change owner）
15. `find` —— 文件查找
16. `tar` —— 打包压缩（tape archive）
17. `grep` —— 查找字符串（global regular expression print）
18. `echo` —— 回显与查看变量
19. `chmod` —— 文件访问权限（change mode）
20. `>` `>>` —— 输出重定向（redirect）
21. `|` —— 管道操作（pipe）
22. `demo.sh` —— 通过脚本运行一组命令
23. `work.sh` —— 脚本中的重定向、管道与后台运行

附录：命令速查表、易错点提醒。

## 使用说明

- 运行环境：CentOS / Ubuntu，Shell 为 `bash`
- 文档约定：`$` 表示普通用户提示符，`#` 表示 root 提示符，`~` 表示当前用户主目录
- 直接点击上面的链接即可在 GitHub 上阅读；本地可用任意支持 Markdown 的编辑器查看

## 仓库结构

```
linux-notes/
├── README.md                      # 本文件：仓库说明与索引
├── .gitignore
└── linux/
    └── linux-commands-notes.md    # Linux 常用命令笔记
```

## 计划

后续会按课程/主题继续添加笔记，每个主题一个子目录、一份 Markdown 文件，并在本文件的「目录」中登记。

## License

笔记内容为个人学习整理，可自由参考使用。
