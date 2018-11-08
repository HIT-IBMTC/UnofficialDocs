============
按键绑定
============

按键绑定为按键与动作建立了映射关系。


文件格式
***********

对于按键绑定的配置存储于后缀为 ``.sublime-keymap`` 的文件中，文件中记录的是JSON内容。所有按键绑定
配置文件都需要按照如下模式命名： ``Default (<platform>).sublime-keymap`` 。否则，Sublime Text
将忽略这些文件。


平台相关的键位设置
--------------------------

每一个平台都有它自己的按键配置文件：

* ``Default (Windows).sublime-keymap``
* ``Default (OSX).sublime-keymap``
* ``Default (Linux).sublime-keymap``

也有针对不同的硬件提供商指定的 `HCI <http://en.wikipedia.org/wiki/Human%E2%80%93computer_interaction>`_ 指导文档准备的按键配置文件。

一个按键绑定项的结构
--------------------------

键位表是一个按键绑定项的数组。接下来将要解释的是按键绑定中的有效构成元素：

``keys``
	一组大小写敏感的按键组合。可以用 ``+`` 指定修饰符。向数组中添加元素可以设置组合键，例如：
	``["ctrl+k","ctrl+j"]``。

``command``
	要执行的命令的名称。

``args``
	传递给 ``command`` 命令的参数字典。字典中键的名称是 ``command`` 命令的参数名称。

``context``
	选择性控制按键绑定是否起效的上下文内容数组。只有当所有上下文都为真时，按键绑定才能被触发。
	请参考下面的 :ref:`上下文参考` 内容来了解更多内容。

下面是一个说明上面提到的大部分特性的例子::

	{ "keys": ["shift+enter"], "command": "insert_snippet", "args": {"contents": "\n\t$0\n"}, "context":
		[
			{ "key": "setting.auto_indent", "operator": "equal", "operand": true },
			{ "key": "selection_empty", "operator": "equal", "operand": true, "match_all": true },
			{ "key": "preceding_text", "operator": "regex_contains", "operand": "\\{$", "match_all": true },
			{ "key": "following_text", "operator": "regex_contains", "operand": "^\\}", "match_all": true }
		]
	}

.. _context-reference:
.. _上下文参考:


上下文的结构
----------------------

``key``
	要请求的上下文操作符的名称。

``operator``
	对 ``key`` 进行测试的类型。

``operand``
	与 ``key`` 的结果进行测试的值。

``match_all``
	需要多所有选择都进行测试。默认值为 ``false`` 。

上下文操作子
^^^^^^^^^^^^^^^^

``auto_complete_visible``
	如果自动不全列表可见就返回 ``true`` 。

``has_next_field``
	当下一个代码片段域可用时返回 ``true`` 。

``has_prev_field``
	当上一个代码片段域可用时返回 ``true`` 。

``num_selections``
	返回当前选中的数目。

``overlay_visible``
	当覆盖控件可见时返回 ``true`` 。

``panel_visible``
	当有面板可见时返回 ``true`` 。

``following_text``
	限制测试只对脱字号后的文本进行。

``preceding_text``
	限制测试只对脱字号前的文本进行。

``selection_empty``
	当没有选中内容的时候返回 ``true`` 。

``setting.x``
	返回 ``x`` 设置项的值。 ``x`` 可以为任意字符串。

``text``
	限制测试只对选中的文本有效。

``selector``
	返回当前作用域。

上下文操作符
^^^^^^^^^^^^^^^^^

``equal``, ``not_equal``
	测试是否相等。

``regex_match``, ``not_regex_match``
	与一个正则表达式进行匹配。

``regex_contains``, ``not_regex_contains``
	与一个正则表达式进行匹配（检测是否包含）。



命令模式
************

Sublime Text提供一个称为 ``command_mode`` （命令模式）的设置项，启用这个设置可以阻止按键内容
被送往缓冲区。这个设置项在模拟Vim的模式功能的时候很有用处。


可绑定的按键
*************

按键可以通过字面值或者名字来指定。你可以在下面找到一个有效名称的列表：

* ``up``
* ``down``
* ``right``
* ``left``
* ``insert``
* ``home``
* ``end``
* ``pageup``
* ``pagedown``
* ``backspace``
* ``delete``
* ``tab``
* ``enter``
* ``pause``
* ``escape``
* ``space``
* ``keypad0``
* ``keypad1``
* ``keypad2``
* ``keypad3``
* ``keypad4``
* ``keypad5``
* ``keypad6``
* ``keypad7``
* ``keypad8``
* ``keypad9``
* ``keypad_period``
* ``keypad_divide``
* ``keypad_multiply``
* ``keypad_minus``
* ``keypad_plus``
* ``keypad_enter``
* ``clear``
* ``f1``
* ``f2``
* ``f3``
* ``f4``
* ``f5``
* ``f6``
* ``f7``
* ``f8``
* ``f9``
* ``f10``
* ``f11``
* ``f12``
* ``f13``
* ``f14``
* ``f15``
* ``f16``
* ``f17``
* ``f18``
* ``f19``
* ``f20``
* ``sysreq``
* ``break``
* ``context_menu``
* ``browser_back``
* ``browser_forward``
* ``browser_refresh``
* ``browser_stop``
* ``browser_search``
* ``browser_favorites``
* ``browser_home``

修饰符
---------

* ``shift``
* ``ctrl``
* ``alt``
* ``super`` (Windows key, Command key…)
* ``super`` （在Windows平台为Windows键，在OS X平台为Command键……）

关于可绑定按键的警告
---------------------------

如果你正在开发一个包，请谨记下面几点：

* 在Windows平台上，不要使用 ``Ctrl+Alt+<alphanum>`` 进行任何键位绑定。
* 在OS X平台上，不要使用 ``Option+<alphanum>`` 进行任何键位绑定。

在以上两种情况下，用户都将在插入非ascii字符时遇到问题。

如果你是终端用户，你可以随意重新映射这些按键组合。


让按键映射井井有条
**************************

Sublime Text自带的按键组合存放在 ``Packages/Default`` 目录下。其他包组可以包含它们特有的按键
映射文件。对于你自己的键位映射设置而言，推荐的文件存放地址是 ``Packages/User`` 目录。

请参考 :ref:`排序与优先级顺序` 以了解关于Sublime Text排序文件并进行合并的更多信息。

国际化键盘
***********************

根据Sublime Text将按键名称与物理按键进行映射的方式，此二者在不同平台上可能有不同的配对。


常见问题解答
***************

.. TODO: fix formatting for API cross-ref.

使用 `sublime.log_commands(flag)`_ 开启命令日志。这对于调试按键映射有所帮助。

.. _sublime.log_commands(flag): http://www.sublimetext.com/docs/2/api_reference.html
