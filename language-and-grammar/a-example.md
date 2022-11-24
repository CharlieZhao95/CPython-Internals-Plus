# 5.6 一个更复杂的示例

添加 `proceed` 作为一个备选关键词是一个简单的修改，解析器生成器会将`proceed` 作为 `pass_stmt` 语句的一个单词符号。没有对编译器做任何修改就可以让这个关键字生效。

实际上，大多数的语法修改都要比这个例子复杂的多。

Python 3.8 通过定义 `:=` 格式引入了赋值表达式。赋值表达式既对命名变量赋值又返回命名变量的值。

`if` 语句是 Python 语言添加赋值表达式而受影响的语句之一。

在 3.8 之前，`if` 语句被定义为：

* `if` 关键字后跟着一个 `test` 以及一个 `:`；
* 嵌套着的一系列语句（`suite`）；
* 0 或多个 `if` 语句，后面跟着一个 `test` ，一个`:` 以及一个 `suite` ；
* 一个可选的 `else` 语句，后面跟着一个 `:` 以及一个 `suite` 。

在语法文件中，if 语句被定义为：

<figure><img src="../.gitbook/assets/图5.6.1 if语句定义.png" alt=""><figcaption></figcaption></figure>

用铁路图来展示：

<figure><img src="../.gitbook/assets/图5.6.2 if语句定义铁路图.png" alt=""><figcaption></figcaption></figure>

为了支持这个赋值表达式，这个修改动作必须要做到前向兼容。因此，在 `if` 语句中的`:=` 用法是可选的。

在 `if` 语句中被使用的 `test` 单词符号在众多语句中都是通用的。比如，`assert` 语句跟着一个 `test` 单词符号（并且第二个 `test` 是可选的）。

<figure><img src="../.gitbook/assets/图5.6.3 assert表达式定义.png" alt=""><figcaption></figcaption></figure>

Python 3.8 添加了另一个备选的 `test` 单词符号类型，以便语法能规定哪些语句能支持赋值表达式而哪些不能支持赋值表达式。这个语句被称为 `namedexpr_test` ，其在语法中的定义如下图所示：

<figure><img src="../.gitbook/assets/图5.6.4 namedexpr表达式定义.png" alt=""><figcaption></figcaption></figure>

或者用铁路图来展示，则如下图所示：

<figure><img src="../.gitbook/assets/图5.6.5 namedexpr表达式定义铁路图.png" alt=""><figcaption></figcaption></figure>

`if` 语句的新语法已经通过将 test 替换为 namedexpr\_test 进行了修改。

<figure><img src="../.gitbook/assets/图5.6.6 修改后的if语句定义.png" alt=""><figcaption></figcaption></figure>

用铁路图来展示如下图所示：

<figure><img src="../.gitbook/assets/图5.6.7 修改后的if语句定义铁路图.png" alt=""><figcaption></figcaption></figure>

为了将 `:=` 和已经存在的 `COLON(:)` 和 `EQUAL(=)` 单词符号区分开，`Grammar/Tokens`  添加了下面的单词符号：

<figure><img src="../.gitbook/assets/图5.6.8 赋值语句单词符号.png" alt=""><figcaption></figcaption></figure>

为了支持复制表达式，不仅仅是做了这点修改。这个特性的引入更改了 CPython 编译器的很多部分，详情参见 [Pull Request](https://github.com/python/cpython/pull/10497)。

{% hint style="info" %}
**See Also**

如果你想了解更多 CPython 解析器生成器方面的内容，在2019 年 PyCon 欧洲会议上，`pgen` 的作者录制了一个关于实现和设计的演示文稿，详情参见 “[野兽的灵魂](https://www.youtube.com/watch?v=1\_23AVsiQEc)” 主题演讲。
{% endhint %}
