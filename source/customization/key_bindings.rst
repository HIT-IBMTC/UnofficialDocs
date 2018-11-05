============
键位绑定
============

.. seealso::
更多信息请参考

   :doc:`键位绑定参考文档 <../reference/key_bindings>`
        有关键位绑定的完整文档。

键位绑定使得你能够将按键与动作进行映射。

文件格式
===========

键位绑定是以 ``.sublime-keymap`` 作为扩展名的JSON文件。为了让键位能够更好的与各平台融合，系统为
Linux，OSX以及Windows平台分别提供了不同的文件。只有与当前系统相符的键位映射文件才会生效。

示例
*******

下面这段代码是从Windows平台默认的按键映射文件中截取出来的::

	[
		{ "keys": ["ctrl+shift+n"], "command": "new_window" },
		{ "keys": ["ctrl+o"], "command": "prompt_open_file" }
	]

定义与重载键位绑定
====================================

Sublime Text自带一套默认的键位绑定（一般对应这个文件：
:file:`Packages/Default/Default (Windows).sublime-keymap)`）。如果你想重载其中的部分键位
或者添加新的键位绑定，你可以在具有更高文件优先级的位置重新存储一个文件，例如
:file:`Packages/User/Default (Windows).sublime-keymap`。

请参考 :ref:`排序与优先级顺序` 来了解Sublime Text中关于文件优先级以及文件合并的更多信息。

高级键位绑定
=====================

对于简单的键位绑定，就是一个键位组合对应一个命令。除此之外，还有一些传递参数和上下文信息的复杂语法。

参数传递
*****************

通过 ``args`` 键可以指定需要的参数::

		{ "keys": ["shift+enter"], "command": "insert", "args": {"characters": "\n"} }

在这个例子中当你按下 :kbd:`Shift+Enter` 的时候， ``\n`` 将会作为参数传递给 ``insert`` 命令。

上下文
********

利用上下文，可以通过插入符的位置及其他状态信息来决定某种键位组合是否可以发挥作用。

::

	{ "keys": ["escape"], "command": "clear_fields", "context":
		[
			{ "key": "has_next_field", "operator": "equal", "operand": true }
		]
	}

这段代码的作用是 *当有下一个可用域的时候就清空当前代码片段，并返回正常编辑模式* 。因此，当你
没有在代码域中循环切换的时候，按下 :kbd:`ESC` 键就 *不会* 触发这个按键绑定（然而，如果 :kbd:`ESC`
恰巧与另外的上下文进行了绑定，那么那个动作仍然会被触发——事实上，对于 :kbd:`ESC` 来说，这种情况
并不少见）。
