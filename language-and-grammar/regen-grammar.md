# 5.5 重新生成语法

我们修改部分 Python 语法来观察 pgen 的实际运行情况。在 `Grammar\Grammar` 文件中搜索 `pass_stmt` 并查看 `pass` 语句的定义：

<figure><img src="../.gitbook/assets/图5.5.1 pass语法.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/图5.5.2 pass铁路图.png" alt=""><figcaption></figcaption></figure>

通过添加一个`|`以及 `proceed` 改变这行定义，让其能接受终端的`paas` 或者`proceed` 作为关键字。

<figure><img src="../.gitbook/assets/图5.5.3 pass修改后的铁路图.png" alt=""><figcaption></figcaption></figure>

接下来，通过运行 `pgen` 来重新构建语法文件。 CPython 附带脚本能自动生成 `pgen` 。

在 macOS 和 Linux 上，可以运行 `make regen-grammar` 来重新生成语法。

<figure><img src="../.gitbook/assets/图5.5.2 重新生成语法 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

在 Windows 上，可以使用 --regen 标志来运行 `PCBuild` 目录中的build.bat。

<figure><img src="../.gitbook/assets/图5.5.5 Windows下重新生成语法.png" alt=""><figcaption></figcaption></figure>

你应该能在输出上看到新的 `include/graminit.h` 和 `Python/graminit.c` 文件已经重新生成。

当你基于重新生成的解析器表，并使用上一章节使用过的编译过程重新编译 CPython，其将使用新的语法。

如果你的代码编译成功，你就可以使用新的 CPython 二进制文件并启动一个交互式解释器。

在交互式解释器中，你可以尝试定义一个函数。用你编译进 Python 语法中的 `proceed` 关键字来替代 `pass` 语句。

<figure><img src="../.gitbook/assets/图5.5.6 使用proceed.png" alt=""><figcaption></figcaption></figure>

恭喜你，你已经修改了 CPython 语法并且编译出属于你自己的 CPython 版本。

接下来，我们会继续探索单词符号以及其与语法的关系。

### 单词符号（Token）

{% hint style="info" %}
**Note**

译者注：“Token” 一词有多种翻译名称，大多数被翻译成 “记号”，但我个人还是赞同《现代编译原理（赵克佳等人译）》中的翻译，将其翻译为 “单词符号” 更加贴切，简称 “单词”。
{% endhint %}

在 `Grammar` 目录中，除了语法文件之外，还有一个 `Grammar/Tokens` 文件，其包含了解析树的叶子节点上能找到的所有类型。每个单词都有一个名字和一个生成的唯一ID。这些名字使其更容易在词法分析器中更容易被引用。

{% hint style="info" %}
**Note**

`Grammar/Tokens` 文件是 Python 3.8 的新特性。
{% endhint %}



比如，左括号 被称为 `LPAR` ，而顿号被称为 `SEMI` 。你会在本书后半部分看到这些单词符号。

<figure><img src="../.gitbook/assets/图5.5.7 单词一.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/图5.5.8 单词二.png" alt=""><figcaption></figcaption></figure>

与语法文件一样，如果你修改了 `Grammar/Token` 文件，你就需要重新运行 `pgen`。

你可以使用 CPython 的 tokenize 模块查看单词符号的运行情况。

{% hint style="info" %}
**Note**

CPython 源码中有两种词法分析器。本章节演示的是用 Python 语言实现的，而另外一种则是用 C 语言实现的。用 Python 语言写的词法分析器是一个实用工具，而 Python 解释器用的则是用 C 语言实现的。它们都有相同的输出和行为。C 语言版本的词法分析器是为了性能而设计，而 Python 版本的词法分析器则是为了调试而设计。
{% endhint %}

创建一个 `test_tokens.py` 的简单 Python 脚本：

<figure><img src="../.gitbook/assets/图5.5.9 示例代码.png" alt=""><figcaption></figcaption></figure>

将 `test_tokens.py` 文件输入到 Python 的 `tokenize` 标准库中，你会看到一系列按行展开的单词和符号。

使用 -e 标志来输出准确的单词符号名称：

<figure><img src="../.gitbook/assets/图5.5.10 示例代码单词1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/图5.5.11 示例代码单词2.png" alt=""><figcaption></figcaption></figure>

在输出中，第一列是行/列坐标范围，第二列是单词符号名称，而最后一列则是单词符号的值。

在输出中，词法分析器模块已经暗示了几个单词符号：

* `ENCODING` 单词符号用于 `utf-8` ;
* 结尾处有一个空行；
* `DEDENT` 用于结束函数申明；
* `ENDMARKER` 用于结束文件。

一个最佳实践是在 Python 源文件末尾中保留一个空行。如果你忘记这样去做，CPython 会为你自动补上。

`tokeinze` 模块是用纯 Python 语言写的，相关代码归档于 `Lib/tokenize.py` 中。

你可以使用 -d 标志来执行 Python 应用来查看 C 语言版本词法分析器的详细输出。使用之前创建的 `test_tokens.py` 脚本，用如下命令执行：

<figure><img src="../.gitbook/assets/图5.5.12 示例代码单词转化1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/图5.5.13 示例代码单词转化2.png" alt=""><figcaption></figcaption></figure>

在输出中，你可以非常明显的看到 `procceed` 是一个关键词。在下一章节中，我们将会看到执行 Python 二进制文件如何进入到词法分析器以及从那里执行代码会发生什么。

{% hint style="info" %}
**Note**

如要你要清理代码，请回退对`Grammar/Grammar` 的修改，重新生成语法文件以及清理构建和重新编译：

对于 macOS 或者 Linux：

$ git checkout -- Grammar/Grammar

$ make regen-grammar

$ make clobber

$ make -j2 -s

或者对于 Windows：

&#x20;\> git checkout -- Grammar/Grammar

&#x20;\> build.bat --regen

&#x20;\> build.bat -t CleanAll

&#x20;\> build.bat -t Build
{% endhint %}
