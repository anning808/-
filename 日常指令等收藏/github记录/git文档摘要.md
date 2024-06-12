https://git-scm.com/docs/git/zh_HANS-CN
## 名称

git - 简单的内容追踪系统

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%A6%82%E8%BF%B0)概述

_git_ [-v | --version] [-h | --help] [-C <启动路径>] [-c <配置名>=<配置值>]
    [--exec-path[=<核心程序路径>]] [--html-path] [--man-path] [--info-path]
    [-p|--paginate|-P|--no-pager] [--no-replace-objects] [--bare]
    [--git-dir=<仓库路径>] [--work-tree=<工作目录路径>] [--namespace=<命名空间名>]
    [--config-env=<配置名>=<环境变量>] <命令> [<参数>]

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%8F%8F%E8%BF%B0)描述

Git 是一个快速、可扩展的分布式版本控制系统，它拥有异常丰富的命令集，可以提供高级操作和对内部的完全访问。

参阅 [gittutorial[7]](https://git-scm.com/docs/gittutorial/zh_HANS-CN) 开始使用，然后查看 [giteveryday[7]](https://git-scm.com/docs/giteveryday/zh_HANS-CN) 以获取一组有用的基础命令。 [Git 用户手册](https://git-scm.com/docs/user-manual/zh_HANS-CN)中有对 Git 更为深入的介绍。

在掌握了基本概念之后，你可以回到本页了解 Git 提供的命令。 你可以通过 "git help" 命令了解更多关于单个 Git 命令的信息。 [gitcli[7]](https://git-scm.com/docs/gitcli/zh_HANS-CN) 手册页面提供了命令行命令语法的概述。

最新 Git 文档的格式化和超文本副本可以在 [https://git.github.io/htmldocs/git.html](https://git.github.io/htmldocs/git.html) 或 [https://git-scm.com/docs上查看。](https://git-scm.com/docs%E4%B8%8A%E6%9F%A5%E7%9C%8B%E3%80%82)

## 选项

[](https://git-scm.com/docs/git/zh_HANS-CN#git--v)-v

[](https://git-scm.com/docs/git/zh_HANS-CN#git---version)--version

打印 _git_ 程序的 Git 套件版本。

此选项在内部转换为 `git version ...` 并接受与[git-version[1]](https://git-scm.com/docs/git-version/zh_HANS-CN) 命令相同的选项。如果 `--version` 后接 `--help` 选项，则会优先展示 `--version` 选项的帮助信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git--h)-h

[](https://git-scm.com/docs/git/zh_HANS-CN#git---help)--help

打印概要和最常用的命令列表。如果给出选项 `--all` 或 `-a`，则打印所有可用的命令。如果给出某个命令，该选项将调出该命令的手册页面。

因为 `git --help ...` 会在内部转换为 `git help ...`，所以有关其他可以控制手册页面显示方式的选项可以参阅 [git-help[1]](https://git-scm.com/docs/git-help/zh_HANS-CN) 来获得更详细的信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git--Cltgt)-C <启动路径>

运行时就像 git 命令在 _<启动路径>_ 而不是在当前工作目录下启动一样。 当给出多个 `-C` 选项时，每个后续的非绝对的 `-C <启动路径>` 都是相对于前一个 `-C <启动路径>` 解释的。 如果 _<启动路径>_ 存在但为空，例如 `-C ""`，那么命令就在当前工作目录启动。


这个选项会影响到像 `--git-dir` 和 `--work-tree` 这样的需要路径名的选项，因为它们会用 `-C` 选项设置的启动路径进行相对路径的解释。例如，以下的调用是等价的：

git --git-dir=a.git --work-tree=b -C c status
git --git-dir=c/a.git --work-tree=c/b status

[](https://git-scm.com/docs/git/zh_HANS-CN#git--cltgtltgt)-c <配置名>=<配置值>

向命令传递一个配置参数。给出的值将覆盖配置文件的值。 <配置名> 的格式应与 _git config_ 列出的格式相同（子键由 `.` 分隔）。

注意在 `git -c foo.bar ...` 中省略 `=` 是允许的，并会将 `foo.bar` 设置为布尔值 true（就像配置文件中的 `[foo]bar` 一样）。包括等号但有一个空值（如 `git -c foo.bar= ...` ）会将 `foo.bar` 设置为空字符串，如果 `git config --type=bool` 空值会转换为 `false`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---config-envltgtltgt)--config-env=<配置名>=<环境变量>

像 `-c <配置名>=<配置值>` 一样，给配置变量 _<配置名>_ 一个值，其中 <环境变量> 是一个环境变量的名字，可以从中获取该环境变量的值。与 `-c` 选项不同的是，没有直接将值设置为空字符串的快捷方式，除非环境变量本身就设置为空字符串。 如果 `<环境变量>` 在环境变量中不存在，则会报错。`<环境变量>` 不应当包含等号，以避免与包含等号的 `<配置名>` 产生歧义。

当你想把临时配置选项传递给 Git，但在操作系统上，其他进程可能能读取你的命令行（如 `/proc/self/cmdline`），但不能读取你的环境（如 `/proc/self/environ`）时，这个功能就很有用了。这种行为在 Linux 上是默认的，但在你的系统上可能不是。

请注意，这可能会增加变量的安全性，例如 `http.extraHeader`，因为他的敏感信息包含在值中，但并不会增加例如 `url.<原始 URL 前缀>.insteadOf` 这种变量的安全性，因为其敏感信息可能会包含在部分键中。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---exec-pathltgt)--exec-path[=<核心程序路径>]

安装 Git 核心程序的路径。 这也可以通过设置 GIT_EXEC_PATH 环境变量来控制。如果没有给出路径，_git_ 会打印当前的设置，然后退出。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---html-path)--html-path

打印 Git 的 HTML 文档的安装路径（尾部不带斜杠）然后退出。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---man-path)--man-path

打印该版本 Git man page 的 manpath（参阅 `man(1)`）之后退出。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---info-path)--info-path

打印记录该版本 Git 信息文件的安装路径并退出。

[](https://git-scm.com/docs/git/zh_HANS-CN#git--p)-p

[](https://git-scm.com/docs/git/zh_HANS-CN#git---paginate)--paginate

如果标准输出是一个终端，则将所有的输出输出到 _less（git 默认的分页器）_（或者是 $PAGER 变量设置的值）中。 这会覆盖 `pager.<cmd>` 配置选项（参阅下面的 “配置机制” 部分）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git--P)-P

[](https://git-scm.com/docs/git/zh_HANS-CN#git---no-pager)--no-pager

不使用分页器进行 Git 的输出。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---git-dirltgt)--git-dir=<仓库路径>

设置仓库的路径（".git" 目录）。这也可以通过设置 `GIT_DIR` 环境变量来控制。<仓库路径> 可以是绝对路径或是当前工作目录的相对路径。

使用该选项（或 `GIT_DIR` 环境变量）指定 ".git" 目录的位置，这会关闭对带有 ".git" 子目录仓库的扫描（这是找到仓库和顶级工作区的方式），并告诉 Git 当前在顶级工作区。 如果当前并不在工作区的顶级目录，你应该用 `--work-tree=<工作区路径>` 选项（或 `GIT_WORK_TREE` 环境变量）告诉 Git 顶级工作区在哪里

如果你只是想在 `<启动路径>` 中运行 Git，可以使用 `git -C <启动路径>`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---work-treeltgt)--work-tree=<工作区路径>

设置工作树的路径。<工作区路径> 可以是一个绝对路径或与当前工作目录相对的路径。 这也可以通过设置 GIT_WORK_TREE 环境变量和 core.worktree 配置变量来控制（参阅 [git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN) 中的 core.worktree 获取更为详细的论述）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---namespaceltgt)--namespace=<路径>

设置 Git 命名空间。 参阅 [gitnamespaces[7]](https://git-scm.com/docs/gitnamespaces/zh_HANS-CN) 以了解更多细节。 这相当于设置 `GIT_NAMESPACE` 环境变量。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---bare)--bare

将该仓库视为裸仓库。 如果没有设置 GIT_DIR 环境变量，它将在当前工作目录生成仓库。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---no-replace-objects)--no-replace-objects

不使用候补引用来替换 Git 对象。更多信息参阅 [git-replace[1]](https://git-scm.com/docs/git-replace/zh_HANS-CN)。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---literal-pathspecs)--literal-pathspecs

按字面意思处理路径规格（即不使用通配符，不使用路径规范功能）。 这相当于将 `GIT_LITERAL_PATHSPECS` 环境变量设置为 `1`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---glob-pathspecs)--glob-pathspecs

在所有的路径规范中添加 "glob" 模式匹配功能。这相当于将 `GIT_GLOB_PATHSPECS` 环境变量设置为 `1`。可以用路径规范字符 ":(literal)" 在单个路径规格上禁用 "glob" 模式匹配功能。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---noglob-pathspecs)--noglob-pathspecs

在所有路径规范中添加 "literal" 模式匹配功能。这相当于将 `GIT_NOGLOB_PATHSPECS` 环境变量设为 `1`。可以使用路径规范字符 ":(glob)" 在单个路径规范上启用通配符模式匹配功能。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---icase-pathspecs)--icase-pathspecs

在所有的路径规范中添加 "icase" 模式匹配功能。这相当于将 `GIT_ICASE_PATHSPECS` 环境变量设置为 `1`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---no-optional-locks)--no-optional-locks

禁止执行需要文件锁的可选操作。这相当于将 `GIT_OPTIONAL_LOCKS` 设置为 `0`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git---list-cmdsltgtltgt82308203)--list-cmds=<命令组>[,<命令组>…​]

按组列出命令。这是一个内部/实验性的选项，将来可能会改变或删除。支持的组有：buildins、parseopt（使用 parse-options 的内置命令）、main（libexec 目录下的所有命令）、others（ `$PATH` 中其他带有 git- 前缀的命令）、list-<目录>（参阅 command-list.txt 中的目录【原文是这么说的，但是下面那个 Git 命令章节已经列出了所有命令列表】）、nohelpers（排除辅助命令）、alias 和 config（在配置 completion.commands 中检索命令列表）

