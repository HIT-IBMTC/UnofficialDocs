语法定义
==================

语法定义使Sublime Text能够识别编程和标记语言。最值得注意的是，它们与颜色一起使用以提供语法突出显示。语法定义定义将缓冲区中的文本划分为命名区域的范围。Sublime Text中的几个编辑功能广泛使用了这种细粒度的上下文信息。

本质上，语法定义包括用于查找文本的正则表达式，以及或多或少任意的点分隔字符串，称为 *scopes* 或 *scope names*。对于给定正则表达式的每次出现，Sublime Text都会为匹配的文本提供其对应的 *scope name*。

先决条件
*************

为了学习本教程，您需要安装 PackageDev_，这是一个旨在简化Sublime Text新语法定义创建的软件包。按照README文件“Getting Started”部分中的安装说明进行操作。

.. _PackageDev: https://github.com/SublimeText/PackageDev

文件格式
***********

Sublime uses `property list`_ files (Plist) to store syntax definitions. Because
editing XML files is a cumbersome task, though, we'll be using JSON_ instead and
converting it to Plist afterwards. This is where the AAAPackageDev package mentioned
above comes in.
Sublime Text使用 属性列表_（Plist）文件来存储语法定义。但是，因为编辑XML文件是一项繁琐的任务，所以我们将使用 YAML_，然后将其转换为Plist格式。这是PackageDev包（如上所述）的用武之地。

.. _属性列表: http://en.wikipedia.org/wiki/Property_list
.. _YAML: http://en.wikipedia.org/wiki/YAML

.. note::
    如果您在本教程中遇到意外错误，则可能是PackageDev或YAML的责任。不要立即认为您的问题是由于Sublime Text中的错误造成的。

无论如何，如果您更喜欢使用XML，请手动编辑Plist文件，但始终牢记它们对转义序列，许多XML标记等的不同需求。

.. _scopes-and-scope-selectors:

Scopes
******

Scopes是Sublime Text中的关键概念。实质上，它们在缓冲区中被命名为文本区域。他们自己不做任何事情，但Sublime Text在需要上下文信息时偷看他们。

例如，当您触发代码片段时，Sublime Text会检查绑定到代码片段的scope并查看插入符号在文件中的位置。如果插入符号的当前位置与代码片段的scope选择器匹配，则Sublime Text会将其触发。否则，没有任何反应。

.. sidebar:: Scopes与Scope选择器

  *scope* 和 *scope选择器* 之间存在细微差别：scope是语法定义中定义的名称，而scope选择器用于代码片段和目标scope的键绑定等项目。在创建新的语法定义时，您关心scope;如果要将代码片段限制到某个scope，可以使用scope选择器。

Scopes可以嵌套以允许高度粒度。您可以像使用CSS选择器一样向下钻取层次结构。例如，由于scope选择器，您可以仅在Python源代码中的单引号字符串中激活键绑定，但不能在任何其他语言的单引号字符串中激活。

Sublime Text继承了Textmate的scope思想，Textmate是Mac的文本编辑器。`Textmate的在线手册`_包含有关scope选择器的更多信息，这些信息对Sublime Text用户也很有用。特别是，颜色方案广泛使用scope来设置所需颜色的语言的每个方面。

.. _`Textmate的在线手册`: http://manual.macromates.com/en/


语法定义的工作原理
***************************

语法定义的核心是与范围名称配对的正则表达式数组。Sublime Text将尝试将这些模式与缓冲区的文本进行匹配，并将相应的范围名称附加到所有实例。这些正则表达式和范围名称对称为 *规则*。

.. XXX: What are those exceptions mentioned below?

规则按顺序应用，一次一行。每个规则都会使用匹配的文本区域，因此将从下一个规则的匹配尝试中排除（除少数例外）。实际上，这意味着在创建新的语法定义时，应该注意从更具体的规则转到更一般的规则。否则，贪婪的正则表达式可能会吞下您希望以不同方式设置样式的部分。

可以组合来自单独文件的语法定义，也可以递归地应用它们。

你的第一个语法定义
****************************

举例来说，让我们为Sublime Text代码片段创建一个语法定义。我们将设计实际的代码片段内容，而不是 ``.sublime-snippet`` 文件。

