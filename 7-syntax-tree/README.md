# 七、语法树解析

在上一章节中，你探索了如果从各类来源中读取 Python 文本。其需要转换为编译器可以使用的结构，这个过程被称为解析：

<figure><img src="../.gitbook/assets/7.0.1 解析过程.png" alt=""><figcaption></figcaption></figure>

在本章节，你将继续探索文本是如果被解析未一个可以编译的逻辑结构。

CPython 会使用具象语法树和抽象语法树两种结构来解析代码。

<figure><img src="../.gitbook/assets/7.0.2 CPython使用CST和AST来解析代码.png" alt=""><figcaption></figcaption></figure>

解析过程的两个步骤：

1. 使用解析器-词法分析器（Lexer）创建具象语法树；
2. 通过使用解析器从具象语法树创建出抽象语法树。

这两个步骤是许多语言中都会使用到的常见范式。
