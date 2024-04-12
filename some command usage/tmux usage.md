## 1.安装
```bash
# Ubuntu 或 Debian
$ sudo apt-get install tmux

# CentOS 或 Fedora
$ sudo yum install tmux

# Mac
$ brew install tmux
```
## 2.启动与退出
```bash
$ tmux  # 进入会话窗口

# 按下Ctrl+d或者
$ exit # 退出会话窗口
```
## 3.前缀快捷键

- `ctrl+b`唤起快捷键
- 帮助信息 : `ctrl+b`后按下?
- 按下`esc`或者`q`键退出帮助模式
## 4.会话管理
### 4.1 新建会话

- 第一个启动的Tmux窗口编号为0，然后依次类推
- 或者直接起个别名
```bash
$ tmux new -s <session-name>
```
### 4.2 将当前会话与终端分离

- 命令的方式
```bash
$ tmux detach
```

- 快捷键的方式
   - `Ctrl + b  d`
### 4.3 查看会话
```bash
$ tmux ls
# or
$ tmux list-session
```
### 4.4 接入会话
```bash
# 使用会话编号
$ tmux attach -t 0

# 使用会话名称
$ tmux attach -t <session-name>
```
### 4.5 杀死会话
```bash

# 使用会话编号
$ tmux kill-session -t 0

# 使用会话名称
$ tmux kill-session -t <session-name>
```
### 4.6 切换会话
```bash
# 使用会话编号
$ tmux switch -t 0

# 使用会话名称
$ tmux switch -t <session-name>
```
### 4.7 重命名会话
```bash
$ tmux rename-session -t 0 <new-name>
```
### 4.8 快捷键总览

- Ctrl+b d：分离当前会话。
- Ctrl+b s：列出所有会话。
- Ctrl+b $：重命名当前会话。

## 5.窗格操作
### 5.1划分窗格
```bash

# 划分上下两个窗格
$ tmux split-window

# 划分左右两个窗格
$ tmux split-window -h
```
### 5.2 移动光标
```bash
# 光标切换到上方窗格
$ tmux select-pane -U

# 光标切换到下方窗格
$ tmux select-pane -D

# 光标切换到左边窗格
$ tmux select-pane -L

# 光标切换到右边窗格
$ tmux select-pane -R
```
### 5.3 交换窗格的位置
```bash

# 当前窗格上移
$ tmux swap-pane -U

# 当前窗格下移
$ tmux swap-pane -D
```
### 5.4 快捷键操作
```
Ctrl+b %：划分左右两个窗格。
Ctrl+b "：划分上下两个窗格。
Ctrl+b <arrow key>：光标切换到其他窗格。<arrow key>是指向要切换到的窗格的方向键，比如切换到下方窗格，就按方向键↓。
Ctrl+b ;：光标切换到上一个窗格。
Ctrl+b o：光标切换到下一个窗格。
Ctrl+b {：当前窗格与上一个窗格交换位置。
Ctrl+b }：当前窗格与下一个窗格交换位置。
Ctrl+b Ctrl+o：所有窗格向前移动一个位置，第一个窗格变成最后一个窗格。
Ctrl+b Alt+o：所有窗格向后移动一个位置，最后一个窗格变成第一个窗格。
Ctrl+b x：关闭当前窗格。
Ctrl+b !：将当前窗格拆分为一个独立窗口。
Ctrl+b z：当前窗格全屏显示，再使用一次会变回原来大小。
Ctrl+b Ctrl+<arrow key>：按箭头方向调整窗格大小。
Ctrl+b q：显示窗格编号。
```
## 6.窗口管理

- 除了划分窗格，还可以新建窗口
### 6.1新建窗口
```bash

$ tmux new-window

# 新建一个指定名称的窗口
$ tmux new-window -n <window-name>
```
### 6.2 切换窗口
```bash
# 切换到指定编号的窗口
$ tmux select-window -t <window-number>

# 切换到指定名称的窗口
$ tmux select-window -t <window-name>
```
### 6.3重命名窗口
```bash
$ tmux rename-window <new-name>
```
### 6.4 快捷键
```bash
Ctrl+b c：创建一个新窗口，状态栏会显示多个窗口的信息。
Ctrl+b p：切换到上一个窗口（按照状态栏上的顺序）。
Ctrl+b n：切换到下一个窗口。
Ctrl+b <number>：切换到指定编号的窗口，其中的<number>是状态栏上的窗口编号。
Ctrl+b w：从列表中选择窗口。
Ctrl+b ,：窗口重命名。
```

## 7.其他命令
```bash
# 列出所有快捷键，及其对应的 Tmux 命令
$ tmux list-keys

# 列出所有 Tmux 命令及其参数
$ tmux list-commands

# 列出当前所有 Tmux 会话的信息
$ tmux info

# 重新加载当前的 Tmux 配置
$ tmux source-file ~/.tmux.conf
```

> cite: https://www.ruanyifeng.com/blog/2019/10/tmux.html