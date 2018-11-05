========
代码片段
========

无论你是在敲代码还是撰写你的大作，你都会一遍一遍地写到一些小的重复片段。使用代码片段
保存这些冗长的部分。是一种很聪明的模板，它会根据你输入的上下文帮助你添加文本。

要新建一个代码片段，选择目录 **Tools | New Snippet…**。Sublime Text会介绍给你一个
框架来新建一个代码片段。

代码片段可以存储在任何一个包文件下，为了在你学习时方便，你可以将其保存在你的
目录 ``Packages/User`` 下。

代码片段文件格式
********************

代码片段一般保存在Sublime Text的包里。这里使用了扩展 ``sublime-snippet`` 简化XML的
文件。比如，你可以在包 ``Email`` 里保存 ``greeting.sublime-snippet`` 。

典型的代码片段的结构如下（包括Sublime Text为方便你使用添加的默认提示）：

.. code-block:: xml

    <snippet>
        <content><![CDATA[Type your snippet here]]></content>
        <!-- Optional: Tab trigger to activate the snippet -->
        <tabTrigger>xyzzy</tabTrigger>
        <!-- Optional: Scope the tab trigger will be active in -->
        <scope>source.python</scope>
        <!-- Optional: Description to show in the menu -->
        <description>My Fancy Snippet</description>
    </snippet>

元素 ``snippet`` 包括Sublime Text需要的所有信息，以获取需要添加什么，是否添加以及
什么情况下。接下来我们看一下每一个部分。

``content``
    实际的代码片段。代码片段可以包括从最简单到相当复杂的模板。接下来，我们会看到
    它们的例子。

    在编写你自己的代码片段时，你要记住以下内容：

        - 如果你想输入 ``$`` ，你需要按照方式如下转义：``\$``。

        - 如果代码片段包括缩进，一般使用tab。选项 ``translateTabsToSpaces`` 为 ``true`` 时，
          代码片段添加的tab会自动转换成空格。

        标签 ``content`` 必须包含在块 ``<![CDATA[…]]>`` 中。如果不这么写，代码片段
        可不好用哦~

``tabTrigger``
    定义你在需要添加代码片段时按下的按键序列。在你按下按键序列后，再按下键盘上的 `Tab` 键，代码片段才会生效。

    tab触发器是一个隐式的按键绑定。

``scope``
    作用域选择子决定了代码片段在哪个范围内生效。
    详情请看 :ref:`scopes-and-scope-selectors` 。

``description``
    在代码片段目录显示代码片段时使用。如果没显示，Sublime Text默认使用代码片段名。

根据之前的信息，现在可以开始按照下面的章节写自己的代码片段了。

.. note::
注释：
    为了简洁，除非另外说明，这里的例子中只包含 ``content`` 元素。

代码片段特性
****************

环境变量
---------------------

代码片段根据环境变量获得上下文信息。Sublime Text会自动设定下面所述的变量值。

你也可以加上自己的变量来提供额外的信息。这些自定义变量都在文件 ``.sublime-options``
中定义。

======================    ====================================================================================
**$PARAM1, $PARAM2…**      传递给 ``insert_snippet`` 命令的各个参数。
**$SELECTION**             代码片段被触发的时候选中的文本内容。
**$TM_CURRENT_LINE**       代码片段被触发的时候光标所在行的内容。
**$TM_CURRENT_WORD**       代码片段被触发的时候光标所在的单词。
**$TM_FILENAME**           正在编辑的文件名称，包含文件扩展名。
**$TM_FILEPATH**           正在编辑的文件的文件路径。
**$TM_FULLNAME**           用户的用户名。
**$TM_LINE_INDEX**         插入代码片段的列的位置，位置是从0开始计数的。
**$TM_LINE_NUMBER**        插入代码片段的行的位置，位置是从1开始计数的。
**$TM_SELECTED_TEXT**      与 ``$SELECTION`` 是等价的。
**$TM_SOFT_TABS**          当 ``translateTabsToSpaces`` 选项是真的时候，值是 ``YES`` ，否则为 ``NO`` 。
**$TM_TAB_SIZE**           tab对应的空格数（受 ``tabSize`` 选项的控制）。
======================    ====================================================================================