[](https://git-scm.com/docs/git/zh_HANS-CN#git---attr-sourceltgt)--attr-source=<树对象>

从 <树对象> 而不是工作区中读取 gitattributes。参见 [gitattributes[5]](https://git-scm.com/docs/gitattributes/zh_HANS-CN)。这相当于设置 `GIT_ATTR_SOURCE` 环境变量。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_git_%E5%91%BD%E4%BB%A4)Git 命令

我们把 Git 分为上层封装命令（“瓷件”）和底层核心命令（“管件”）。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E4%B8%8A%E5%B1%82%E5%B0%81%E8%A3%85%E5%91%BD%E4%BB%A4%E7%93%B7%E4%BB%B6)上层封装命令（瓷件）

我们将瓷件命令分为主要命令和一些辅助性的用户工具。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_%E4%B8%BB%E8%A6%81%E7%93%B7%E4%BB%B6%E5%91%BD%E4%BB%A4)主要瓷件命令

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-addzhHANS-CNgit-add1a)[git-add[1]](https://git-scm.com/docs/git-add/zh_HANS-CN)

将文件内容添加到索引中。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-amzhHANS-CNgit-am1a)[git-am[1]](https://git-scm.com/docs/git-am/zh_HANS-CN)

从一个邮箱中应用一系列的补丁。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-archivezhHANS-CNgit-archive1a)[git-archive[1]](https://git-scm.com/docs/git-archive/zh_HANS-CN)

从一个命名的目录中创建一个文件档案。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-bisectzhHANS-CNgit-bisect1a)[git-bisect[1]](https://git-scm.com/docs/git-bisect/zh_HANS-CN)

使用二进制搜索查找引入错误的提交。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-branchzhHANS-CNgit-branch1a)[git-branch[1]](https://git-scm.com/docs/git-branch/zh_HANS-CN)

列出、创建或删除分支。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-bundlezhHANS-CNgit-bundle1a)[git-bundle[1]](https://git-scm.com/docs/git-bundle/zh_HANS-CN)

通过存档移动对象和引用。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-checkoutzhHANS-CNgit-checkout1a)[git-checkout[1]](https://git-scm.com/docs/git-checkout/zh_HANS-CN)

切换分支或恢复工作区文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-cherry-pickzhHANS-CNgit-cherry-pick1a)[git-cherry-pick[1]](https://git-scm.com/docs/git-cherry-pick/zh_HANS-CN)

应用一些现有的提交所带来的变化。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-citoolzhHANS-CNgit-citool1a)[git-citool[1]](https://git-scm.com/docs/git-citool/zh_HANS-CN)

git-commit 的图形化替代方案。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-cleanzhHANS-CNgit-clean1a)[git-clean[1]](https://git-scm.com/docs/git-clean/zh_HANS-CN)

从工作区上删除未跟踪的文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-clonezhHANS-CNgit-clone1a)[git-clone[1]](https://git-scm.com/docs/git-clone/zh_HANS-CN)

将版本库克隆到一个新目录中。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-commitzhHANS-CNgit-commit1a)[git-commit[1]](https://git-scm.com/docs/git-commit/zh_HANS-CN)

记录对仓库的更改。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-describezhHANS-CNgit-describe1a)[git-describe[1]](https://git-scm.com/docs/git-describe/zh_HANS-CN)

根据一个可用的引用，给一个对象易于理解的名字。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-diffzhHANS-CNgit-diff1a)[git-diff[1]](https://git-scm.com/docs/git-diff/zh_HANS-CN)

显示提交之间的变化，提交和工作区，等。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-fetchzhHANS-CNgit-fetch1a)[git-fetch[1]](https://git-scm.com/docs/git-fetch/zh_HANS-CN)

从另一个仓库下载对象和引用。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-format-patchzhHANS-CNgit-format-patch1a)[git-format-patch[1]](https://git-scm.com/docs/git-format-patch/zh_HANS-CN)

为提交电子邮件准备补丁。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-gczhHANS-CNgit-gc1a)[git-gc[1]](https://git-scm.com/docs/git-gc/zh_HANS-CN)

清理不必要的文件，优化本地仓库。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-grepzhHANS-CNgit-grep1a)[git-grep[1]](https://git-scm.com/docs/git-grep/zh_HANS-CN)

打印符合模式的行。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-guizhHANS-CNgit-gui1a)[git-gui[1]](https://git-scm.com/docs/git-gui/zh_HANS-CN)

Git的可移植图形界面。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-initzhHANS-CNgit-init1a)[git-init[1]](https://git-scm.com/docs/git-init/zh_HANS-CN)

创建一个空的 Git 仓库或重新初始化现有仓库。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-logzhHANS-CNgit-log1a)[git-log[1]](https://git-scm.com/docs/git-log/zh_HANS-CN)

展示提交日志。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-maintenancezhHANS-CNgit-maintenance1a)[git-maintenance[1]](https://git-scm.com/docs/git-maintenance/zh_HANS-CN)

运行任务来优化Git存储库的数据。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-mergezhHANS-CNgit-merge1a)[git-merge[1]](https://git-scm.com/docs/git-merge/zh_HANS-CN)

将两个或更多的开发历史连接在一起。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-mvzhHANS-CNgit-mv1a)[git-mv[1]](https://git-scm.com/docs/git-mv/zh_HANS-CN)

移动或重命名一个文件、一个目录或一个符号链接。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-noteszhHANS-CNgit-notes1a)[git-notes[1]](https://git-scm.com/docs/git-notes/zh_HANS-CN)

添加或检查对象注释。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-pullzhHANS-CNgit-pull1a)[git-pull[1]](https://git-scm.com/docs/git-pull/zh_HANS-CN)

从另一个仓库或本地分支获取并整合。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-pushzhHANS-CNgit-push1a)[git-push[1]](https://git-scm.com/docs/git-push/zh_HANS-CN)

更新远程仓库引用和相关对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-range-diffzhHANS-CNgit-range-diff1a)[git-range-diff[1]](https://git-scm.com/docs/git-range-diff/zh_HANS-CN)

比较两个提交范围（例如一个分支的两个版本）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-rebasezhHANS-CNgit-rebase1a)[git-rebase[1]](https://git-scm.com/docs/git-rebase/zh_HANS-CN)

在另一个基础提示的基础上重新提交。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-resetzhHANS-CNgit-reset1a)[git-reset[1]](https://git-scm.com/docs/git-reset/zh_HANS-CN)

重置当前HEAD到指定状态。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-restorezhHANS-CNgit-restore1a)[git-restore[1]](https://git-scm.com/docs/git-restore/zh_HANS-CN)

恢复工作区文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-revertzhHANS-CNgit-revert1a)[git-revert[1]](https://git-scm.com/docs/git-revert/zh_HANS-CN)

还原一些现有的提交。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-rmzhHANS-CNgit-rm1a)[git-rm[1]](https://git-scm.com/docs/git-rm/zh_HANS-CN)

从工作区和索引中删除文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-shortlogzhHANS-CNgit-shortlog1a)[git-shortlog[1]](https://git-scm.com/docs/git-shortlog/zh_HANS-CN)

汇总 _git log_ 输出。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-showzhHANS-CNgit-show1a)[git-show[1]](https://git-scm.com/docs/git-show/zh_HANS-CN)

展示各种类型的对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-sparse-checkoutzhHANS-CNgit-sparse-checkout1a)[git-sparse-checkout[1]](https://git-scm.com/docs/git-sparse-checkout/zh_HANS-CN)

将你的工作区减少到一个被跟踪的文件子集。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-stashzhHANS-CNgit-stash1a)[git-stash[1]](https://git-scm.com/docs/git-stash/zh_HANS-CN)

把这些变化贮藏在一个脏工作目录中。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-statuszhHANS-CNgit-status1a)[git-status[1]](https://git-scm.com/docs/git-status/zh_HANS-CN)

显示工作区状态。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-submodulezhHANS-CNgit-submodule1a)[git-submodule[1]](https://git-scm.com/docs/git-submodule/zh_HANS-CN)

初始化、更新或检查子模块。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-switchzhHANS-CNgit-switch1a)[git-switch[1]](https://git-scm.com/docs/git-switch/zh_HANS-CN)

切换分支。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-tagzhHANS-CNgit-tag1a)[git-tag[1]](https://git-scm.com/docs/git-tag/zh_HANS-CN)

创建、列出、删除或校验以 GPG 签名的标签对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-worktreezhHANS-CNgit-worktree1a)[git-worktree[1]](https://git-scm.com/docs/git-worktree/zh_HANS-CN)

管理多个工作区。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitkzhHANS-CNgitk1a)[gitk[1]](https://git-scm.com/docs/gitk/zh_HANS-CN)

Git 仓库浏览器。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsscalarzhHANS-CNscalar1a)[scalar[1]](https://git-scm.com/docs/scalar/zh_HANS-CN)

管理大型 Git 仓库的工具。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_%E8%BE%85%E5%8A%A9%E5%91%BD%E4%BB%A4)辅助命令

操控：

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-configzhHANS-CNgit-config1a)[git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN)

