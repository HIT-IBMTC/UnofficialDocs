.. sublime: wordWrap false

========
代码片段
========

与Textmate的兼容性
***************************

Sublime Text的代码片段与Textmate的代码片段基本兼容。

文件格式
***********

代码片段文件是有 ``sublime-snippet`` 后缀的XML文件。

.. code-block:: xml

    <snippet>
        <content><![CDATA[]]></content>
        <tabTrigger></tabTrigger>
        <scope></scope>
        <description></description>
    </snippet>

``content``
    实际的代码片段内容。

``tabTrigger``
    与这段代码片段关联的隐式按键绑定。最后一个（隐式的）按键式 ``TAB`` 。

``scope``
    适用于这段代码片段的作用域选择子。

``description``
    为了菜单项准备的用户友好的说明性文字。

转义序列
****************

``\$``
    Literal ``$``.

``\$``
    常量 ``$`` 。

环境变量
*********************

======================      =====================================================================
``$PARAM1 .. $PARAMn``      传递给 ``insertSnippet`` 命令的各个参数。
``$SELECTION``              代码片段被触发的时候选中的文本内容。
``$TM_CURRENT_LINE``        代码片段被触发的时候光标所在行的内容。
``$TM_CURRENT_WORD``        代码片段被触发的时候光标所在的单词。
``$TM_FILENAME``            正在编辑的文件名称，包含文件扩展名。
``$TM_FILEPATH``            正在编辑的文件的文件路径。
``$TM_FULLNAME``            用户的用户名。
``$TM_LINE_INDEX``          插入代码片段的列的位置，位置是从0开始计数的。
``$TM_LINE_NUMBER``         插入代码片段的行的位置，位置是从1开始计数的。
``$TM_SELECTED_TEXT``       与 ``$SELECTION`` 是等价的。
``$TM_SOFT_TABS``           当 ``translateTabsToSpaces`` 选项是真的时候，值是 ``YES`` ，否则为 ``NO`` 。
``$TM_TAB_SIZE``            tab对应的空格数（受 ``tabSize`` 选项的控制）。
======================      =====================================================================

字域
******

标记代码片段中可以通过 ``TAB`` 或 ``SHIFT + TAB`` 循环的各个位置。

语法： ``$1`` .. ``$n``

``$0``
    退出标记。当循环到这个标记的时候，编辑器应该返回至正常的编辑模式。默认情况下，Sublime Text在 ``content``
    元素内容的结尾位置隐式的添加这个标记。

有相同名字的字域互相映射。

占位符
*************

带有默认值的字域。

语法： ``${1:PLACE_HOLDER}`` .. ``${n:PLACE_HOLDER}``

字域和占位符可以遇其他的站位符进行组合和嵌套。

替换
**************

Syntax:
语法：

    - ``${var_name/regex/format_string/}``
    - ``${var_name/regex/format_string/options}``

``var_name``
    替换展开所以来的字域的名字：1，2，3……
``regex``
    Perl风格的正则表达式：关于 `正则表达式 <http://www.boost.org/doc/libs/1_44_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html>`_ ，请参考Boost库的文档。
``format_string``
    参考Boost库文档的 `格式字符串 <http://www.boost.org/doc/libs/1_44_0/libs/regex/doc/html/boost_regex/format/perl_format.html>`_ 内容。
``options``
    可选的。可以选择下面的任何一个：
        ``i``
            忽略大小写敏感的正则。
        ``g``
            替换所有匹配 ``regex`` 的内容。
        ``m``
            在字符串中不要忽略换行符。