接下来，我们看一个使用变量的简单的例子：

.. code-block:: perl

    ====================================
    USER NAME:          $TM_FULLNAME
    FILE NAME:          $TM_FILENAME
     TAB SIZE:          $TM_TAB_SIZE
    SOFT TABS:          $TM_SOFT_TABS
    ====================================

    # Output:
    ====================================
    USER NAME:          guillermo
    FILE NAME:          test.txt
     TAB SIZE:          4
    SOFT TABS:          YES
    ====================================


字域
------

有了字域标记，你可以通过在代码片段中的某一位置按下键盘的 `Tab` 键来循环。一旦添加
了代码片段，字域可以通过自定义的信息帮助你走查。

.. code-block:: perl

    First Name: $1
    Second Name: $2
    Address: $3

上面的例子中，当你按下一次键盘 `Tab` 键时，光标会跳转到 ``$1`` 。当你连续按下 `Tab` 两次是，会跳转
到 ``$2`` 等等。你也可以按下键盘的 `Shift+Tab` 键后退。如果你在最高制表位按下 `Tab` 键，Sublime Text
会将光标停留在代码片段内容的末尾，以便你可以重新开始编辑。

如果你想控制退出点的位置，你可以使用 ``$0`` 标记。

你可以通过按下键盘的 `Esc` 键跳出字域循环。

镜像字域
---------------

相同的字域标记会相互映射：当你输入了第一个，剩下的立刻填充相同的值。

.. code-block:: perl

    First Name: $1
    Second Name: $2
    Address: $3
    User name: $1

这个例子中，"User name"会填充"First Name"的值。

占位符
-------------

通过扩展一些字域的语法，你可以为每一个域设定默认值。如果在你的代码片段里需要设定
一个一般的情况，你又希望不失去自定义的便捷，占位符是非常有用的。

.. code-block:: perl

    First Name: ${1:Guillermo}
    Second Name: ${2:López}
    Address: ${3:Main Street 1234}
    User name: $1

变量也可以用作占位符：

.. code-block:: perl

    First Name: ${1:Guillermo}
    Second Name: ${2:López}
    Address: ${3:Main Street 1234}
    User name: ${4:$TM_FULLNAME}

你也可以在其他的占位符里嵌套占位符：

.. code-block:: perl

    Test: ${1:Nested ${2:Placeholder}}

替换
-------------

.. WARNING::
    这部分是一个草稿，可能会有不准确的内容。

除了占位符语法，制表符的设置可以使用替换设定更多复杂的操作。使用替换可以根据映射的制表符
设置动态地生成文本。

替换的语法如下：

    - ``${var_name/regex/format_string/}``
    - ``${var_name/regex/format_string/options}``

**var_name**
    变量名：1, 2, 3…

**regex**
    Perl风格的正则表达式：关于 `正则表达式 <http://www.boost.org/doc/libs/1_44_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html>`_ ，请参考Boost库的文档。

**format_string**
    参考Boost库文档的 `格式字符串 <http://www.boost.org/doc/libs/1_44_0/libs/regex/doc/html/boost_regex/format/perl_format.html>`_ 内容。

**options**
    可选的。可以选择下面的任何一个：
        **i**
            忽略大小写敏感的正则。
        **g**
            替换所有匹配 ``regex`` 的内容。
        **m**
            在字符串中不要忽略换行符。

有了替换，比如，你可以如此简单地添加文本下划线：

.. code-block:: perl

          Original: ${1:Hey, Joe!}
    Transformation: ${1/./=/g}

    # Output:

          Original: Hey, Joe!
    Transformation: =========