获取和设置存储库或全局选项。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-fast-exportzhHANS-CNgit-fast-export1a)[git-fast-export[1]](https://git-scm.com/docs/git-fast-export/zh_HANS-CN)

Git 数据导出器。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-fast-importzhHANS-CNgit-fast-import1a)[git-fast-import[1]](https://git-scm.com/docs/git-fast-import/zh_HANS-CN)

快速 Git 数据导入器的后端。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-filter-branchzhHANS-CNgit-filter-branch1a)[git-filter-branch[1]](https://git-scm.com/docs/git-filter-branch/zh_HANS-CN)

重写分支。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-mergetoolzhHANS-CNgit-mergetool1a)[git-mergetool[1]](https://git-scm.com/docs/git-mergetool/zh_HANS-CN)

运行合并冲突解决工具来解决合并冲突。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-pack-refszhHANS-CNgit-pack-refs1a)[git-pack-refs[1]](https://git-scm.com/docs/git-pack-refs/zh_HANS-CN)

包装头和标签用于有效的仓库访问。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-prunezhHANS-CNgit-prune1a)[git-prune[1]](https://git-scm.com/docs/git-prune/zh_HANS-CN)

从对象数据库中修剪所有不可达的对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-reflogzhHANS-CNgit-reflog1a)[git-reflog[1]](https://git-scm.com/docs/git-reflog/zh_HANS-CN)

管理reflog信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-remotezhHANS-CNgit-remote1a)[git-remote[1]](https://git-scm.com/docs/git-remote/zh_HANS-CN)

管理一组被跟踪的仓库。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-repackzhHANS-CNgit-repack1a)[git-repack[1]](https://git-scm.com/docs/git-repack/zh_HANS-CN)

在存储库中打包未打包的对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-replacezhHANS-CNgit-replace1a)[git-replace[1]](https://git-scm.com/docs/git-replace/zh_HANS-CN)

创建、列出、删除替换对象的引用。

审阅：

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-annotatezhHANS-CNgit-annotate1a)[git-annotate[1]](https://git-scm.com/docs/git-annotate/zh_HANS-CN)

用提交信息来注释文件行。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-blamezhHANS-CNgit-blame1a)[git-blame[1]](https://git-scm.com/docs/git-blame/zh_HANS-CN)

显示文件的每一行最后修改的版本和作者。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-bugreportzhHANS-CNgit-bugreport1a)[git-bugreport[1]](https://git-scm.com/docs/git-bugreport/zh_HANS-CN)

收集信息，以便用户提交错误报告。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-count-objectszhHANS-CNgit-count-objects1a)[git-count-objects[1]](https://git-scm.com/docs/git-count-objects/zh_HANS-CN)

计数对象的解包数量和它们的磁盘消耗。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-diagnosezhHANS-CNgit-diagnose1a)[git-diagnose[1]](https://git-scm.com/docs/git-diagnose/zh_HANS-CN)

生成一个诊断信息的压缩文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-difftoolzhHANS-CNgit-difftool1a)[git-difftool[1]](https://git-scm.com/docs/git-difftool/zh_HANS-CN)

使用常见的差异工具显示变化。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-fsckzhHANS-CNgit-fsck1a)[git-fsck[1]](https://git-scm.com/docs/git-fsck/zh_HANS-CN)

验证数据库中的对象的连接性和有效性。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-helpzhHANS-CNgit-help1a)[git-help[1]](https://git-scm.com/docs/git-help/zh_HANS-CN)

显示有关 Git 的帮助信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-instawebzhHANS-CNgit-instaweb1a)[git-instaweb[1]](https://git-scm.com/docs/git-instaweb/zh_HANS-CN)

在gitweb中浏览您的工作存储库。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-merge-treezhHANS-CNgit-merge-tree1a)[git-merge-tree[1]](https://git-scm.com/docs/git-merge-tree/zh_HANS-CN)

执行合并时无需触及索引或工作目录树。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-rererezhHANS-CNgit-rerere1a)[git-rerere[1]](https://git-scm.com/docs/git-rerere/zh_HANS-CN)

重新使用记录的冲突合并的解决方案。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-show-branchzhHANS-CNgit-show-branch1a)[git-show-branch[1]](https://git-scm.com/docs/git-show-branch/zh_HANS-CN)

显示分支和它们的提交。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-verify-commitzhHANS-CNgit-verify-commit1a)[git-verify-commit[1]](https://git-scm.com/docs/git-verify-commit/zh_HANS-CN)

检查提交的 GPG 签名。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-verify-tagzhHANS-CNgit-verify-tag1a)[git-verify-tag[1]](https://git-scm.com/docs/git-verify-tag/zh_HANS-CN)

检查标签的 GPG 签名。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-versionzhHANS-CNgit-version1a)[git-version[1]](https://git-scm.com/docs/git-version/zh_HANS-CN)

显示 Git 的版本信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-whatchangedzhHANS-CNgit-whatchanged1a)[git-whatchanged[1]](https://git-scm.com/docs/git-whatchanged/zh_HANS-CN)

显示每次提交带来的差异的日志。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitwebzhHANS-CNgitweb1a)[gitweb[1]](https://git-scm.com/docs/gitweb/zh_HANS-CN)

Git web 接口（Git 仓库的 web 前端）。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_%E4%B8%8E%E5%85%B6%E4%BB%96%E5%BA%94%E7%94%A8%E4%BA%A4%E4%BA%92)与其他应用交互

这些命令是为了与外部 SCM（源代码管理工具）以及通过电子邮件与其他人进行交互。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-archimportzhHANS-CNgit-archimport1a)[git-archimport[1]](https://git-scm.com/docs/git-archimport/zh_HANS-CN)

导入一个 GNU Arch 仓库到 Git。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-cvsexportcommitzhHANS-CNgit-cvsexportcommit1a)[git-cvsexportcommit[1]](https://git-scm.com/docs/git-cvsexportcommit/zh_HANS-CN)

将单个提交导出到 CVS 检出。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-cvsimportzhHANS-CNgit-cvsimport1a)[git-cvsimport[1]](https://git-scm.com/docs/git-cvsimport/zh_HANS-CN)

把你的数据从另一个人们爱恨交加的 SCM 中解救出来。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-cvsserverzhHANS-CNgit-cvsserver1a)[git-cvsserver[1]](https://git-scm.com/docs/git-cvsserver/zh_HANS-CN)

用于 Git 的 CVS 服务模拟器。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-imap-sendzhHANS-CNgit-imap-send1a)[git-imap-send[1]](https://git-scm.com/docs/git-imap-send/zh_HANS-CN)

从标准输入流向 IMAP 文件夹发送补丁集。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-p4zhHANS-CNgit-p41a)[git-p4[1]](https://git-scm.com/docs/git-p4/zh_HANS-CN)

从 Perforce 仓库导入和提交。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-quiltimportzhHANS-CNgit-quiltimport1a)[git-quiltimport[1]](https://git-scm.com/docs/git-quiltimport/zh_HANS-CN)

将 quilt 补丁集应用到当前分支。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-request-pullzhHANS-CNgit-request-pull1a)[git-request-pull[1]](https://git-scm.com/docs/git-request-pull/zh_HANS-CN)

生成一个待处理更改摘要。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-send-emailzhHANS-CNgit-send-email1a)[git-send-email[1]](https://git-scm.com/docs/git-send-email/zh_HANS-CN)

以电子邮件的形式发送补丁集合。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-svnzhHANS-CNgit-svn1a)[git-svn[1]](https://git-scm.com/docs/git-svn/zh_HANS-CN)

Subversion 仓库和 Git 之间的双向操作。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_%E9%87%8D%E7%BD%AE%E6%81%A2%E5%A4%8D%E5%92%8C%E8%BF%98%E5%8E%9F)重置、恢复和还原

包括了三个名称相似的命令：`git reset`、`git restore` 和 `git revert`。