.. note::
  由于语法定义主要用于启用语法突出显示，因此我们将使用样式将源代码文件分解为范围。 但请记住，颜色与语法定义不同，除了语法高亮之外，范围还有更多用途。

这些是我们想要在代码段中设置样式的元素：

    - 变量 (``$PARAM1``, ``$USER_NAME``\ …)
    - 简单字段 (``$0``, ``$1``\ …)
    - 带有占位符的复杂字段 (``${1:Hello}``)
    - 嵌套字段 (``${1:Hello ${2:World}!}``)
    - 转义序列 (``\\$``, ``\\<``\ …)
    - 非法序列 (``$``, ``<``\ …)

.. note::
    在继续之前，请确保已安装AAAPackageDev软件包，如上所述。

创建新的语法定义
--------------------------------

要创建新的语法定义，请按照下列步骤操作：

  - 前往 **Tools | Packages | Package Development | New Syntax Definition**
  - 将新文件作为 ``.JSON-tmLanguage`` 文件保存到你的 ``Packages/User`` 文件夹中。

您现在应该看到这样的文件::

  { "name": "Syntax Name",
    "scopeName": "source.syntax_name",
    "fileTypes": [""],
    "patterns": [
    ],
    "uuid": "ca03e751-04ef-4330-9a6b-9b99aae1c418"
  }

我们现在来看看关键元素。

``uuid``
    位于末尾，这是此语法定义的唯一标识符。每个新的语法定义都有自己的uuid。不要修改它们。

``name``
    Sublime Text将在语法定义下拉列表中显示的名称使用简短的描述性名称。通常，您将使用要为其创建语法定义的编程语言名称。

``scopeName``
    此语法定义的顶级范围。它采用表单 ``source.<lang_name>`` 或 ``text.<lang_name>``。对于编程语言，请使用 ``source``。对于标记和其他一切，``text``。

``fileTypes``
    这是文件扩展名列表。打开这些类型的文件时，Sublime Text将自动为它们激活此语法定义。

``patterns``
    您的模式的容器。

对于我们的示例，请使用以下信息填写模板::

    {   "name": "Sublime Snippet (Raw)",
        "scopeName": "source.ssraw",
        "fileTypes": ["ssraw"],
        "patterns": [
        ],
        "uuid": "ca03e751-04ef-4330-9a6b-9b99aae1c418"
    }

.. note::
    JSON是一种非常严格的格式，因此请确保正确地获取所有逗号和引号。如果转换为Plist失败，请查看输出面板以获取有关错误的更多信息。稍后我们将解释如何将JSON中的语法定义转换为Plist。

分析模式
******************

``patterns`` 数组可以包含几种类型的元素。我们将在以下部分中介绍其中一些内容。如果您想了解有关模式的更多信息，请参阅Textmate的在线手册


.. sidebar:: 语法定义中的正则表达式语法

  Sublime Text在语法定义中使用 Oniguruma_ 的正则表达式语法。一些现有的语法定义使用了这个正则表达式引擎支持的功能，这些功能不属于perl风格的正则表达式，因此需要Oniguruma。

  .. _Oniguruma: http://www.geocities.jp/kosako3/oniguruma/doc/RE.txt

匹配
-------

他们采取这种形式：

.. code-block:: js

    { "match": "[Mm]y \s+[Rr]egex",
      "name": "string.ssraw",
      "comment": "This comment is optional."
    }

``match``
    正则表达式Sublime Text将用于尝试查找匹配项。

``name``
    应用于 ``match`` 事件的scope的名称。

``comment``
    关于此模式的可选注释。

让我们回到我们的例子。看起来像这样：

.. code-block:: js

    { "name": "Sublime Snippet (Raw)",
      "scopeName": "source.ssraw",
      "fileTypes": ["ssraw"],
      "patterns": [
      ],
      "uuid": "ca03e751-04ef-4330-9a6b-9b99aae1c418"
    }

也就是说，确保 ``patterns`` 数组为空。

现在我们可以开始为Sublime代码片段添加规则。让我们从简单的字段开始。这些可以与正则表达式匹配，如下所示：

.. code-block:: perl

    \$[0-9]+
    # or...
    \$\d+

但是，因为我们正在用JSON编写正则表达式，所以我们需要考虑JSON自己的转义规则。因此，我们之前的例子变成：

.. code-block:: js

    \\$\\d+

随着逃避，我们可以像这样构建我们的模式：

