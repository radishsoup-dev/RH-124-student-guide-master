# RH-124 Student Guide

## SSH 远程连接与 Shell 会话

### SSH 连接远程主机

```bash
ssh -i mylab.pem remoteuser@remotehost
```

### 退出 Shell

- `exit` 命令
- `Ctrl+D` 组合键

### 查看当前用户

```bash
whoami
```

## Shell 命令基础

### 命令的组成

在 Shell 中输入的每个字符串最多可以包含三个部分：命令、选项（通常以连字符 `-` 或双连字符 `--` 开头）以及参数。

### 连续执行命令

```bash
command1 ; command2
```

### 日期与时间

```bash
date
date +%R
date +%x
```

## 用户与密码管理

### 修改当前用户的密码

不带任何选项的 `passwd` 命令用于更改当前用户的密码。要更改密码，首先需要输入该账户的原始密码。

```bash
passwd
```

### 创建用户

```bash
useradd
```

## 文件查看与内容统计

### 识别文件类型

```bash
file /etc/passwd
file /home
file /bin/passwd
```

### 查看文件内容

使用 `cat` 输出一个或多个文件的内容：

```bash
cat file1 file2
```

使用 `less` 分页查看文件内容：

```bash
less
```

### 查看文件开头与末尾

```bash
head
tail -n 3 /etc/passwd
```

### 统计文件内容

`wc` 命令用于统计文件中的行数、单词数和字符数。使用 `-l`、`-w` 或 `-c` 选项可分别仅显示对应的行数、单词数或字符数。

```bash
wc -l /etc/passwd ; wc -l /etc/group
wc -c /etc/group /etc/hosts
```

| 选项 | 含义 |
|---|---|
| `-l` | 行数 |
| `-w` | 单词数 |
| `-c` | 字节数 |

## 多行命令与续行符