- [git-revert[1]](https://git-scm.com/docs/git-revert/zh_HANS-CN) 是关于进行新的提交，以还原其他提交所做更改的命令。
    
- [git-restore[1]](https://git-scm.com/docs/git-restore/zh_HANS-CN) 是有关从索引或其他提交中恢复工作区中文件的命令。这个命令不会更新你的分支。该命令也可以用来从另一个提交中恢复索引中的文件。
    
- [git-reset[1]](https://git-scm.com/docs/git-reset/zh_HANS-CN) 是有关更新分支、移动提示的命令，以便从分支中添加或删除提交。这个操作会修改提交历史。
    
    `git reset` 也可以用来恢复索引，这个功能 `git restore` 的功能有些重合。
    

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E5%BA%95%E5%B1%82%E6%A0%B8%E5%BF%83%E5%91%BD%E4%BB%A4%E7%AE%A1%E4%BB%B6)底层核心命令（管件）

尽管 Git 包含了它自己的上层封装命令，但它的底层核心命令足以支持替代上层封装命令的开发。 上层封装命令开发者可以从阅读 [git-update-index[1]](https://git-scm.com/docs/git-update-index/zh_HANS-CN) 和 [git-read-tree[1]](https://git-scm.com/docs/git-read-tree/zh_HANS-CN) 开始。

这些底层命令的界面（输入、输出、选项集和语义）要比上层封装命令稳定得多，因为这些命令主要是用于脚本的使用。 另一方面，为了改善终端用户的体验，上层封装命令接口是可以改变的。

下面的描述将底层核心命令分为操作对象（在仓库、索引和工作目录树中）的命令、审阅和比较对象的命令，以及在仓库之间移动对象和引用的命令。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%8E%A7%E5%88%B6%E5%91%BD%E4%BB%A4)控制命令

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-applyzhHANS-CNgit-apply1a)[git-apply[1]](https://git-scm.com/docs/git-apply/zh_HANS-CN)

对文件和/或索引打补丁。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-checkout-indexzhHANS-CNgit-checkout-index1a)[git-checkout-index[1]](https://git-scm.com/docs/git-checkout-index/zh_HANS-CN)

从索引中复制文件到工作目录。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-commit-graphzhHANS-CNgit-commit-graph1a)[git-commit-graph[1]](https://git-scm.com/docs/git-commit-graph/zh_HANS-CN)

编写并验证 Git commit-graph 文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-commit-treezhHANS-CNgit-commit-tree1a)[git-commit-tree[1]](https://git-scm.com/docs/git-commit-tree/zh_HANS-CN)

创建一个新的提交对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-hash-objectzhHANS-CNgit-hash-object1a)[git-hash-object[1]](https://git-scm.com/docs/git-hash-object/zh_HANS-CN)

计算对象 ID，根据文件创建 blob（可选）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-index-packzhHANS-CNgit-index-pack1a)[git-index-pack[1]](https://git-scm.com/docs/git-index-pack/zh_HANS-CN)

为现有的打包档案建立打包索引文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-merge-filezhHANS-CNgit-merge-file1a)[git-merge-file[1]](https://git-scm.com/docs/git-merge-file/zh_HANS-CN)

运行一个三向文件合并。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-merge-indexzhHANS-CNgit-merge-index1a)[git-merge-index[1]](https://git-scm.com/docs/git-merge-index/zh_HANS-CN)

为需要合并的文件运行合并程序。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-mktagzhHANS-CNgit-mktag1a)[git-mktag[1]](https://git-scm.com/docs/git-mktag/zh_HANS-CN)

创建一个具有额外验证功能的标签对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-mktreezhHANS-CNgit-mktree1a)[git-mktree[1]](https://git-scm.com/docs/git-mktree/zh_HANS-CN)

根据 ls-tree 格式的文本构建目录树对象（tree-object）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-multi-pack-indexzhHANS-CNgit-multi-pack-index1a)[git-multi-pack-index[1]](https://git-scm.com/docs/git-multi-pack-index/zh_HANS-CN)

编写和验证多包索引。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-pack-objectszhHANS-CNgit-pack-objects1a)[git-pack-objects[1]](https://git-scm.com/docs/git-pack-objects/zh_HANS-CN)

创建一个打包的对象档案。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-prune-packedzhHANS-CNgit-prune-packed1a)[git-prune-packed[1]](https://git-scm.com/docs/git-prune-packed/zh_HANS-CN)

删除已经存在于包文件中的额外对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-read-treezhHANS-CNgit-read-tree1a)[git-read-tree[1]](https://git-scm.com/docs/git-read-tree/zh_HANS-CN)

将目录树的信息读入索引。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-symbolic-refzhHANS-CNgit-symbolic-ref1a)[git-symbolic-ref[1]](https://git-scm.com/docs/git-symbolic-ref/zh_HANS-CN)

读取、修改和删除符号引用。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-unpack-objectszhHANS-CNgit-unpack-objects1a)[git-unpack-objects[1]](https://git-scm.com/docs/git-unpack-objects/zh_HANS-CN)

从打包的档案中解压对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-update-indexzhHANS-CNgit-update-index1a)[git-update-index[1]](https://git-scm.com/docs/git-update-index/zh_HANS-CN)

将工作区中的文件内容登记到索引中。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-update-refzhHANS-CNgit-update-ref1a)[git-update-ref[1]](https://git-scm.com/docs/git-update-ref/zh_HANS-CN)

安全地更新存储在引用中的对象名称。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-write-treezhHANS-CNgit-write-tree1a)[git-write-tree[1]](https://git-scm.com/docs/git-write-tree/zh_HANS-CN)

从当前索引创建一个树对象。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_%E5%AE%A1%E9%98%85%E5%91%BD%E4%BB%A4)审阅命令

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-cat-filezhHANS-CNgit-cat-file1a)[git-cat-file[1]](https://git-scm.com/docs/git-cat-file/zh_HANS-CN)

为仓库对象提供内容或类型和大小信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-cherryzhHANS-CNgit-cherry1a)[git-cherry[1]](https://git-scm.com/docs/git-cherry/zh_HANS-CN)

查找尚未应用于上游仓库的提交。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-diff-fileszhHANS-CNgit-diff-files1a)[git-diff-files[1]](https://git-scm.com/docs/git-diff-files/zh_HANS-CN)

比较工作区和索引中的文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-diff-indexzhHANS-CNgit-diff-index1a)[git-diff-index[1]](https://git-scm.com/docs/git-diff-index/zh_HANS-CN)

将目录树与工作目录或索引进行比较。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-diff-treezhHANS-CNgit-diff-tree1a)[git-diff-tree[1]](https://git-scm.com/docs/git-diff-tree/zh_HANS-CN)

比较两个树对象中发现的 blob 的内容和模式。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-for-each-refzhHANS-CNgit-for-each-ref1a)[git-for-each-ref[1]](https://git-scm.com/docs/git-for-each-ref/zh_HANS-CN)

输出每个引用的信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-for-each-repozhHANS-CNgit-for-each-repo1a)[git-for-each-repo[1]](https://git-scm.com/docs/git-for-each-repo/zh_HANS-CN)

在一个仓库列表上运行一个 Git 命令。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-get-tar-commit-idzhHANS-CNgit-get-tar-commit-id1a)[git-get-tar-commit-id[1]](https://git-scm.com/docs/git-get-tar-commit-id/zh_HANS-CN)

从使用 git-archive 创建的存档中提取提交 ID。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-ls-fileszhHANS-CNgit-ls-files1a)[git-ls-files[1]](https://git-scm.com/docs/git-ls-files/zh_HANS-CN)

显示索引和工作区中的文件信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-ls-remotezhHANS-CNgit-ls-remote1a)[git-ls-remote[1]](https://git-scm.com/docs/git-ls-remote/zh_HANS-CN)

列出远程仓库中的引用。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-ls-treezhHANS-CNgit-ls-tree1a)[git-ls-tree[1]](https://git-scm.com/docs/git-ls-tree/zh_HANS-CN)

列出一个树对象的内容。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-merge-basezhHANS-CNgit-merge-base1a)[git-merge-base[1]](https://git-scm.com/docs/git-merge-base/zh_HANS-CN)

尽可能找到好的共同祖先进行合并。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-name-revzhHANS-CNgit-name-rev1a)[git-name-rev[1]](https://git-scm.com/docs/git-name-rev/zh_HANS-CN)

为给定的 revs 寻找符号名称。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-pack-redundantzhHANS-CNgit-pack-redundant1a)[git-pack-redundant[1]](https://git-scm.com/docs/git-pack-redundant/zh_HANS-CN)

寻找多余的包文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-rev-listzhHANS-CNgit-rev-list1a)[git-rev-list[1]](https://git-scm.com/docs/git-rev-list/zh_HANS-CN)

按时间顺序倒序列出提交对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-rev-parsezhHANS-CNgit-rev-parse1a)[git-rev-parse[1]](https://git-scm.com/docs/git-rev-parse/zh_HANS-CN)

挑选并调整参数。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-show-indexzhHANS-CNgit-show-index1a)[git-show-index[1]](https://git-scm.com/docs/git-show-index/zh_HANS-CN)

显示打包的档案索引。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-show-refzhHANS-CNgit-show-ref1a)[git-show-ref[1]](https://git-scm.com/docs/git-show-ref/zh_HANS-CN)

列出本地仓库中的引用。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-unpack-filezhHANS-CNgit-unpack-file1a)[git-unpack-file[1]](https://git-scm.com/docs/git-unpack-file/zh_HANS-CN)

创建包含 blob 内容的临时文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-varzhHANS-CNgit-var1a)[git-var[1]](https://git-scm.com/docs/git-var/zh_HANS-CN)

显示 Git 逻辑变量。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-verify-packzhHANS-CNgit-verify-pack1a)[git-verify-pack[1]](https://git-scm.com/docs/git-verify-pack/zh_HANS-CN)

验证打包的 Git 存档文件。

一般来说，审阅命令不接触工作区中的文件。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_%E5%90%8C%E6%AD%A5%E4%BB%93%E5%BA%93)同步仓库

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-daemonzhHANS-CNgit-daemon1a)[git-daemon[1]](https://git-scm.com/docs/git-daemon/zh_HANS-CN)

一个非常简单的Git存储库服务器。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-fetch-packzhHANS-CNgit-fetch-pack1a)[git-fetch-pack[1]](https://git-scm.com/docs/git-fetch-pack/zh_HANS-CN)

从另一个仓库接收丢失的对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-http-backendzhHANS-CNgit-http-backend1a)[git-http-backend[1]](https://git-scm.com/docs/git-http-backend/zh_HANS-CN)

通过 HTTP 实现 Git 的服务器端功能。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-send-packzhHANS-CNgit-send-pack1a)[git-send-pack[1]](https://git-scm.com/docs/git-send-pack/zh_HANS-CN)

通过Git协议将对象推送到另一个存储库。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-update-server-infozhHANS-CNgit-update-server-info1a)[git-update-server-info[1]](https://git-scm.com/docs/git-update-server-info/zh_HANS-CN)

更新辅助信息文件以帮助哑服务器。

以下是上面使用的辅助命令，终端用户一般不会直接使用这些命令。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-http-fetchzhHANS-CNgit-http-fetch1a)[git-http-fetch[1]](https://git-scm.com/docs/git-http-fetch/zh_HANS-CN)

通过HTTP从远程Git存储库下载。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-http-pushzhHANS-CNgit-http-push1a)[git-http-push[1]](https://git-scm.com/docs/git-http-push/zh_HANS-CN)

通过HTTP/DAV将对象推送到另一个存储库。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-receive-packzhHANS-CNgit-receive-pack1a)[git-receive-pack[1]](https://git-scm.com/docs/git-receive-pack/zh_HANS-CN)

接收推送到仓库的内容。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-shellzhHANS-CNgit-shell1a)[git-shell[1]](https://git-scm.com/docs/git-shell/zh_HANS-CN)

Git-only SSH 通道使用的限制登录脚本。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-upload-archivezhHANS-CNgit-upload-archive1a)[git-upload-archive[1]](https://git-scm.com/docs/git-upload-archive/zh_HANS-CN)

将归档文件发送回git-archive。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-upload-packzhHANS-CNgit-upload-pack1a)[git-upload-pack[1]](https://git-scm.com/docs/git-upload-pack/zh_HANS-CN)

将打包的对象发送回git-fetch-pack。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_%E5%86%85%E9%83%A8%E8%BE%85%E5%8A%A9%E5%91%BD%E4%BB%A4)内部辅助命令

这些是其他命令使用的内部辅助命令；终端用户一般不会直接使用这些命令。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-check-attrzhHANS-CNgit-check-attr1a)[git-check-attr[1]](https://git-scm.com/docs/git-check-attr/zh_HANS-CN)

显示属性信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-check-ignorezhHANS-CNgit-check-ignore1a)[git-check-ignore[1]](https://git-scm.com/docs/git-check-ignore/zh_HANS-CN)

调试 gitignore / 排除文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-check-mailmapzhHANS-CNgit-check-mailmap1a)[git-check-mailmap[1]](https://git-scm.com/docs/git-check-mailmap/zh_HANS-CN)

显示联系人的规范名称和电子邮件地址。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-check-ref-formatzhHANS-CNgit-check-ref-format1a)[git-check-ref-format[1]](https://git-scm.com/docs/git-check-ref-format/zh_HANS-CN)

确保一个引用名称是否规范。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-columnzhHANS-CNgit-column1a)[git-column[1]](https://git-scm.com/docs/git-column/zh_HANS-CN)

以列显示数据。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-credentialzhHANS-CNgit-credential1a)[git-credential[1]](https://git-scm.com/docs/git-credential/zh_HANS-CN)

检索和存储用户凭证。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-credential-cachezhHANS-CNgit-credential-cache1a)[git-credential-cache[1]](https://git-scm.com/docs/git-credential-cache/zh_HANS-CN)

用于在内存中临时存储密码的助手。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-credential-storezhHANS-CNgit-credential-store1a)[git-credential-store[1]](https://git-scm.com/docs/git-credential-store/zh_HANS-CN)

帮助在磁盘上存储凭证。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-fmt-merge-msgzhHANS-CNgit-fmt-merge-msg1a)[git-fmt-merge-msg[1]](https://git-scm.com/docs/git-fmt-merge-msg/zh_HANS-CN)

产生一个合并提交信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-hookzhHANS-CNgit-hook1a)[git-hook[1]](https://git-scm.com/docs/git-hook/zh_HANS-CN)

运行 git hooks。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-interpret-trailerszhHANS-CNgit-interpret-trailers1a)[git-interpret-trailers[1]](https://git-scm.com/docs/git-interpret-trailers/zh_HANS-CN)

在提交信息中添加或解析结构化信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-mailinfozhHANS-CNgit-mailinfo1a)[git-mailinfo[1]](https://git-scm.com/docs/git-mailinfo/zh_HANS-CN)

从单一的电子邮件中提取补丁和作者。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-mailsplitzhHANS-CNgit-mailsplit1a)[git-mailsplit[1]](https://git-scm.com/docs/git-mailsplit/zh_HANS-CN)

简单的 UNIX mbox 拆分器程序。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-merge-one-filezhHANS-CNgit-merge-one-file1a)[git-merge-one-file[1]](https://git-scm.com/docs/git-merge-one-file/zh_HANS-CN)

与 git-merge-index 一起使用的标准帮助程序。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-patch-idzhHANS-CNgit-patch-id1a)[git-patch-id[1]](https://git-scm.com/docs/git-patch-id/zh_HANS-CN)

为补丁计算唯一的主键 ID。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-sh-i18nzhHANS-CNgit-sh-i18n1a)[git-sh-i18n[1]](https://git-scm.com/docs/git-sh-i18n/zh_HANS-CN)

Git 的 i18n (国际化)安装代码- shell 脚本.

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-sh-setupzhHANS-CNgit-sh-setup1a)[git-sh-setup[1]](https://git-scm.com/docs/git-sh-setup/zh_HANS-CN)

通用 Git shell 脚本安装代码。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgit-stripspacezhHANS-CNgit-stripspace1a)[git-stripspace[1]](https://git-scm.com/docs/git-stripspace/zh_HANS-CN)

删除不必要的空白。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%8C%87%E5%8D%97)指南