.. code-block:: js

    { "match": "\\$\\d+",
      "name": "keyword.source.ssraw",
      "comment": "Tab stops like $1, $2..."
    }

.. sidebar:: 选择正确的范围名称

    命名范围有时并不明显。有关范围名称的指导，请查看Textmate在线手册。如果要实现与现有颜色的最高兼容性，重复使用其中概述的基本类别非常重要。

    颜色中包含硬编码的范围名称。它们不可能包含您能想到的每个范围名称，因此它们的目标是标准的以及一些罕见的。这意味着使用相同语法定义的两种颜色可能会以不同方式呈现文本！

    还要记住，您应该使用最适合您的需求或偏好的范围名称。如果你有充分的理由将一个像 ``constant.numeric`` 这样的范围分配给数字以外的任何东西，那就完全没问了。

我们也可以将它添加到我们的语法定义中：

.. code-block:: js

    {   "name": "Sublime Snippet (Raw)",
        "scopeName": "source.ssraw",
        "fileTypes": ["ssraw"],
        "patterns": [
            { "match": "\\$\\d+",
              "name": "keyword.source.ssraw",
              "comment": "Tab stops like $1, $2..."
            }
        ],
        "uuid": "ca03e751-04ef-4330-9a6b-9b99aae1c418"
    }

我们现在准备将我们的文件转换为 ``.tmLanguage``。出于兼容性原因，语法定义使用Textmate的 ``.tmLanguage`` 扩展。如上所述，它们只是Plist格式的XML文件。

请按照以下步骤执行转换：

    - 在 **Tools | Build System** 中选择 ``Json to tmLanguage``
    - 按下 :kbd:`F7`
    - 将在与 ``.JSON-tmLanguage`` 文件相同的文件夹中为您生成 ``.tmLanguage`` 文件
    - Sublime Text会将更改重新加载到语法定义中

您现在已经创建了第一个语法定义。接下来，打开一个新文件并使用扩展名 ``.ssraw`` 保存。缓冲区的语法名称应自动切换到“Sublime Snippet（Raw）”，如果键入 ``$1`` 或任何其他简单的片段字段，则应该获得语法突出显示。

让我们继续为环境变量创建另一个规则。

.. code-block:: js

    { "match": "\\$[A-Za-z][A-Za-z0-9_]+",
      "name": "keyword.source.ssraw",
      "comment": "Variables like $PARAM1, $TM_SELECTION..."
    }

重复上述步骤以更新 ``.tmLanguage`` 文件并重新启动Sublime Text。

微调匹配
-------------------

您可能已经注意到，例如，``$PARAM1`` 中的整个文本的样式方式相同。根据您的需求或个人喜好，您可能希望 ``$`` 能够脱颖而出。这就是 ``captures`` 的来源。使用捕获，您可以将模式分解为组件以单独定位它们。

让我们重写之前的一个模式来使用 ``captures``：

.. code-block:: js

    { "match": "\\$([A-Za-z][A-Za-z0-9_]+)",
      "name": "keyword.ssraw",
      "captures": {
          "1": { "name": "constant.numeric.ssraw" }
       },
      "comment": "Variables like $PARAM1, $TM_SELECTION..."
    }

捕获会为您的规则带来复杂性，但它们非常简单。请注意数字如何从左到右引用括号组。当然，您可以拥有任意数量的捕获组。

可以说，您希望其他范围在视觉上与此范围保持一致。继续改变它。

始末规则
----------------

到目前为止，我们一直在使用一个简单的规则。虽然我们已经看到如何将模式分解为更小的组件，但有时您会希望将源代码的大部分目标明确区分为开始和结束标记。

用引号和其他分隔结构括起来的文字字符串最好用开始结束规则处理。这是其中一条规则的骨架::

      { "name": "",
        "begin": "",
        "end": ""
      }

好吧，至少在他们最简单的版本中。让我们来看一个包括所有可用选项::

       { "name": "",
         "begin": "",
         "beginCaptures": {
           "0": { "name": "" }
         },
         "end": "",
         "endCaptures": {
           "0": { "name": "" }
         },
         "patterns": [
            {  "name": "",
               "match": ""
                         }
         ],
         "contentName": ""
       }

有些元素可能看起来很熟悉，但它们的组合可能令人生畏。让我们分别看一下。

