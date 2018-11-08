===========
补全
===========

.. seealso::

   :doc:`Reference for completions <../reference/completions>`
      有关所有可用选项的完整文档。
   `Sublime Text Documentation <http://www.sublimetext.com/docs/2/tab_completion.html>`_
   	关于此主题的官方文档。

补全提供了IDE的精神功能，以建议术语和插入片段。补全通过补全列表工作，或者可选择按 :kbd:`Tab` 键。

请注意，Sublime Text将查找并为您插入的更广泛意义上的单词的补全不仅限于补全文件，因为其他来源有助于补全单词列表，即：

	 * 代码片段
	 * API注入的补全
	 * 缓冲内容

但是，``.sublime-completions`` 文件是Sublime Text为您提供的最明确的方式。本主题涉及 ``.sublime-completions`` 文件的创建以及所有补全源之间的交互。


文件格式
===========

补全是具有 ``.sublime-completions`` 扩展名的JSON文件。补全文件中的条目可以包含片段或纯字符串。


例如
*******

以下是HTML补全的摘录：

.. code-block:: js

	{
		"scope": "text.html - source - meta.tag, punctuation.definition.tag.begin",
	
		"completions":
		[
			{ "trigger": "a", "contents": "<a href=\"$1\">$0</a>" },
			{ "trigger": "abbr", "contents": "<abbr>$0</abbr>" },
			{ "trigger": "acronym", "contents": "<acronym>$0</acronym>" }
		]
	}

``scope``
	确定何时使用此补全列表填充补全列表。有关更多信息，请参阅 :ref:`scopes-and-scope-selectors`。

在上面的示例中，我们仅使用基于触发器的补全，但补全文件也支持简单补全。简单的补全只是简单的字符串。通过一些简单的补全扩展我们的示例，我们最终得到一个如下列表：

.. code-block:: js

	{
		"scope": "text.html - source - meta.tag, punctuation.definition.tag.begin",
	
		"completions":
		[
			{ "trigger": "a", "contents": "<a href=\"$1\">$0</a>" },
			{ "trigger": "abbr", "contents": "<abbr>$0</abbr>" },
			{ "trigger": "acronym", "contents": "<acronym>$0</acronym>" },
			
			"ninja",
			"robot",
			"pizza"
		]
	}


补全来源
=======================

补全不仅源自 ``.sublime-completions`` 文件。这是补全源的详尽列表：

	* 代码片段
	* API注入的补全
	* ``.sublime-completions`` 文件
	* 缓冲区中的单词

补全来源的优先顺序
***********************************

这是补全优先顺序的排列

	* 代码片段
	* API注入的补全
	* ``.sublime-completions`` 文件
	* 缓冲区中的单词

如果当前前缀与其制表符触发器完全匹配，则片段将始终获胜。对于其余的补全源，执行模糊匹配。此外，片段总是会在模糊匹配中丢失。请注意，这仅在要自动插入补全时才有意义。显示补全列表时，即使前缀仅部分匹配代码段的选项卡触发器，也会沿其他项列出代码段。

如何使用补全
======================

有两种方法可以使用补全，虽然在补全筛选时给予补全的优先级总是相同的，但结果会有不同，下面将对此进行解释。

可以通过两种方式插入补全：

	* 通过补全列表（:kbd:`Ctrl+spacebar`）;
	* 按 :kbd:`Tab`.


补全清单
********************

补全列表（:kbd:`Ctrl+空格键`）可以以两种方式工作：通过调出要补全的建议单词列表，或直接插入最佳匹配。

如果最佳补全的选择不明确，则将向用户呈现交互式列表，该用户必须自己选择项目。与其他项目不同，此列表中的片段以以下格式显示：``<tab_trigger>：<name>``，其中 ``<tab_trigger>`` 和 ``<name>`` 是可变的。

使用 :kbd:`Ctrl+空格键` 补全只有在补全候选列表可以缩小到给定当前前缀的一个明确选择时才会自动完成。

:kbd:`Tab`-完成补全
********************************

如果您希望能够通过制表符补全，则必须将设置 ``tab_completion`` 设置为 ``true``。默认情况下，``tab_completion`` 设置为 ``true``。片段标签完成不受此设置的影响：它们将始终根据其标签触发器完成。

启用 ``tab_completion`` 后，项目的补全始终是自动的，这意味着，与补全列表的情况不同，Sublime Text将始终为您做出决定。选择最佳补全的规则与上述相同，但如果含糊不清，Sublime Text仍将插入被认为最合适的项目。

插入文字制表符
---------------------------------

启用 ``tab_completion`` 后，可以按 ``Shift+Tab`` 键插入文字制表符。