以下文档页是关于 Git 概念的指南。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitcore-tutorialzhHANS-CNgitcore-tutorial7a)[gitcore-tutorial[7]](https://git-scm.com/docs/gitcore-tutorial/zh_HANS-CN)

面向开发者的 Git 核心教程。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitcredentialszhHANS-CNgitcredentials7a)[gitcredentials[7]](https://git-scm.com/docs/gitcredentials/zh_HANS-CN)

向 Git 提供用户名和密码。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitcvs-migrationzhHANS-CNgitcvs-migration7a)[gitcvs-migration[7]](https://git-scm.com/docs/gitcvs-migration/zh_HANS-CN)

为 CVS 用户提供的 Git。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitdiffcorezhHANS-CNgitdiffcore7a)[gitdiffcore[7]](https://git-scm.com/docs/gitdiffcore/zh_HANS-CN)

调整差异输出。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgiteverydayzhHANS-CNgiteveryday7a)[giteveryday[7]](https://git-scm.com/docs/giteveryday/zh_HANS-CN)

一套有用的最小化 Git 命令集。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitfaqzhHANS-CNgitfaq7a)[gitfaq[7]](https://git-scm.com/docs/gitfaq/zh_HANS-CN)

关于使用 Git 的常见问题。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitglossaryzhHANS-CNgitglossary7a)[gitglossary[7]](https://git-scm.com/docs/gitglossary/zh_HANS-CN)

Git 术语表。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitnamespaceszhHANS-CNgitnamespaces7a)[gitnamespaces[7]](https://git-scm.com/docs/gitnamespaces/zh_HANS-CN)

Git 命名空间。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitremote-helperszhHANS-CNgitremote-helpers7a)[gitremote-helpers[7]](https://git-scm.com/docs/gitremote-helpers/zh_HANS-CN)

与远程仓库互动的辅助程序。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitsubmoduleszhHANS-CNgitsubmodules7a)[gitsubmodules[7]](https://git-scm.com/docs/gitsubmodules/zh_HANS-CN)

将一个仓库挂载在另一个仓库内。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgittutorialzhHANS-CNgittutorial7a)[gittutorial[7]](https://git-scm.com/docs/gittutorial/zh_HANS-CN)

Git 入门教程。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgittutorial-2zhHANS-CNgittutorial-27a)[gittutorial-2[7]](https://git-scm.com/docs/gittutorial-2/zh_HANS-CN)

Git 的教程介绍：第二部分。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitworkflowszhHANS-CNgitworkflows7a)[gitworkflows[7]](https://git-scm.com/docs/gitworkflows/zh_HANS-CN)

推荐使用 Git 的工作流程概述。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E4%BB%93%E5%BA%93%E5%91%BD%E4%BB%A4%E5%92%8C%E6%96%87%E4%BB%B6%E6%8E%A5%E5%8F%A3)仓库、命令和文件接口

本文档讨论了用户希望直接与之交互的仓库和命令接口。有关标准的更多细节，请参阅 [git-help[1]](https://git-scm.com/docs/git-help/zh_HANS-CN) 中的 `--user-formats`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitattributeszhHANS-CNgitattributes5a)[gitattributes[5]](https://git-scm.com/docs/gitattributes/zh_HANS-CN)

按路径定义属性。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitclizhHANS-CNgitcli7a)[gitcli[7]](https://git-scm.com/docs/gitcli/zh_HANS-CN)

Git 命令行界面和约定。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgithookszhHANS-CNgithooks5a)[githooks[5]](https://git-scm.com/docs/githooks/zh_HANS-CN)

Git 使用的钩子。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitignorezhHANS-CNgitignore5a)[gitignore[5]](https://git-scm.com/docs/gitignore/zh_HANS-CN)

指定有意忽略的未跟踪文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitmailmapzhHANS-CNgitmailmap5a)[gitmailmap[5]](https://git-scm.com/docs/gitmailmap/zh_HANS-CN)

地图作者/修订者姓名和/或电子邮件地址。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitmoduleszhHANS-CNgitmodules5a)[gitmodules[5]](https://git-scm.com/docs/gitmodules/zh_HANS-CN)

定义子模块属性。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitrepository-layoutzhHANS-CNgitrepository-layout5a)[gitrepository-layout[5]](https://git-scm.com/docs/gitrepository-layout/zh_HANS-CN)

Git 仓库布局。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitrevisionszhHANS-CNgitrevisions7a)[gitrevisions[7]](https://git-scm.com/docs/gitrevisions/zh_HANS-CN)

为 Git 指定修订版本和范围。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F%E5%8D%8F%E8%AE%AE%E5%92%8C%E5%85%B6%E4%BB%96%E5%BC%80%E5%8F%91%E8%80%85%E6%8E%A5%E5%8F%A3)文件格式、协议和其他开发者接口

本文档讨论了文件格式、线上协议和其他 git 开发者接口。参阅 [git-help[1]](https://git-scm.com/docs/git-help/zh_HANS-CN) 中的 `--developer-interfaces` 。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitformat-bundlezhHANS-CNgitformat-bundle5a)[gitformat-bundle[5]](https://git-scm.com/docs/gitformat-bundle/zh_HANS-CN)

捆绑文件格式。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitformat-chunkzhHANS-CNgitformat-chunk5a)[gitformat-chunk[5]](https://git-scm.com/docs/gitformat-chunk/zh_HANS-CN)

基于块的文件格式。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitformat-commit-graphzhHANS-CNgitformat-commit-graph5a)[gitformat-commit-graph[5]](https://git-scm.com/docs/gitformat-commit-graph/zh_HANS-CN)

Git 提交图格式。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitformat-indexzhHANS-CNgitformat-index5a)[gitformat-index[5]](https://git-scm.com/docs/gitformat-index/zh_HANS-CN)

Git 索引格式。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitformat-packzhHANS-CNgitformat-pack5a)[gitformat-pack[5]](https://git-scm.com/docs/gitformat-pack/zh_HANS-CN)

Git 包格式。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitformat-signaturezhHANS-CNgitformat-signature5a)[gitformat-signature[5]](https://git-scm.com/docs/gitformat-signature/zh_HANS-CN)

Git 加密签名格式。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitprotocol-capabilitieszhHANS-CNgitprotocol-capabilities5a)[gitprotocol-capabilities[5]](https://git-scm.com/docs/gitprotocol-capabilities/zh_HANS-CN)

协议 v0 和 v1 功能。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitprotocol-commonzhHANS-CNgitprotocol-common5a)[gitprotocol-common[5]](https://git-scm.com/docs/gitprotocol-common/zh_HANS-CN)

各种协议的共同点。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitprotocol-httpzhHANS-CNgitprotocol-http5a)[gitprotocol-http[5]](https://git-scm.com/docs/gitprotocol-http/zh_HANS-CN)

Git 基于 HTTP 的协议。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitprotocol-packzhHANS-CNgitprotocol-pack5a)[gitprotocol-pack[5]](https://git-scm.com/docs/gitprotocol-pack/zh_HANS-CN)

数据包如何通过网络传输。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ahrefdocsgitprotocol-v2zhHANS-CNgitprotocol-v25a)[gitprotocol-v2[5]](https://git-scm.com/docs/gitprotocol-v2/zh_HANS-CN)

Git Wire 协议第 2 版。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E9%85%8D%E7%BD%AE%E6%9C%BA%E5%88%B6)配置机制

Git 使用一种简单的文本格式来存储每个仓库和每个用户的定制内容。 这样的配置文件可能是像这样：

#
# 符号 '#' 或是 ';' 表示本行是注释信息
#

; 核心变量
[core]
	; 不信任文件模式
	filemode = false

; 用户身份信息
[user]
	name = "Junio C Hamano"
	email = "gitster@pobox.com"

各种命令从配置文件中读取并相应地调整其操作。有关配置机制的列表和更多细节，请参阅 [git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN)。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%A0%87%E8%AF%86%E7%AC%A6%E6%9C%AF%E8%AF%AD)标识符术语

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltgt)<对象>

表示任何类型的对象的名称。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltblobgt)<blob>

表示一个数据对象的名称。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltgt-1)<目录树>

表示一个树对象名称。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltgt-1-1)<提交>

表示一个提交对象名称。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-lttree-ishgt)<tree-ish>

表示一个树、提交或标签对象的名称。 一个需要 <树对象> 参数的命令最终会对 <树> 对象进行操作，但会自动解除对指向 <树> 对象的 <提交> 和 <标签> 对象的引用。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltgt-1-1-1)<提交号>

表示一个提交或标记对象的名称。 一个需要 <提交号> 参数的命令最终要对 <提交> 对象进行操作，但会自动解除对指向 <提交> 的 <标签> 对象的引用。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltgt-1-1-1-1)<类型>

表示需要一个对象类型。 目前是其中之一：`blob`、`tree`、`commit` 或 `tag`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltgt-1-1-1-1-1)<文件>

表示一个文件名——几乎总是相对于 `GIT_INDEX_FILE` 描述的树状结构的根部文件。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E7%AC%A6%E5%8F%B7%E6%A0%87%E8%AF%86%E7%AC%A6)符号标识符

任何接受任意 <对象> 的 Git 命令也可以使用下面的符号表示法：

[](https://git-scm.com/docs/git/zh_HANS-CN#git-HEAD)HEAD

表示当前分支。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltgt-1-1-1-1-1-1)<标签>

一个有效的标签 ‘名称’（即一个 `refs/tags/<标签>` 引用）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltheadgt)<head>

一个有效的分支 ‘名称’（即一个 `refs/heads/<head>` 引用）。

