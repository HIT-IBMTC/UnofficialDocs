.. sublime: wordWrap false

==================
语法定义
==================

.. warning::
    这部分内容仍处于草稿阶段，并可能包含错误信息。

与Textmate的兼容性
***************************

Sublime Text的语法定义内容基本上与Textmate的语法定义文件是相互兼容的。

文件格式
***********

语法定义文件是以 ``tmLanguage`` 作为扩展名的Plist（属性列表）文件。然而，在本文档中，我们将使用
JSON文件来进行讲解，然后再将JSON文件转换为Plist文件。

.. code-block:: js

  { "name": "Sublime Snippet (Raw)",
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
              "1": { "name": "keyword.control.ssraw" },
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

``name``
    所定义语法的描述性名称。这个名称显示在Sublime Text界面右下角的下拉菜单中。通常情况下， ``name`` 表示的是
    编程语言或其他标记语言的名称。

``scopeName``
    语法定义文件起效的顶级坐拥域名称。这个名称一般是 ``source.<语言名称>`` 或者 ``text.<语言名称>`` 。
    如果是在定义编程语言的语法，那么就使用 ``source`` 这个名称，否则使用 ``text`` 作为名称。

``fileTypes``
    记录适用于这个语法文件的文件扩展名的数组。数组中的每项是去掉"."的文件扩展名称。


``uuid``
    这个语法定义的唯一标示符。目前被忽略。

``foldingStartMarker``
    目前被忽略，用于标示折叠内容的开始。

``foldingStopMarker``
    目前被忽略，用于标示折叠内容的结束。

``patterns``
    记录与缓冲区中的文字进行匹配的模式的数组。

``repository``
    从模式元素中抽象出来的数组。定义这个数组一方面有助于保持语法定义文件整洁，另一方面也能为像
    递归模式这样的功能服务。这个内容是可选的。


模式数组
******************

在 ``patterns`` 数组中包含的元素。

``match``
    包含下列原素：

    ============    ==================================================
    ``match``       用于搜索的模式。
    ``name``        设置被 ``match`` 匹配到的内容所具有的作用域。
    ``comment``     可选。只是为了提供信息。
    ``captures``    可选。是对 ``match`` 的精炼。见下文解释
    ============    ==================================================

    ``captures`` 内容可以包含 *多* 组下面要说明的元素对：

    ========      ==================================
    ``0..n``      被索引的组的名字。
    ``name``      组内元素具有的作用域。
    ========      ==================================

    示例：

    .. code-block:: js

        // Simple

        { "name": "constant.character.escape.ssraw",
          "match": "\\\\(\\$|\\>|\\<)"
          "comment". "Sequences like \$, \> and \<"
        }

        // With captures

        { "match": "\\$(\\d+)",
          "name": "keyword.ssraw",
          "captures": {
              "1": { "name": "constant.numeric.ssraw" }
           },
          "comment": "Tab stops like $1, $2..."
        }

``include``
    在仓库中包含其他的语法定义内容或者当前定义的内容。

    References:

        =========       ===========================
        $self           当前的语法定义。
        #itemName       仓库中名为 "itemName" 的内容。
        source.js       外部的语法定义。
        =========       ===========================

    示例：

    .. code-block:: js

        // Requires presence of DoubleQuotedStrings element in the repository.
        { "include": "#DoubleQuotedStrings" }

        // Recursively includes the current syntax definition.
        { "include": "$self" }

        // Includes and external syntax definition.
        { "include": "source.js" }

``begin..end``
    定义一个可以跨多行的作用域。

    包含下面的一些元素：

        =================       ================================================
        ``begin``               模式的开始标志。
        ``end``                 模式的终止标志。
        ``name``                整个区域的作用域名称。
        ``beginCaptures``       为 ``begin`` 准备的 ``captures``。请参考 ``captures``。
        ``endCaptures``         为 ``end`` 准备的 ``captures``。请参考 ``captures``。
        ``patterns``            与正文内容进行配对的 ``patterns`` 。
        ``contentName``         除掉标志符之后的内容的作用域名称。
        =================       ================================================

    示例：

    .. code-block:: js

        { "name": "variable.complex.ssraw",
          "begin": "(\\$)(\\{)([0-9]+):",
          "beginCaptures": {
              "1": { "name": "keyword.control.ssraw" },
              "3": { "name": "constant.numeric.ssraw" }
           },
           "patterns": [
              { "include": "$self" },
              { "name": "string.ssraw",
                "match": "."
              }
           ],
           "end": "\\}"
        }

仓库
**********

在 ``include`` 元素中，既可以从 ``patterns`` 中饮用，也可以从自身引用。请参考 ``include``
章节来了解更多信息。


仓库可以包含下列元素：

  - Simple elements:

    .. code-block:: js

      "elementName": {
        "match":  "some regexp",
        "name":   "some.scope.somelang"
      }

  - Complex elements:

    .. code-block:: js

      "elementName": {
        "patterns": [
          { "match":  "some regexp",
            "name":   "some.scope.somelang"
          },
          { "match":  "other regexp",
            "name":   "some.other.scope.somelang"
          }
        ]
      }

示例：

.. code-block:: js

    "repository": {
      "numericConstant": {
        "patterns": [
          { "match":  "\\d*(?<!\\.)(\\.)\\d+(d)?(mb|kb|gb)?",
            "name":   "constant.numeric.double.powershell",
            "captures": {
              "1": { "name": "support.constant.powershell" },
              "2": { "name": "support.constant.powershell" },
              "3": { "name": "keyword.other.powershell" }
              }
          },
          { "match":  "(?<!\\w)\\d+(d)?(mb|kb|gb)?(?!\\w)",
            "name":   "constant.numeric.powershell",
            "captures": {
              "1": { "name": "support.constant.powershell" },
              "2": { "name": "keyword.other.powershell" }
              }
          }
        ]
      },
      "scriptblock": {
        "begin":  "\\{",
        "end":    "\\}",
        "name":   "meta.scriptblock.powershell",
        "patterns": [
          { "include": "$self" }
        ]
      },
    }


转义序列
****************

请确保对JSON/XML序列内容进行正确的转义。

.. EXPLAIN
