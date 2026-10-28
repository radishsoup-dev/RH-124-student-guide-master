# RH-124-student-guide-master

ssh -i mylab.pem remoteuser@remotehost

exit 命令  Ctrl+D 组合键

whoami

在 shell 中输入的每个字符串最多可以包含三个部分：命令、选项（通常以连字符 - 或双连字符 -- 开头）以及参数。

command1 ; command2

date date +%R date +%x

不带任何选项的 passwd 命令用于更改当前用户的密码。要更改密码，首先需要输入该账户的原始密码。

file /etc/passwd
file /home
file /bin/passwd

cat file1 file2

less

head tail -n 3 /etc/passwd

wc 命令用于统计文件中的行数、单词数和字符数。使用 -l、-w 或 -c 选项可分别仅显示对应的行数、单词数或字符数。

wc -l /etc/passwd ; wc -l /etc/group
wc -c /etc/group /etc/hosts

useradd 

要在多行中编写同一个命令，请使用反斜杠（\），它被称为转义字符。反斜杠会忽略其后面一个字符的含义。

head -n 3 \
/usr/share/dict/words \
/usr/share/dict/linux.words

history 
感叹号字符（!）是一个元字符，用于在不重新输入的情况下展开先前的命令。!number 命令会展开为与指定编号匹配的命令。!string 命令会展开为以指定字符串开头的最近一条命令。
让历史命令只展开、不执行？

Alt+. 多次Alt+.  Esc+.

| 快捷键 | 功能说明 |
|---|---|
| `Ctrl+A` | 将光标移动到命令行的开头。 |
| `Ctrl+E` | 将光标移动到命令行的末尾。 |
| `Ctrl+U` | 删除从光标位置到命令行开头的所有内容。 |
| `Ctrl+K` | 删除从光标位置到命令行末尾的所有内容。 |
| `Ctrl+LeftArrow` | 将光标移动到命令行中前一个单词的开头。 |
| `Ctrl+RightArrow` | 将光标移动到命令行中后一个单词的末尾。 |
| `Ctrl+R` | 按指定的匹配内容反向搜索命令历史记录。 |

pwd

touch 

mkdir 命令的 -p（父目录）选项会为请求的目标路径创建任何缺失的父目录。

ls -R Thesis/

cp thesis_chapter1.txt thesis_chapter2.txt Thesis ProjectX两个目录作为参数列出：Thesis 和 ProjectX 目录。最后一个参数 ProjectX 目录是目标，作为目标是有效的。Thesis 参数被 cp 命令忽略

cp命令的-r选项来复制目录及其内容。

mv 命令的 -v 选项来显示命令操作的详细输出。

每个文件最初都有一个硬链接，即从其初始名称到文件系统上数据的链接。当您为文件创建硬链接时，您创建了另一个指向该相同数据的名称。新的硬链接与原始文件名的作用完全相同。创建链接后，您无法区分新的硬链接和文件的原始名称。
您可以使用 ls -l 命令来确定文件是否具有多个硬链接。

使用 ln 命令来创建一个指向现有文件的硬链接（另一个文件名）。该命令至少需要两个参数：现有文件的路径，以及你想要创建的硬链接的路径。
ln newfile.txt /tmp/newfile-hlink2.txt

要确定两个文件是否存在硬链接关系，可使用 ls 命令的 -i 选项列出每个文件的 inode 编号。如果这两个文件位于同一文件系统且 inode 编号相同，那么它们就是指向相同数据文件内容的硬链接。

ls -il newfile.txt /tmp/newfile-hlink2.txt

硬链接只能用于常规文件。你不能使用 ln 命令为目录或特殊文件创建硬链接。
其次，只有当两个文件位于同一个文件系统时，你才能使用硬链接。

使用 df 命令列出位于不同文件系统的目录。

ln 命令的 -s 选项用于创建符号链接

符号链接可以链接位于不同文件系统上的两个文件。
符号链接可以指向目录或特殊文件，而不仅仅是常规文件。
在以下示例中，ln -s 命令为 /home/user/newfile-link2.txt 文件创建了一个符号链接。该符号链接的名称为 /tmp/newfile-symlink.txt。
ln -s /home/user/newfile-link2.txt /tmp/newfile-symlink.txt

"dangling symbolic link"

硬链接将名称指向存储设备上的数据。
符号链接将名称指向另一个名称，而该名称再指向存储设备上的数据。

cd 命令会使用符号链接的名称而非实际目录的名称来更新当前工作目录。如果您希望使用实际目录的名称来更新当前工作目录，可以使用 -P 选项。

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

ls [ac]*
ls ????
echo ~root

echo {Sunday,Monday,Tuesday,Wednesday}.log
echo file{1..3}.txt
echo file{a,b}{1,2}.txt
echo file{a{1,2},b,c}.txt

mkdir ../RHEL{7,8,9}

VARIABLENAME=value
echo $USERNAME
echo ${USERNAME} 

Command Substitution
echo Today is $(date +%A).

单引号会阻止所有的 shell 扩展。双引号会阻止大多数的 shell 扩展。
双引号会抑制通配符展开和 shell 扩展，但仍允许命令替换和变量替换。

/dev/null 特殊文件
2> file
&> file
&>> file

tee

set 命令
