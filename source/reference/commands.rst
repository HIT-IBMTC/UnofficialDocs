=======
命令
=======

概览
========

.. named actions, used everywhere, take json arguments
对命令列表的整理仍在进行中。


关于命令参数中的路径信息
================================

有些命令把路径作为参数。这些命令中，有些支持代码片段那样的语法，有些则不支持。第一中类型的命令
可以接受类似 *${packages}/SomeDir/SomeFile.Ext* 这样的参数，而第二种类型的命令可以接受类似
*Packages/SomeDir/SomeFile.Ext* 这样的参数。

大体上来说，较新引入的命令支持类似代码片段的语法。

通常情况下，认为命令参数中的相对路径是相对与 ``数据`` 目录来说的。

参数中路径包含的变量
-------------------------------

构建系统中使用的变量在参数中也可以使用。参考 :ref:`构建系统变量` 来了解更多信息。


命令列表
========

**build**
	运行某个构件系统。

	- **variant** [String]: 可选参数。要运行配置的名称。

**run_macro_file**
	运行一个 *.sublime-macro* 文件。

	- **file** [String]: 宏文件路径。

**insert_snippet**
	从字符串或者 *.sublime-snippet* 文件中插入一个代码片段。

	- **contents** [String]: 要插入的代码片段的字符串内容。
	- **name** [String]: 要插入的 *.sublime-snippet* 文件的路径。

**insert**
	插入一个字符串。

	- **characters** [String]: 要插入的字符串内容。

**move**
	根据指定的单位移动光标。

	- **by** [Enum]: 可选值: *characters*, *words*, *word_ends*, *subwords*, *subword_ends*, *lines*, *pages*, *stops* 。
	- **forward** [Bool]: 在缓冲区中向前或向后移动。
	- **word_begin** [Bool]
	- **empty_line** [Bool]
	- **punct_begin** [Bool]
	- **separators** [Bool]

**move_to**
	将光标移动到指定位置。

	- **to** [Enum]: 可选值: *bol*, *eol*, *bof*, *eof*, *brackets*.
	- **extend** [Bool]: 是否扩展选择内容。默认值是 ``false`` 。

**new_window**
	打开一个新的窗口。

**close_window**
	关闭当前活跃窗口。

**switch_file**
	在有相同文件名、不同扩展名的两个文件之间进行切换。

	- **extensions** [[String]]: 切换可以发生的文件扩展名（不包括点号）。

**close**
	关闭当前视图。

**close_file**
	关闭当前视图，在某些情况下关闭整个应用程序。
	XXX 看上去好像不对。

**toggle_sidebar**
	开启或关闭侧边栏。

**toggle_full_screen**
	开启或退出全屏模式。

**toggle_distraction_free**
	开启或退出免打扰模式。

**left_delete**
	删除光标前的那个字符。

**right_delete**
	删除光标后的那个字符。

**undo**
	撤销上次操作。

**redo**
	重做上次撤销的操作。

**redo_or_repeat**
	再次执行上次的动作。

**soft_undo**
	先移动到编辑位置再进行撤销操作。

**soft_redo**
	先移动到编辑位置再进行重做操作。

**cut**
	把当前选中的文字从缓冲区中移除，并送到系统剪贴板中。换句话说，执行剪切操作。

**copy**
	把当前选中的文字送到系统剪贴板中。

**paste**
	把剪贴板中的内容插入到光标后。

**paste_and_indent**
	把剪贴板中的内容插入到光标后同时根据上下文进行缩进。

**select_lines**
	在当前选择的内容中添加一行。

	- **forward** [Bool]: 添加下一行还是上一行。默认值是 ``true`` 。

**scroll_lines**
	在视图中滚动行。

	- **amount** [Float]: 正值向下滚动，负值向上滚动。

**prev_view**
	切换到上一个视图。

**next_view**
	切换到下一个视图。

**next_view_in_stack**
	切换到最近的活跃视图。

**previous_view_in_stack**
	切换到最近活跃视图的前一个活跃视图。我不认为这种说法非常确切，这么说甚至是不正确的。

**select_all**
	选择视图的全部内容。

**split_selection_into_lines**
	不出所料的，把当前的选择切散成不同行。

**single_selection**
	把多重选择整合成单一选择。

**clear_fields**
	跳出活跃代码片段域的选择。

**hide_panel**
	隐藏当前活跃面板。

	- **cancel** [Bool]: 当面板打开的时候恢复它之前选择的内容。（仅对增量搜索面板有效。）

**hide_overlay**
	隐藏覆盖控件。使用 show_overlay 命令打开覆盖控件。

**hide_auto_complete**
	隐藏自动补全列表。

**insert_best_completion**
	插入根据当前上下文能推断出的最佳补全内容。 XXX 可能没什么用。 XXX

	- **default** [String]: 当没有找到最佳补全内容时插入的字符串。

**replace_completion_with_next_completion**
	XXX 对用户来说没什么用。 XXX

**reindent**
	XXX ??? XXX

	（译者注：重新进行缩进操作，常用于整理文件缩进。）