更完整的对象名称列表，请参阅 [gitrevisions[7]](https://git-scm.com/docs/gitrevisions/zh_HANS-CN) 中的 “校正说明” 部分。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84)文件/目录结构

请参阅 [gitrepository-layout[5]](https://git-scm.com/docs/gitrepository-layout/zh_HANS-CN) 文档。

阅读 [githooks[5]](https://git-scm.com/docs/githooks/zh_HANS-CN) 了解关于每个钩子的更多细节。

更高级别的源代码管理工具(SCM)可以在 `$GIT_DIR ` 中提供和管理额外的信息。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%9C%AF%E8%AF%AD)术语

请参阅 [gitglossary[7]](https://git-scm.com/docs/gitglossary/zh_HANS-CN)。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)环境变量

各种 Git 命令都会关注环境变量并改变自身的行为。 以 “布尔值” 为标记的环境变量，其取值方式与布尔值配置变量相同，例如，"true"、"yes"、"on" 和正数都被视为 "yes"。

常规变量：

### [](https://git-scm.com/docs/git/zh_HANS-CN#_git_%E4%BB%93%E5%BA%93)Git 仓库

这些环境变量适用于 Git 的 ‘所有’ 核心命令。需要注意的是，如果使用位于 Git 之上的版本控制系统（SCMS），这些环境变量可能会被使用或覆盖，所以在使用非 Git 原生的前端界面时要特别小心。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITINDEXFILEcode)`GIT_INDEX_FILE`

这个环境变量指定了一个备用的索引文件。如果没有指定，则默认使用 `$GIT_DIR/index`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITINDEXVERSIONcode)`GIT_INDEX_VERSION`

这个环境变量指定在写出索引文件时使用什么版本。 它不会影响现有的索引文件。 默认情况下，索引文件的版本为 2 或 3。参见 [git-update-index[1]](https://git-scm.com/docs/git-update-index/zh_HANS-CN) 获取更多信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITOBJECTDIRECTORYcode)`GIT_OBJECT_DIRECTORY`

如果对象存储目录是通过这个环境变量指定的，那么 sha1 目录将在该目录下创建 ， 否则将使用默认的 `$GIT_DIR/objects` 目录。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITALTERNATEOBJECTDIRECTORIEScode)`GIT_ALTERNATE_OBJECT_DIRECTORIES`

由于 Git 对象的不可改变性，旧的对象可以被归档到共享的只读目录中。这个变量指定了一个由 ":" 分隔（在 Windows 下为 ";" 分隔）的 Git 对象目录列表，可以用来搜索 Git 对象。新的对象将不会写入这些目录中。

以 `"`（双引号）开头的条目将被解释为 C 语言风格字符串路径，去掉前导和后导的双引号，并支持反斜杠转义。例如，`"path-with-\"-and-:-in-it":vanilla-path` 有两个路径：`path-with-"-and-:-in-it` 和 `vanilla-path`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITDIRcode)`GIT_DIR`

如果设置了 `GIT_DIR` 环境变量，那么它会指定一个路径来代替默认的 `.git` 作为仓库的基础目录。 `--git-dir` 命令行选项也能够设置这个值。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITWORKTREEcode)`GIT_WORK_TREE`

设置到工作区的根路径。 这也可以由 `--work-tree` 命令行选项和 core.worktree 配置变量设置。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITNAMESPACEcode)`GIT_NAMESPACE`

设置 Git 命名空间；详见 [gitnamespaces[7]](https://git-scm.com/docs/gitnamespaces/zh_HANS-CN)。 `--namespace` 命令行选项也可以设置这个值。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITCEILINGDIRECTORIEScode)`GIT_CEILING_DIRECTORIES`

这个变量值应该是一个用冒号分隔的绝对路径列表。 如果设置了这个变量，它是一个 Git 在寻找仓库目录时不会检查的目录列表（对于排除缓慢加载的网络目录很有用）。 它不会排除当前工作目录或在命令行或环境中设置的 GIT_DIR。 通常情况下，Git 需要读取这个列表中的条目，并解析任何可能存在的符号链接，以便将它们与当前目录进行比较。 然而，如果连这个访问都很慢，你可以在列表中添加一个空条目，告诉 Git 后面的条目不是符号链接，不需要解析；例如，`GIT_CEILING_DIRECTORIES=/maybe/symlink::/very/slow/non/symlink`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITDISCOVERYACROSSFILESYSTEMcode)`GIT_DISCOVERY_ACROSS_FILESYSTEM`

当在一个没有 ".git " 仓库目录的目录下运行时，Git 会尝试在父目录中找到这样的目录来寻找工作区的顶端，但默认情况下它不会跨越文件系统的边界。 这个布尔环境变量可以被设置为 "true"，以告诉 Git 不要在文件系统边界处停止。 和 `GIT_CEILING_DIRECTORIES` 一样，这不会影响通过 `GIT_DIR` 或在命令行中设置的明确的仓库目录。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITCOMMONDIRcode)`GIT_COMMON_DIR`

如果这个变量被设置为一个路径，通常在 $GIT_DIR 中的非工作区文件将被从这个路径中取出。工作区中特定的文件，如 HEAD 或 index，则从 $GIT_DIR 取出。详情见 [gitrepository-layout[5]](https://git-scm.com/docs/gitrepository-layout/zh_HANS-CN) 和 [git-worktree[1]](https://git-scm.com/docs/git-worktree/zh_HANS-CN)。这个变量的优先级低于其他路径变量，如 GIT_INDEX_FILE、GIT_OBJECT_DIRECTORY…​

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITDEFAULTHASHcode)`GIT_DEFAULT_HASH`

如果设置了该变量，新仓库的默认哈希算法将设置为该值。克隆时将忽略此值，始终使用远程仓库的设置。默认值为 "sha1"。 参见 [git-init[1]](https://git-scm.com/docs/git-init/zh_HANS-CN) 中的 `--object-format`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITDEFAULTREFFORMATcode)`GIT_DEFAULT_REF_FORMAT`

如果设置了该变量，新仓库的默认引用后端格式将设置为该值。默认为 "files"。参见 [git-init[1]](https://git-scm.com/docs/git-init/zh_HANS-CN) 中的 `--ref-format`。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_git_%E6%8F%90%E4%BA%A4)Git 提交

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITAUTHORNAMEcode)`GIT_AUTHOR_NAME`

在创建提交、标签对象或在编写日志时，作者身份中使用的可读名称。这会覆盖 `user.name` 和 `author.name` 配置的值。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITAUTHOREMAILcode)`GIT_AUTHOR_EMAIL`

在创建提交、标签对象或在编写引用日志时，用以表明作者身份所使用的电子邮件地址。这会覆盖 `user.email` 和 `author.email` 的配置值。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITAUTHORDATEcode)`GIT_AUTHOR_DATE`

在创建提交、标签对象或在编写引用日志时，用于标注作者修改的日期。有效格式见 [git-commit[1]](https://git-scm.com/docs/git-commit/zh_HANS-CN)。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITCOMMITTERNAMEcode)`GIT_COMMITTER_NAME`

在创建提交、标签对象或在编写引用日志时，提交者身份中使用的可读名称。这会覆盖 `user.name` 和 `committer.name` 的配置值。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITCOMMITTEREMAILcode)`GIT_COMMITTER_EMAIL`

在创建提交、标签对象或在编写引用日志时，用以表明提交者所使用的电子邮件地址。这会覆盖 `user.email` 和 `committer.email` 的配置值。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITCOMMITTERDATEcode)`GIT_COMMITTER_DATE`

在创建提交、标签对象或在编写 reflogs 时，用于标注提交者所作修改的日期。有效格式见 [git-commit[1]](https://git-scm.com/docs/git-commit/zh_HANS-CN)。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeEMAILcode)`EMAIL`

如果没有设置其他相关的环境变量或配置设置，作者和提交者的电子邮件地址会参考该值。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_git_%E5%B7%AE%E5%BC%82%E6%AF%94%E8%BE%83diffs)Git 差异比较（Diffs）

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITDIFFOPTScode)`GIT_DIFF_OPTS`

唯一有效的设置是 "--unified=??" 或 "-u??"，用于设置创建统一的差异时显示的文本行数。 这优先于 Git diff 命令行中传递的任何 "-U" 或 "--unified" 选项值。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITEXTERNALDIFFcode)`GIT_EXTERNAL_DIFF`

当环境变量 `GIT_EXTERNAL_DIFF` 被设置时，由它命名的程序被调用来生成差异文本，而不使用 Git 内置的差异比较工具。 对于一个被添加、删除或修改的操作， `GIT_EXTERNAL_DIFF` 会被调用，一共接收 7 个参数：

path old-file old-hex old-mode new-file new-hex new-mode

使用：

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltoldnewgt-file)<old|new>-file

是 GIT_EXTERNAL_DIFF 用来读取 <old|new> 内容的文件，

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltoldnewgt-hex)<old|new>-hex

是 40 个十六进制的 SHA-1 哈希值，

[](https://git-scm.com/docs/git/zh_HANS-CN#git-ltoldnewgt-mode)<old|new>-mode

是文件模式的八进制表示法。

文件参数可以指向用户的工作文件（例如 "git-diff-files" 中的 `new-file`），`/dev/null`（例如在添加新文件时的 `old-file`），或一个临时文件（例如索引中的 `old-file`）。 无需担心 `GIT_EXTERNAL_DIFF` 对临时文件的链接是否解除——当 `GIT_EXTERNAL_DIFF` 退出时，工具对临时文件的链接会被删除。

`GIT_EXTERNAL_DIFF` 被调用时，对于未合并的路径可通过 <path> 进行设置。

在调用每个路径 `GIT_EXTERNAL_DIFF` 时，都会设置两个环境变量 `GIT_DIFF_PATH_COUNTER` 和 `GIT_DIFF_PATH_TOTAL`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITDIFFPATHCOUNTERcode)`GIT_DIFF_PATH_COUNTER`

以 1 为单位的计数器，每条路径递增 1。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITDIFFPATHTOTALcode)`GIT_DIFF_PATH_TOTAL`

路径总数。

### [](https://git-scm.com/docs/git/zh_HANS-CN#_%E5%85%B6%E4%BB%96)其他

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITMERGEVERBOSITYcode)`GIT_MERGE_VERBOSITY`

一个用于控制递归合并策略输出量的数字。 会覆盖 merge.verbosity 设置。 参见 [git-merge[1]](https://git-scm.com/docs/git-merge/zh_HANS-CN)

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITPAGERcode)`GIT_PAGER`

该环境变量优先于 `$PAGER`。如果设置为空字符串或 "cat"，Git 将不会启动分页器。 参见 [git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN) 中的 `core.pager` 选项。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITPROGRESSDELAYcode)`GIT_PROGRESS_DELAY`

一个数字，用于控制延迟多少秒后才显示可选的进度指示器。默认为 2。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITEDITORcode)`GIT_EDITOR`

该环境变量覆盖 `$EDITOR` 和 `$VISUAL`。 有几条 Git 命令在交互模式下启动编辑器时会用到它。参见 [git-var[1]](https://git-scm.com/docs/git-var/zh_HANS-CN) 和 [git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN) 中的 `core.editor` 选项。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITSEQUENCEEDITORcode)`GIT_SEQUENCE_EDITOR`

在编辑交互式变基的待办事项列表时，该环境变量会覆盖配置的 Git 编辑器。参见 [git-rebase[1]](https://git-scm.com/docs/git-rebase/zh_HANS-CN) 和 [git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN) 中的 `sequence.editor` 选项。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITSSHcode)`GIT_SSH`

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITSSHCOMMANDcode)`GIT_SSH_COMMAND`

如果设置了这两个环境变量中的任何一个，_git fetch_ 和 _git push_ 在连接远程系统时就会使用指定的命令而不是 _ssh_。 传递给配置命令的命令行参数由 ssh 变量决定。 详见 [git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN) 中的 `ssh.variant` 选项。

`$GIT_SSH_COMMAND` 优先于 `$GIT_SSH`，由 shell 解释，允许包含额外参数。 另一方面，`$GIT_SSH` 必须是一个程序的路径（如果程序需要额外参数，可以是一个 shell 脚本）。

通常，通过个人的 `.ssh/config` 文件配置任何所需的选项会更容易。 详情请查阅 ssh 文档。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITSSHVARIANTcode)`GIT_SSH_VARIANT`