``begin``
    正则表达式为此范围的开头标记。

``end``
    正则表达式为此范围的结束标记。

``beginCaptures``
    捕获 ``begin`` 标记。它们像捕捉简单匹配一样工作。可选的。

``endCaptures``
    与 ``beginCaptures`` 相同但是结束标记。可选。

``contentName``
    整个匹配区域的范围，从开始标记到结束标记，包括在内。这将有效地为此规则中定义的beginCaptures，endCaptures和模式创建嵌套作用域。可选的。

``patterns``
    仅与开始端内容匹配的模式数组 - 它们与开始或结束消耗的文本不匹配。

我们将使用此规则来设置片段中嵌套的复杂字段的样式::

    { "name": "variable.complex.ssraw",
       "begin": "(\\$)(\\{)([0-9]+):",
       "beginCaptures": {
           "1": { "name": "keyword.ssraw" },
           "3": { "name": "constant.numeric.ssraw" }
       },
       "patterns": [
           { "include": "$self" },
           {  "name": "string.ssraw",
              "match": "."
           }
       ],
       "end": "\\}"
    }

这是我们将在本教程中看到的最复杂的模式。``begin`` 和 ``end`` 键是不言自明的：它们定义了 ``${<NUMBER>：`` 和 ``}`` 之间的区域。``beginCaptures`` 进一步将开始标记划分为较小的范围。

然而，最有趣的部分是 ``patterns``。递归和订购的重要性终于在这里出现了。

我们在上面已经看到，字段可以嵌套。为了解释这一点，我们需要递归地设置嵌套字段的样式。这就是 ``include`` 规则在提供 ``$self`` 值时的作用：它递归地将我们的整个语法定义应用于我们的开始结束规则中包含的文本部分，不包括 ``begin`` 和 ``end`` 消耗的文本。

请记住，匹配的文本已被使用，并在下次匹配尝试时被排除。

为了完成复杂的字段，我们将占位符设置为字符串。由于我们已经在复杂字段中匹配了所有可能的标记，因此我们可以安全地告诉Sublime Text为任何剩余文本（``.``）提供文字字符串范围。

最后的接触
-------------

最后，让我们的样式转义序列和非法序列，并结束。

::

        {  "name": "constant.character.escape.ssraw",
           "match": "\\\\(\\$|\\>|\\<)"
        },

        {  "name": "invalid.ssraw",
           "match": "(\\$|\\<|\\>)"
        }

这里唯一的难点就是获得转义符号的数量。除此之外，如果您熟悉正则表达式，则规则非常简单。

但是，您必须注意将第二个规则放在与 ``$`` 字符匹配的任何其他规则之后，否则您可能无法获得所需的结果。

另请注意，在添加这两个附加规则后，上面的递归始末规则将按预期工作。

最后，这是最终的语法定义::

  {   "name": "Sublime Snippet (Raw)",
      "scopeName": "source.ssraw",
      "fileTypes": ["ssraw"],
      "patterns": [
          { "match": "\\$(\\d+)",
            "name": "keyword.ssraw",
            "captures": {
                "1": { "name": "constant.numeric.ssraw" }
             },
            "comment": "Tab stops like $1, $2..."
          },

          { "match": "\\$([A-Za-z][A-Za-z0-9_]+)",
            "name": "keyword.ssraw",
            "captures": {
                "1": { "name": "constant.numeric.ssraw" }
             },
            "comment": "Variables like $PARAM1, $TM_SELECTION..."
          },

          { "name": "variable.complex.ssraw",
            "begin": "(\\$)(\\{)([0-9]+):",
            "beginCaptures": {
                "1": { "name": "keyword.ssraw" },
                "3": { "name": "constant.numeric.ssraw" }
             },
             "patterns": [
                { "include": "$self" },
                { "name": "string.ssraw",
                  "match": "."
                }
             ],
             "end": "\\}"
          },

          { "name": "constant.character.escape.ssraw",
            "match": "\\\\(\\$|\\>|\\<)"
          },

          { "name": "invalid.ssraw",
            "match": "(\\$|\\>|\\<)"
          }
      ],
      "uuid": "ca03e751-04ef-4330-9a6b-9b99aae1c418"
  }

有更多可用的构造和代码重用技术，但上面的解释应该让您开始创建语法定义。