要在多行中编写同一个命令，请使用反斜杠（`\`），它被称为转义字符。反斜杠会忽略其后面一个字符的含义。

```bash
head -n 3 \
/usr/share/dict/words \
/usr/share/dict/linux.words
```

## 命令历史与快捷键

### 查看历史记录

```bash
history
```

感叹号字符（`!`）是一个元字符，用于在不重新输入的情况下展开先前的命令。

- `!number` 命令会展开为与指定编号匹配的命令。
- `!string` 命令会展开为以指定字符串开头的最近一条命令。

**待研究：** 让历史命令只展开、不执行？

### 快速引用上一条命令的参数

```text
Alt+.
多次 Alt+.
Esc+.
```

### Shell 命令行快捷键

| 快捷键 | 功能说明 |
|---|---|
| `Ctrl+A` | 将光标移动到命令行的开头。 |
| `Ctrl+E` | 将光标移动到命令行的末尾。 |
| `Ctrl+U` | 删除从光标位置到命令行开头的所有内容。 |
| `Ctrl+K` | 删除从光标位置到命令行末尾的所有内容。 |
| `Ctrl+LeftArrow` | 将光标移动到命令行中前一个单词的开头。 |
| `Ctrl+RightArrow` | 将光标移动到命令行中后一个单词的末尾。 |
| `Ctrl+R` | 按指定的匹配内容反向搜索命令历史记录。 |

## 文件与目录管理

### 查看当前工作目录

```bash
pwd
```

### 创建文件

```bash
touch
```

### 创建目录

`mkdir` 命令的 `-p`（父目录）选项会为请求的目标路径创建任何缺失的父目录。

```bash
mkdir -p directory/subdirectory
```

### 递归列出目录内容

```bash
ls -R Thesis/
```

### 复制文件

```bash
cp thesis_chapter1.txt thesis_chapter2.txt Thesis ProjectX
```

两个目录作为参数列出：`Thesis` 和 `ProjectX` 目录。最后一个参数 `ProjectX` 目录是目标，作为目标是有效的。`Thesis` 参数被 `cp` 命令忽略。

`cp` 命令的 `-r` 选项用于复制目录及其内容。

### 移动文件

`mv` 命令的 `-v` 选项用于显示命令操作的详细输出。

## 硬链接与符号链接

### 硬链接的基本概念

每个文件最初都有一个硬链接，即从其初始名称到文件系统上数据的链接。当您为文件创建硬链接时，您创建了另一个指向该相同数据的名称。新的硬链接与原始文件名的作用完全相同。创建链接后，您无法区分新的硬链接和文件的原始名称。

您可以使用 `ls -l` 命令来确定文件是否具有多个硬链接。

### 创建硬链接

使用 `ln` 命令来创建一个指向现有文件的硬链接（另一个文件名）。该命令至少需要两个参数：现有文件的路径，以及你想要创建的硬链接的路径。

```bash
ln newfile.txt /tmp/newfile-hlink2.txt
```

### 通过 inode 判断硬链接关系

要确定两个文件是否存在硬链接关系，可使用 `ls` 命令的 `-i` 选项列出每个文件的 inode 编号。如果这两个文件位于同一文件系统且 inode 编号相同，那么它们就是指向相同数据文件内容的硬链接。

```bash
ls -il newfile.txt /tmp/newfile-hlink2.txt
```

### 硬链接的限制

硬链接只能用于常规文件。你不能使用 `ln` 命令为目录或特殊文件创建硬链接。

其次，只有当两个文件位于同一个文件系统时，你才能使用硬链接。

使用 `df` 命令列出位于不同文件系统的目录。

```bash
df
```

### 创建符号链接

`ln` 命令的 `-s` 选项用于创建符号链接。

符号链接具有以下特点：

- 符号链接可以链接位于不同文件系统上的两个文件。
- 符号链接可以指向目录或特殊文件，而不仅仅是常规文件。

在以下示例中，`ln -s` 命令为 `/home/user/newfile-link2.txt` 文件创建了一个符号链接。该符号链接的名称为 `/tmp/newfile-symlink.txt`。

```bash
ln -s /home/user/newfile-link2.txt /tmp/newfile-symlink.txt
```

### 悬空符号链接

```text
dangling symbolic link
```

### 硬链接与符号链接的区别

- 硬链接将名称指向存储设备上的数据。
- 符号链接将名称指向另一个名称，而该名称再指向存储设备上的数据。

### 使用 `cd -P` 处理符号链接路径

`cd` 命令会使用符号链接的名称而非实际目录的名称来更新当前工作目录。如果您希望使用实际目录的名称来更新当前工作目录，可以使用 `-P` 选项。

## Shell 通配符与路径名展开

### 通配符模式

| 模式 | 含义 |
|---|---|
| `*` | 匹配任意长度的字符串，包括空字符串。 |
| `?` | 匹配任意单个字符。 |
| `[abc…]` | 匹配方括号所列字符中的任意一个字符。 |
| `[!abc…]` | 匹配方括号所列字符以外的任意一个字符。 |
| `[^abc…]` | 匹配方括号所列字符以外的任意一个字符。 |
| `[[:alpha:]]` | 匹配任意字母字符。 |
| `[[:lower:]]` | 匹配任意小写字母。 |
| `[[:upper:]]` | 匹配任意大写字母。 |
| `[[:alnum:]]` | 匹配任意字母或数字字符。 |
| `[[:punct:]]` | 匹配任意可打印的标点字符，即既不是空白字符，也不是字母或数字的可打印字符。 |
| `[[:digit:]]` | 匹配任意一位十进制数字（`0`–`9`）。 |
| `[[:space:]]` | 匹配任意单个空白字符，可能包括制表符、换行符、回车符、换页符或空格。 |

### 通配符示例

```bash
ls [ac]*
ls ????
```

### 用户主目录展开

```bash
echo ~root
```

## Shell 花括号展开

### 使用花括号生成多个字符串

```bash
echo {Sunday,Monday,Tuesday,Wednesday}.log
echo file{1..3}.txt
echo file{a,b}{1,2}.txt
echo file{a{1,2},b,c}.txt
```

### 批量创建目录

```bash
mkdir ../RHEL{7,8,9}
```

## Shell 变量与命令替换

### 变量赋值与引用

```bash
VARIABLENAME=value
echo $USERNAME
echo ${USERNAME}
```

### 命令替换（Command Substitution）

```bash
echo Today is $(date +%A).
```

## 引号与 Shell 扩展

单引号会阻止所有的 shell 扩展。

双引号会阻止大多数的 shell 扩展。

双引号会抑制通配符展开和 shell 扩展，但仍允许命令替换和变量替换。

## 输入输出重定向

### 特殊文件 `/dev/null`

```text
/dev/null
```

### 重定向标准错误

```bash
2> file
```

### 同时重定向标准输出与标准错误

```bash
&> file
```

### 追加标准输出与标准错误

```bash
&>> file
```

### 使用 `tee`

```bash
tee
```

## Shell 选项与环境变量

### `set` 命令

```bash
set
```

### 变量展开示例

```bash
echo Repeat ${COUNT}x
```

### Bash 历史记录相关变量

Bash 启动时会设置一些 Shell 变量。你可以修改它们以调整 Shell 的行为。

`HISTFILE`、`HISTFILESIZE` 和 `HISTTIMEFORMAT` 这几个 Shell 变量会影响 Shell 的历史记录和 `history` 命令。

- `HISTFILE` 变量指定将 Shell 历史记录保存到哪个文件，默认为 `~/.bash_history` 文件。
- `HISTFILESIZE` 变量指定在该文件中保存多少条历史命令。
- `HISTTIMEFORMAT` 变量定义了历史记录中每条命令的时间戳格式。默认情况下，该变量并不存在。

### `PS1` 变量

另一个例子是 `PS1` 变量。

### 导出环境变量

`export` 命令将 shell 中定义的任何变量标记为导出，从而将其设为环境变量。

```bash
export PATH=${PATH}:/home/user/sbin
```

### 查看环境变量

```bash
env
```

## Bash 启动文件与 Shell 环境初始化

Bash 启动时，会运行几个包含 shell 命令的文本文件来初始化 shell 环境。

相关行为取决于两个因素：

- Shell 是交互式还是非交互式。
- Shell 是登录 Shell 还是非登录 Shell。

### 交互式登录 Shell

交互式登录 shell，`/etc/profile` 和 `~/.bash_profile` 文件配置 Bash 环境。`/etc/profile` 和 `~/.bash_profile` 文件还分别引用 `/etc/bashrc` 和 `~/.bashrc` 文件。

### 交互式非登录 Shell

对于交互式非登录 shell，只有 `/etc/bashrc` 和 `~/.bashrc` 文件配置 Bash 环境。

虽然 `/etc/profile` 和 `/etc/bashrc` 文件适用于整个系统，但 `~/.bash_profile` 和 `~/.bashrc` 文件是用户特定的。

### 非交互式 Shell

非交互式 shell 会调用 `BASH_ENV` 变量定义的任何文件。默认情况下未定义此变量。

### 配置用户级 Shell 环境

要创建可用于所有交互式 shell 的变量，请编辑 `~/.bashrc` 文件。

若要在用户登录后仅应用一次变量，请在 `~/.bash_profile` 文件中定义它。

### 配置系统级 Shell 环境

将一个包含所需更改、扩展名为 `.sh` 的文件添加到 `/etc/profile.d` 目录中。要以 root 用户身份在 `/etc/profile` 目录中创建这些文件。

## 别名与取消设置

### 创建别名

```bash
alias hello='echo "Hello, this is a long string."'
```

### 取消设置变量

要取消设置并取消导出变量，请使用 `unset` 命令。

```bash
unset VARIABLE
```

### 取消导出变量

```bash
export -n PS1
```

### 删除别名

```bash
unalias hello
```

## 用户身份与权限切换

### 查看当前用户身份

```bash
id
```

### 查看指定用户身份

```bash
id user02
```

### 切换用户

```bash
su - user02
```

`su` 启动的是非登录 shell，而命令 `su -`（带 dash 选项）启动的是登录 shell。

这两个命令的主要区别在于：`su -` 会像该用户全新登录一样设置 shell 环境，而 `su` 虽然以该用户身份启动 shell，但使用的是原用户的环境设置。

## sudo 与 root 权限管理

### root 用户的密码登录限制

系统管理员会将 root 用户配置为没有有效密码。因此，用户无法直接通过密码以 root 身份登录系统。

### sudo 的日志记录

sudo 的另一个好处是默认将所有执行的命令记录到 `/var/log/secure`。

### 启动 root Shell

```bash
sudo -i
sudo -s
```

### sudoers 配置文件

`/etc/sudoers` 文件还会将 `/etc/sudoers.d` 目录下的所有文件内容包含进来，作为配置文件的一部分。

## 用户账户默认值与密码老化

### `/etc/login.defs`

`/etc/login.defs` 文件设置了用户账户的部分默认选项，例如有效的 UID 号范围和默认的密码老化规则。

该文件中的值仅影响新创建的用户账户。更改此文件不会影响现有用户。

### `chage` 命令

```bash
chage
```

示例：

```bash
chage -E $(date -d "+30 days" +%F) cloudadmin10
```

### `/sbin/nologin`

```text
/sbin/nologin
```

## Linux 文件权限

### 基本权限

| 权限 | 对文件的影响 | 对目录的影响 |
|---|---|---|
| `r`（读取） | 可以读取文件内容。 | 可以列出目录中的内容，即文件名。 |
| `w`（写入） | 可以修改文件内容。 | 可以在目录中创建或删除文件。 |
| `x`（执行） | 可以将文件作为命令执行。 | 可以进入该目录，将其设为当前工作目录。可以使用 `cd` 命令进入，但列出其中的文件还需要读取权限。 |

### 修改权限：`chmod`

```bash
chmod
```

### 修改所有者：`chown`

```bash
chown
```

### 特殊权限位

| 权限 | 对文件的影响 | 对目录的影响 |
|---|---|---|
| `u+s`（SUID，设置用户 ID） | 执行文件时，进程使用该文件所有者的有效用户 ID，而不是执行该文件的用户的有效用户 ID。 | 无影响。 |
| `g+s`（SGID，设置组 ID） | 执行文件时，进程使用该文件所属组的有效组 ID。 | 在该目录中创建的文件，其所属组会与该目录的所属组一致。 |
| `o+t`（Sticky Bit，粘滞位） | 无影响。 | 对目录具有写权限的用户，只能删除自己拥有的文件；不能删除或强制保存覆盖其他用户拥有的文件。 |

### 默认权限掩码：`umask`

```bash
umask
```

## Linux 进程状态

| 名称 | 标志 | 内核定义的状态名称与说明 |
|---|---|---|
| 运行（Running） | `R` | `TASK_RUNNING`：进程正在 CPU 上执行，或正在等待获得 CPU 执行机会。进程既可以正在执行用户态代码或内核态代码（系统调用），也可以处于就绪队列中，等待运行。 |
| 睡眠（Sleeping） | `S` | `TASK_INTERRUPTIBLE`：进程正在等待某个条件满足，例如硬件请求完成、系统资源可用或收到信号。当相应事件发生或信号到达时，进程会转回运行状态。 |
| 睡眠（不可中断） | `D` | `TASK_UNINTERRUPTIBLE`：进程同样处于睡眠状态，但与 `S` 状态不同，它不会响应普通信号。此状态仅用于中断进程可能导致设备状态不可预测的情况。 |
| 可终止睡眠（Killable） | `K` | `TASK_KILLABLE`：与不可中断的 `D` 状态类似，但经过特殊处理，允许等待中的任务响应要求其被终止的信号，从而彻底退出。许多进程查看工具会将此状态显示为 `D`。 |
| 空闲（Idle） | `I` | `TASK_REPORT_IDLE`：属于 `D` 状态的一个子集。内核在计算平均负载时不会将此类进程计入，通常用于内核线程。其标志包括 `TASK_UNINTERRUPTIBLE` 和 `TASK_NOLOAD`。它与 `TASK_KILLABLE` 类似，也属于 `D` 状态的一个子集，并且可以接受致命信号。 |
| 停止（Stopped） | `T` | `TASK_STOPPED`：进程已停止（暂停运行），通常是由于收到用户或其他进程发送的信号。之后可以通过另一个信号使其继续运行。 |
| 跟踪停止（Traced） | `t` | `TASK_TRACED`：正在被调试器跟踪的进程也会暂时停止，其状态标志同样为 `T`。 |
| 僵尸（Zombie） | `Z` | `EXIT_ZOMBIE`：子进程退出时，会向父进程报告自身的退出状态。此时，除用于标识进程的信息（PID 等）外，其他资源均已释放。 |
| 已死亡（Dead） | `X` | `EXIT_DEAD`：父进程完成清理并回收剩余的子进程结构后，该进程才算被彻底释放。此状态无法通过常规进程列表工具观察到。 |