如果设置了这个环境变量，Git 会自动检测 `GIT_SSH`/`GIT_SSH_COMMAND`/`core.sshCommand` 是否指向 OpenSSH、plink 或 tortoiseplink。该变量覆盖了具有相同作用的配置设置 `ssh.variant`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITSSLNOVERIFYcode)`GIT_SSL_NO_VERIFY`

将此环境变量设置并导出为任意值，Git 就不会在通过 HTTPS 获取或推送数据时验证 SSL 证书。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITATTRSOURCEcode)`GIT_ATTR_SOURCE`

设置读取 gitattributes 的树状对象。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITASKPASScode)`GIT_ASKPASS`

如果设置了这个环境变量，那么需要获取密码或口令的 Git 命令（例如用于 HTTP 或 IMAP 身份验证）就会调用这个程序，并将合适的提示符作为命令行参数，然后从其 STDOUT 中读取密码。参见 [git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN) 中的 `core.askPass` 选项。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTERMINALPROMPTcode)`GIT_TERMINAL_PROMPT`

如果将此布尔环境变量设为 false，git 将不会在终端上提示（例如，要求 HTTP 认证时）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITCONFIGGLOBALcode)`GIT_CONFIG_GLOBAL`

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITCONFIGSYSTEMcode)`GIT_CONFIG_SYSTEM`

从给定文件中获取配置，而不是从全局或系统级配置文件中获取。如果设置了 `GIT_CONFIG_SYSTEM`，则不会读取构建时定义的系统配置文件（通常是 `/etc/gitconfig`）。同样，如果设置了 `GIT_CONFIG_GLOBAL`，则既不会读取 `$HOME/.gitconfig` 也不会读取 `$XDG_CONFIG_HOME/git/config`。可以设置为 `/dev/null`，以跳过读取相应级别的配置文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITCONFIGNOSYSTEMcode)`GIT_CONFIG_NOSYSTEM`

是否跳过从系统范围内的 `$(prefix)/etc/gitconfig` 文件读取设置。 这个布尔环境变量可以与 `$HOME` 和 `$XDG_CONFIG_HOME` 一起使用，为挑剔的脚本创建一个可预测的环境，也可以设为 true 以暂时避免使用有错误的 `/etc/gitconfig` 文件，等待有足够权限的人来修复它。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITFLUSHcode)`GIT_FLUSH`

如果该布尔环境变量设置为 true，那么诸如 _git blame_（增量模式）、_git rev-list_、_git log_、_git check-attr_ 和 _git check-ignore_ 等命令将会在每条记录被刷新后强制刷新输出流。强制刷新输出流。如果该变量设置为 false，这些命令的输出将使用完全缓冲的 I/O 方式进行使用完全缓冲的 I/O 输出。 如果不设置这个环境变量不设置，Git 将根据标准输出流是否重定向到文件，选择缓冲或面向记录的刷新方式根据标准输出流是否被重定向到文件。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACEcode)`GIT_TRACE`

启用一般跟踪信息，如别名扩展、内置命令执行和外部命令执行。

如果该变量设置为 "1"、"2" 或 "true"（比较不区分大小写），跟踪信息将被打印到标准错误流。

如果变量设置为大于 2 小于 10（严格意义上）的整数值，那么 Git 会将该值解释为一个打开的文件描述符，并尝试将跟踪信息写入该文件描述符。

另外，如果变量设置为绝对路径（以 _/_ 字符开头），Git 会将其理解为文件路径，并尝试将跟踪信息附加到该路径上。

取消对变量的设置，或将其设置为空、"0" 或 "false"（不区分大小写），将禁用跟踪信息。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACEFSMONITORcode)`GIT_TRACE_FSMONITOR`

启用文件系统监视器扩展的跟踪信息。 有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACEPACKACCESScode)`GIT_TRACE_PACK_ACCESS`

启用对所有数据包访问的跟踪信息。每次访问都会记录包文件名和包中的偏移量。这可能有助于排除一些与包相关的性能问题。 有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACEPACKETcode)`GIT_TRACE_PACKET`

启用对进出指定程序的所有数据包的跟踪信息。这有助于调试对象协商或其他协议问题。以 "PACK" 开头的数据包会关闭跟踪（但请参阅下文的 `GIT_TRACE_PACKFILE`）。 有关可用的跟踪输出选项，请参见 `GIT_TRACE`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACEPACKFILEcode)`GIT_TRACE_PACKFILE`

可跟踪指定程序发送或接收的打包文件。与其他跟踪输出不同，这种跟踪是逐字的：没有标题，也不引用二进制数据。你几乎肯定希望将其直接存入一个文件（例如 `GIT_TRACE_PACKFILE=/tmp/my.pack`），而不是显示在终端上或与其他跟踪输出混在一起。

请注意，目前仅在克隆和获取的客户端实施了这一功能。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACEPERFORMANCEcode)`GIT_TRACE_PERFORMANCE`

启用与性能相关的跟踪信息，例如每条 Git 命令的总执行时间。 有关可用的跟踪输出选项，请参见 `GIT_TRACE`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACEREFScode)`GIT_TRACE_REFS`

启用引用数据库操作的跟踪信息。 有关可用的跟踪输出选项，请参见 `GIT_TRACE`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACESETUPcode)`GIT_TRACE_SETUP`

在 Git 完成设置阶段后，启用打印 .git、工作树和当前工作目录的跟踪信息。 有关可用的跟踪输出选项，请参见 `GIT_TRACE`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACESHALLOWcode)`GIT_TRACE_SHALLOW`

启用可帮助调试获取/克隆浅仓库的跟踪信息。 有关可用的跟踪输出选项，请参见 `GIT_TRACE`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACECURLcode)`GIT_TRACE_CURL`

启用对 git 传输协议所有传入和传出数据（包括描述性信息）的 curl 完整跟踪转储。 这类似于在命令行上执行 curl `--trace-ascii`。 有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACECURLNODATAcode)`GIT_TRACE_CURL_NO_DATA`

启用 curl 跟踪时（见上文 `GIT_TRACE_CURL`），不要转储数据（即只转储信息行和头文件）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACE2code)`GIT_TRACE2`

启用来自 "trace2" 库的更详细的跟踪信息。 `GIT_TRACE2` 的输出是一种简单的文本格式，可读性高。

如果该变量设置为 "1"、"2" 或 "true"（比较不区分大小写），跟踪信息将被打印到标准错误流。

如果变量设置为大于 2 小于 10（严格意义上）的整数值，那么 Git 会将该值解释为一个打开的文件描述符，并尝试将跟踪信息写入该文件描述符。

另外，如果变量设置为绝对路径（以 _/_ 字符开头），Git 会将其理解为文件路径，并尝试将跟踪信息附加到该路径上。 如果路径已经存在且是一个目录，跟踪信息将被写入该目录下的文件（每个进程一个），文件名根据 SID 的最后一个分量和一个可选的计数器命名（以避免文件名冲突）。

此外，如果变量设置为 `af_unix:[<套接字类型>:]<绝对文件路径>`，Git 将尝试以 Unix 域套接字的方式打开路径。 套接字类型可以是 `stream` 或 `dgram`。

取消对变量的设置，或将其设置为空、"0" 或 "false"（不区分大小写），将禁用跟踪信息。

详见 [Trace2 文档](https://git-scm.com/docs/api-trace2/zh_HANS-CN)。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACE2EVENTcode)`GIT_TRACE2_EVENT`

此设置会写入适合机器解释的基于 JSON 的格式。 有关可用的跟踪输出选项，请参阅 `GIT_TRACE2`，以及 [Trace2 文档](https://git-scm.com/docs/api-trace2/zh_HANS-CN) 了解更多详情。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACE2PERFcode)`GIT_TRACE2_PERF`

除了 `GIT_TRACE2` 中可用的基于文本的信息外，此设置还可写入基于列的格式，以了解嵌套区域。 有关可用的跟踪输出选项，请参阅 `GIT_TRACE2`，以及 [Trace2 文档](https://git-scm.com/docs/api-trace2/zh_HANS-CN) 了解更多详情。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITTRACEREDACTcode)`GIT_TRACE_REDACT`

默认情况下，启动跟踪后，Git 会编辑 cookie、"Authorization:"（授权：）标头、"Proxy-Authorization:"（代理授权：）标头和 packfile URI 的值。将此布尔环境变量设为 false，可阻止这种编辑。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITLITERALPATHSPECScode)`GIT_LITERAL_PATHSPECS`

将这个布尔环境变量设为 true 会导致 Git 按字面意思而非通配符模式来处理所有路径规范。例如，运行 `GIT_LITERAL_PATHSPECS=1 git log -- '*.c'` 将搜索涉及路径 `*.c` 的提交，而不是任何与通配符 `*.c` 匹配的路径。如果你正在向 Git 输入字面路径（例如，之前由 `git ls-tree`、`--raw` 差异输出等提供的路径），你可能需要这样做。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITGLOBPATHSPECScode)`GIT_GLOB_PATHSPECS`

将这个布尔环境变量设置为 true 会导致 Git 将所有路径规范视为通配符模式（又称 "glob" 魔法）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITNOGLOBPATHSPECScode)`GIT_NOGLOB_PATHSPECS`

将这个布尔环境变量设置为 true，Git 就会将所有路径规范都视为字面意思（又称 "literal"（字面）魔法）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITICASEPATHSPECScode)`GIT_ICASE_PATHSPECS`

将此布尔环境变量设置为 true 会导致 Git 将所有路径规范视为不区分大小写。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITREFLOGACTIONcode)`GIT_REFLOG_ACTION`

更新引用时，会创建引用日志条目来记录更新引用的原因（通常是更新引用的高级命令的名称），以及引用的新旧值。 脚本化的上层命令可以使用 `git-sh-setup` 中的 set_reflog_action 辅助函数，在被最终用户作为顶级命令调用时将其名称设置为该变量，并记录在引用日志的正文中。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITREFPARANOIAcode)`GIT_REF_PARANOIA`

