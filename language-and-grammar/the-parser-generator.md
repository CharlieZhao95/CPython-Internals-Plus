# 5.4 解析器生成器

解析器生成器的工作原理是将 EBNF 语句转换为非确定性有限自动机 (NFA)。NFA 状态和转换被转化并合并为确定性有限自动机 (DFA)。

解析器把 DFAs 用作解析表。这项技术在[斯坦福大学形成](http://infolab.stanford.edu/\~ullman/dragon/slides1.pdf)并在19世纪80年代被开发出来，就在 Python 语言出现不久之前。CPython 的解析器生成器 (`pgen`) 是 CPython 项目独有的。

在 Python 3.8 中，`pgen` 应用的实现由 C 语言改写为 Python 语言，模块路径为：Parser/pgen/pgen.py。

此模块的执行方式如下所示：

<figure><img src="../.gitbook/assets/图5.4.1 pgen执行过程.png" alt=""><figcaption></figcaption></figure>

其通常从构建脚本执行，而不是被直接执行。

DFA 没有一个可视化输出，但[作者从 CPython 拉取了一个分支](https://github.com/tonybaloney/cpython/tree/dot\_pgen)并添加了有向图输出功能。`decorator` 语法在 Grammar/Grammar 中被定义为：

<figure><img src="../.gitbook/assets/图5.4.2 装饰器语法定义.png" alt=""><figcaption></figcaption></figure>

解析器生成器创建出一个包含11个状态的复杂 NFA 图。每一个状态都以数字表示出来（在语法中提示他们的名字）。这些转换被称为“弧”。

由于路径减少，所以 DFA 要比 NFA 更加简单。

<figure><img src="../.gitbook/assets/图5.4.3 装饰器的DFA.png" alt=""><figcaption></figcaption></figure>

NFA 和 DFA 图仅对调试复杂语法的设计有用。

我们将用铁路图取代 DFA 和 NFA 图来展示语法。例如，下图展示的是`decorator` 语句可以采用的路径。

<figure><img src="../.gitbook/assets/图5.4.4 装饰器语句的铁路图.png" alt=""><figcaption></figcaption></figure>