**indent**
	增加缩进。

**next_field**
	将光标移动到下一个代码片段中的可修改区域。

**prev_field**
	将光标移动到上一个代码片段中的可修改区域。

**commit_completion**
	向缓冲区中插入自动补全列表中当前选中项的内容。 XXX 对用户来说没很么用。 XXX


**unindent**
	取消缩进。

**toggle_overwrite**
	开启关闭覆盖插入选项。

**expand_selection**
	将选择内容扩展到预定义的边界。

	- **to** [Enum]: 可选值: bol, hardbol, eol, hardeol, bof, eof, brackets, line.

**find_under_expand**
	根据当前选中的内容增加一个新的选择或者把选择项扩展到当前单词。

**close_tag**
	为当前内部文本添加适当的标签。

**toggle_record_macro**
	开始或关闭宏录制器。

**run_macro**
	运行宏缓冲区中存储的宏脚本。

**show_overlay**
	显示请求的覆盖控件。使用 **hide_overlay** 命令来隐藏覆盖控件。

	- **overlay** [Enum]:
                要显示的覆盖控件的类型。可选值：

		- *goto*: 显示 `Goto Anything（快速跳转） <http://docs.sublimetext.info/en/latest/file_management/file_management.html#goto-anything>`_ 覆盖控件。
		- *command_palette*: 显示 `命令面板 <http://docs.sublimetext.info/en/latest/extensibility/command_palette.html>`_.

	- **show_files** [Bool]: 如果显示快速跳转面板，开始的时候显示文件列表，而不是显示一个空的控件。
	- **text** [String]: 放到覆盖控件中的初始值。

**show_panel**
	显示面板。

	- **panel** [Enum]: 可选值: incremental_find, find, replace, find_in_files, console
	- **reverse** [Bool]: 在缓冲区中是否后向搜索内容。
	- **toggle** [Bool]: 当面板已经可见时，是否隐藏面板。

**find_next**
	找到当前搜索内容的下一个匹配项。

**find_prev**
	找到当前搜索内容的上一个匹配项。

**find_under**
	找到与当前选中内容或光标所在位置档次匹配的下一个内容。

**find_under_prev**
	找到与当前选中内容或光标所在位置档次匹配的上一个内容。

**find_all_under**
	选中与当前选择内容或光标所在位置单词匹配的所有内容。

**slurp_find_string**
	复制当前选中内容或当前单词到搜索面板中的 "find" 域。

**slurp_replace_string**
	复制当前选中内容或当前单词到搜索域替换面板中的 "replace" 域。

**next_result**
	移动到下一个搜索到的结果。

**prev_result**
	移动到上一个搜索到的结果。

**toggle_setting**
	修改布尔型设置项的值。

	- **setting** [String]: 要修改的设置项的名称。

**next_misspelling**
	移动到下一个错误拼写的单词的位置。

**prev_misspelling**
	移动到上一个错误拼写的单词的位置。

**swap_line_down**
	交换当前行与下一行。

**swap_line_up**
	交换当前行与上一行。

**toggle_comment**
	为当前行添加或取消注释。

	- **block** [Bool]: 是否使用块注释。

**join_lines**
	把当前行与下一行连接起来。

**duplicate_line**
	重复当前行内容。

**auto_complete**
	打开自动补全列表。

**replace_completion_with_auto_complete**
	XXX 对用户来说没什么用。 XXX

**show_scope_name**
	在状态栏中显示光标所在作用域的名称。

**exec**
	异步运行一个外部进程。

	XXX 为所有选项添加文档。

**transpose**
	移动内容。

**sort_lines**
	对行进行排序。

	- **case_sensitive** [Bool]: 排序时是否考虑大小写。

**set_layout**
	XXX

**focus_group**
	XXX

**move_to_group**
	XXX

**select_by_index**
	XXX

**next_bookmark**
	选择下一个被标记的区域。

**prev_bookmark**
	选择上一个被书签标记的区域。

**toggle_bookmark**
	对活跃区域设置书签或取消书签。（在区域API中使用 ``"bookmarks"`` 作为键可以访问书签内容。）

**clear_bookmarks**
	清楚所有书签。

**select_all_bookmarks**
	选择所有被书签标记过的区域。

**wrap_lines**
	环绕行。默认情况下，在第一个标尺所在的列进行环绕。

	- **width** [Int]: 设置环绕开始的列坐标。

**upper_case**
	把选择的内容改成大写。

**lower_case**
	把选择的内容改成小写。

**set_mark**
	XXX

**select_to_mark**
	XXX

**delete_to_mark**
	XXX

**swap_with_mark**
	XXX

**yank**
	XXX

**show_at_center**
	XXX

**increase_font_size**
	增加字体大小。

**decrease_font_size**
	较少字体大小。

**fold**
	XXX

**unfold**
	XXX

**fold_by_level**
	XXX

**context_menu**
	显示上下文菜单。

.. Some regex-related and search-related commands missing. they don's seem to
.. be too useful.
.. 这里没有列出一些与正则表达式相关或与搜索相关的命令。这些命令看起来没有太大用处。