如果将此布尔环境变量设置为 false，则在遍历引用列表时会忽略已损坏或命名不正确的引用。通常情况下，Git 会尝试包含任何此类引用，这可能会导致某些操作失败。这通常是可取的，因为潜在的破坏性操作（例如 [git-prune[1]](https://git-scm.com/docs/git-prune/zh_HANS-CN)）最好是中止，而不是忽略破损的引用（从而认为它们指向的历史不值得保存）。默认值是 `1`（即偏执地检测并终止所有操作）。通常不需要将其设置为 `0`，但在试图从损坏的版本库中挽救数据时可能会有用。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITCOMMITGRAPHPARANOIAcode)`GIT_COMMIT_GRAPH_PARANOIA`

从提交图加载提交对象时，Git 会对对象数据库中的对象执行存在性检查。这样做是为了避免出现陈旧的提交图（其中包含对已删除提交的引用），但同时也会影响性能。

默认值为 "false"，即禁用上述行为。将其设置为 "true" 可启用存在性检查，这样就不会从提交图中返回过时的提交，但这也会影响性能。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITALLOWPROTOCOLcode)`GIT_ALLOW_PROTOCOL`

如果设置为以冒号分隔的协议列表，其行为就好像 `protocol.allow` 被设置为 `never` 而每个列出的协议都被 `protocol.<名称>.allow` 设置为 `always`（覆盖任何现有配置）。详见 [git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN) 中关于 `protocol.allow` 的描述。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITPROTOCOLFROMUSERcode)`GIT_PROTOCOL_FROM_USER`

将此布尔环境变量设为 false，以防止 fetch/push/clone 使用配置为 `user` 状态的协议。 这对于限制从不可信任的仓库或向 git 命令提供可能不可信任的 URLS 的程序递归初始化子模块非常有用。 详见 [git-config[1]](https://git-scm.com/docs/git-config/zh_HANS-CN)。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITPROTOCOLcode)`GIT_PROTOCOL`

仅供内部使用。 用于有线协议握手。 包含一个用冒号 _:_ 分隔的键列表，可选值为 _key[=value]_。 必须忽略未知键和值的存在。

请注意，可能需要对服务器进行配置，以允许该变量通过某些传输方式传递。在访问本地仓库（即 `file://` 或文件系统路径）以及通过 `git://` 协议时，它会自动传播。对于 git-over-http，它在大多数配置中都会自动工作，但请参阅 [git-http-backend[1]](https://git-scm.com/docs/git-http-backend/zh_HANS-CN) 中的讨论。对于 git-over-ssh，可能需要配置 ssh 服务器以允许客户端传递此变量（例如，在 OpenSSH 中使用 `AcceptEnv GIT_PROTOCOL`）。

此配置为可选配置。如果不传播该变量，客户端将退回到原始的 "v0" 协议（但可能会错过某些性能改进或功能）。该变量目前只影响克隆和获取，尚未用于推送（但将来可能会）。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITOPTIONALLOCKScode)`GIT_OPTIONAL_LOCKS`

如果把这个布尔环境变量设置为 false，Git 将在不执行任何需要加锁的可选子操作的情况下完成任何请求的操作。 例如，这将防止 `git status` 刷新索引的副作用。这对后台运行的进程非常有用，因为它们不希望与仓库中的其他操作造成锁争用。 默认值为 `1`。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITREDIRECTSTDINcode)`GIT_REDIRECT_STDIN`

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITREDIRECTSTDOUTcode)`GIT_REDIRECT_STDOUT`

[](https://git-scm.com/docs/git/zh_HANS-CN#git-codeGITREDIRECTSTDERRcode)`GIT_REDIRECT_STDERR`

仅限 Windows：允许将标准输入/输出/错误句柄重定向到环境变量指定的路径。这在多线程应用程序中特别有用，因为通过 `CreateProcess()` 传递标准句柄的标准方式并不可行，因为这需要将句柄标记为可继承（因此 **每个** 生成的进程都会继承它们，可能会阻塞常规的 Git 操作）。主要的预期用例是使用命名管道进行通信（例如 `\\.\pipe\my-git-stdin-123`）。

支持两个特殊值： 如果 `GIT_REDIRECT_STDERR` 是 `2>&1` ，标准错误将重定向到与标准输出相同的句柄。

[](https://git-scm.com/docs/git/zh_HANS-CN#git-GITPRINTSHA1ELLIPSIS)GIT_PRINT_SHA1_ELLIPSIS（已弃用）

如果设置为 `yes`，会在 SHA-1 值（缩写）后打印省略号。 这会影响分离 HEAD 的指示（[git-checkout[1]）和原始差异输出（linkgit:git-diff[1]](https://git-scm.com/docs/git-checkout[1]%EF%BC%89%E5%92%8C%E5%8E%9F%E5%A7%8B%E5%B7%AE%E5%BC%82%E8%BE%93%E5%87%BA%EF%BC%88linkgit:git-diff/zh_HANS-CN)）。 在上述情况下打印省略号不再被认为是适当的，在可预见的将来，对它的支持可能会被移除（连同变量）。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E8%AE%A8%E8%AE%BA)讨论

有关以下内容的更多详情，请参阅[用户手册中的 Git 概念章节](https://git-scm.com/docs/user-manual/zh_HANS-CN#git-concepts)和 [gitcore-tutorial[7]](https://git-scm.com/docs/gitcore-tutorial/zh_HANS-CN)。

Git 项目通常由一个工作目录和一个位于顶层的 ".git " 子目录组成。 除其他内容外，.git 目录还包含一个压缩对象数据库，代表项目的完整历史；一个 "index" 文件，将历史记录与当前工作区的内容相链接；以及指向历史记录的命名指针，如标签和分支头。

对象数据库包含三种主要类型的对象：blobs，用于保存文件数据；tree，用于指向 blobs 和其他树，以建立目录层次结构；commits，用于引用单个树和一定数量的父提交。

提交相当于其他系统所说的 "changeset" 或 "version"，代表项目历史中的一个步骤，每个父提交代表紧接着的前一个步骤。 有多个父提交的提交代表独立开发线的合并。

所有对象都以其内容的 SHA-1 哈希值命名，通常写成由 40 个十六进制数字组成的字符串。 这些名称具有全局唯一性。 只需对某个提交进行签名，就能证明该提交之前的整个历史。 为此，我们提供了第四种对象类型——tag（标签）。

首次创建时，对象存储在单个文件中，但为了提高效率，以后可能会压缩成 "pack files"。

被称为引用的已命名指针会标记历史中的有趣点。一个引用可以包含一个对象的 SHA-1 名称或另一个引用的名称（后者称为“符号引用”）。名称以 `refs/head/` 开头的引用包含开发中分支的最新提交（或 "head"）的 SHA-1 名称。相关标记的 SHA-1 名称存储在 `refs/tags/` 下。名为 `HEAD` 的符号引用包含当前已签出分支的名称。

索引文件初始化时包含一个所有路径的列表，以及每个路径的一个二进制对象和一组属性。 二进制对象代表当前分支头部的文件内容。 属性（最后修改时间、大小等）取自工作树中的相应文件。 通过比较这些属性，可以发现工作区的后续更改。 索引可根据新内容进行更新，也可根据索引中存储的内容创建新提交。

索引还能为给定路径名存储多个条目（称为 "stages"（阶段））。 在合并过程中，这些阶段用于保存文件的各种未合并版本。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%9B%B4%E5%A4%9A%E6%96%87%E4%BB%B6)更多文件

请参阅 “说明” 部分的参考资料，开始使用 Git。 对于初次使用 Git 的用户来说，以下内容可能过于详细。

[用户手册中的 Git 概念章节](https://git-scm.com/docs/user-manual/zh_HANS-CN#git-concepts)和 [gitcore-tutorial[7]](https://git-scm.com/docs/gitcore-tutorial/zh_HANS-CN) 都介绍了 Git 的底层架构。

请参阅 [gitworkflows[7]](https://git-scm.com/docs/gitworkflows/zh_HANS-CN) 了解推荐的工作流程。

另请参阅 [howto](https://git-scm.com/docs/howto-index/zh_HANS-CN) 文档，了解一些有用的示例。

[Git API 文档](https://git-scm.com/docs/api-index/zh_HANS-CN)中介绍了内部结构。

从 CVS 迁移的用户可能还需要阅读 [gitcvs-migration[7]](https://git-scm.com/docs/gitcvs-migration/zh_HANS-CN)。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E4%BD%9C%E8%80%85)作者

Git 由 Linus Torvalds 发起，目前由 Junio C Hamano 维护。许多贡献来自 Git 邮件列表 <[git@vger.kernel.org](mailto:git@vger.kernel.org)>。http://www.openhub.net/p/git/contributors/summary 提供了更完整的贡献者名单。

如果克隆了 git.git，[git-shortlog[1]](https://git-scm.com/docs/git-shortlog/zh_HANS-CN) 和 [git-blame[1]](https://git-scm.com/docs/git-blame/zh_HANS-CN) 的输出结果可以显示项目特定部分的作者。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E6%8A%A5%E5%91%8A%E7%BC%BA%E9%99%B7)报告缺陷

向 Git 邮件列表 <[git@vger.kernel.org](mailto:git@vger.kernel.org)> 报告错误，开发和维护工作主要在此进行。 您无需订阅该列表即可在此发送信息。 有关以前的错误报告和其他讨论，请参阅 [https://lore.kernel.org/git](https://lore.kernel.org/git) 的列表档案。

与安全相关的问题应在 Git 安全邮件列表 <[git-security@googlegroups.com](mailto:git-security@googlegroups.com)> 中私下披露。

## [](https://git-scm.com/docs/git/zh_HANS-CN#_%E5%8F%82%E8%A7%81)参见

[gittutorial[7]](https://git-scm.com/docs/gittutorial/zh_HANS-CN), [gittutorial-2[7]](https://git-scm.com/docs/gittutorial-2/zh_HANS-CN), [giteveryday[7]](https://git-scm.com/docs/giteveryday/zh_HANS-CN), [gitcvs-migration[7]](https://git-scm.com/docs/gitcvs-migration/zh_HANS-CN), [gitglossary[7]](https://git-scm.com/docs/gitglossary/zh_HANS-CN), [gitcore-tutorial[7]](https://git-scm.com/docs/gitcore-tutorial/zh_HANS-CN), [gitcli[7]](https://git-scm.com/docs/gitcli/zh_HANS-CN), [Git 用户手册](https://git-scm.com/docs/user-manual/zh_HANS-CN), [gitworkflows[7]](https://git-scm.com/docs/gitworkflows/zh_HANS-CN)

## [](https://git-scm.com/docs/git/zh_HANS-CN#_git)GIT

属于 [git[1]](https://git-scm.com/docs/git/zh_HANS-CN) 文档