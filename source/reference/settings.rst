====================
设置项（参考内容）
====================


全局设置项
===============

**对应文件：** ``Global.sublime-settings`` 。


``theme``
   系统使用的主题文件。接受主题文件的名称（例如： ``Default.sublime-theme`` ）。
``remember_open_files``
   设置Sublime Text启动时是否需要打开上次关闭时打开了的缓冲区。
``folder_exclude_patterns``
   与匹配模式相符的目录将不会出现在侧边栏、快速跳转面板等位置。
``file_exclude_patterns``
   与匹配模式相符的文件将不会出现在侧边栏、快速跳转面板等位置。
``scroll_speed``
   如果把值设为 ``0`` ，将禁止平滑滚动。取 ``0`` 到 ``1`` 之间的数值将使滑动变的比较缓慢，取大于
   ``1`` 的值使得滑动更加迅速。
``show_tab_close_buttons``
   如果设置为 ``false`` ，通常情况下就不会显示标签页的关闭按钮，只有当鼠标移动到标签上时才显示。
``mouse_wheel_switches_tabs``
   如果设置为 ``true`` ，在标签区域滑动鼠标滚轮会触发标签页之间的切换。
``open_files_in_new_window``
   仅在OS X平台有效。当用户从Finder中开打文件，或者把文件拖拽到dock面板的图标上，这个设置项将决定
   是否需要创建一个新窗口。


文件设置项
=============

**对应文件：** ``Base File.sublime-settings``， ``<file_type>.sublime-settings``。

空格与缩进
**************************


``auto_indent``
   开/关自动缩进选项。
``tab_size``
   设置tab键对应的空格数。
``translate_tabs_to_spaces``
   设置当用户按下 :kbd:`Tab` 键时，是否需要用 ``tab_size`` 选项所指定数目的空格进行替换。
``use_tab_stops``
   如果 ``translate_tabs_to_spaces`` 选项为 ``true`` ，设置这个选项就使得每次按下 :kbd:`Tab`
   与 :kbd:`Backspace` 键时，将插入/删除 ``tab_size`` 选项所指定数目的空格。
``trim_automatic_white_space``
   开/关对由 ``auto_indent`` 选项自动添加的只有空格的行的检测。
``detect_indentation``
   如果设置为 ``false`` ，当系统加在缓冲区内容的时候，就会停止对tab与空格的检测。如果设置为 ``true``
   则会自动修改 ``translate_tabs_to_spaces`` 以及 ``tab_size`` 的值。
``draw_white_space``
   有效值： ``none``， ``selection``， ``all`` 。
``trim_trailing_white_space_on_save``
   将这个选项设置为 ``true`` 就会在文件保存时候自动去处行为的空格。

显示设置项
***************

``color_scheme``
   设置文本高亮的配色方案。接受从数据目录开始的相对路径（例如：
   ``Packages/Color Scheme - Default/Monokai Bright.tmTheme``）。
``font_face``
   设置可编辑文本的字体样式。
``font_size``
   设置可编辑字体的字体大小。
``font_options``
   有效值：``bold``， ``italic``， ``no_antialias``， ``gray_antialias``，
   ``subpixel_antialias``， ``directwrite`` （对Windows平台有效）。
``gutter``
   开/关侧边栏选项。
``rulers``
   设置标尺所在的列坐标。接受一个数字值的列表（例如 ``[79, 89, 99]`` 或者一个单一的数字值 ``79``）。
``draw_minimap_border``
   如果将这个值设置为 ``true`` ，就将根据视图当前可见文本的内容在迷你地图区域画一个边框。当前选中的
   配色方案中的 ``minimapBorder`` 键对应的值用于控制边框的颜色。
``highlight_line``
   将这个值设置为 ``false`` 来阻止高亮当前光标所在行。
``line_padding_top``
   每行文字与上一行的额外距离，以像素为单位。
``line_padding_bottom``
   每行文字与下一行的额外距离，以像素为单位。
``scroll_past_end``
   设置为 ``false`` 以关闭超出缓冲区的滚动。如果设置为 ``true`` ，Sublime Text将在文本的最后一行
   与窗口下边界之间添加一大块空白区域。
``line_numbers``
   开/关对行号的显示。
``word_wrap``
   如果设置为 ``false`` 那么对于内容很多的行，文本将被截取，而不会自动换行。在水平方向拖动滚动条可以
   查看被截取的文字。
``wrap_width``
   如果这个值大于 ``0`` ， 系统就会在这个指定的列进行换行切分；否则会根据屏幕的宽度进行换行。这个值
   只有在 ``wrap_width`` 设置为 ``true`` 的时候才有效果。
``indent_subsequent_lines``
   如果这个值设置为 ``false`` ，被转换的行就不会进行缩进。只有在 ``wrap_width`` 为 ``true`` 时
   才有效。
``draw_centered``
   如果设置为 ``true`` ，文本将居中对齐，否则为左对齐。
``match_brackets``
   当值设置为 ``false`` 时，光标放在括号周围的时候就不会显示下划线。
``match_brackets_content``
   设置光标在括号周围时，是否要高亮括号中的内容。
``match_brackets_square``
   如果设置项值为 ``false`` ，就停止对方括号的配对显示。只有在 ``match_brackets`` 值为 ``true``
   时有效。
``match_bracktets_braces``
   如果设置项值为 ``false`` ，就停止对花括号的配对显示。只有在 ``match_brackets`` 值为 ``true``
   时有效。
``match_bracktets_angle``
   如果设置项值为 ``false`` ，就停止对尖括号的配对显示。只有在 ``match_brackets`` 值为 ``true``
   时有效。

自动行为设置项
******************

``auto_match_enabled``
   开/关自动配对引号、括号等符号。
``save_on_focus_lost``
   如果这个值为真，那么当用户切换到一个不同的文件，或不同的应用程序时，文件的内容将会自动保存。
``find_selected_text``
   如果设置为 ``true`` ，那么当打开搜索面板时，当前选中的文字会被自动的复制到搜索面板中。
``word_separators``
   设置在动作中用于分割单词的字符，例如光标跨单词移动的时候来界定单词的分隔符。这个单词分隔符并
   不用于所有需要区分单词的情况（例如：换行时不切分单词）。在这种情况下，文本是基于其他标准来界
   定的（例如：语法定义规则）。
``ensure_newline_at_eof_on_save``
   如果文本末尾没有空行，那么当保存文件的时候，会自动在文件末尾添加一个空行。

系统与其他设置项
*********************************

``is_widget``
   当缓冲区为输入对话框中的输入域的时候，返回 ``true`` 。
``spell_check``
   开/关拼写检查选项。
``dictionary``
   拼写检查器可选的单词列表。接受从数据目录开始的一个路径（例如： ``Packages/Language - English/en_US.dic`` ）。
   你也可以 `添加更多目录 <http://extensions.services.openoffice.org/en/dictionaries>`_ 。
``fallback_encoding``
   控制当无法自动判断编码的时候选用的默认编码。系统可以自动检测的编码包括ASCII，UTF-8以及UTF－16.
``default_line_ending``
   控制系统使用的换行符的字符。有效选项： ``system`` （OS相关）， ``windows`` （即 ``CRLF`` ）以及
   ``unix`` （ ``LF`` ）。
``tab_completion``
   控制按下 :kbd:`Tab` 时是否进行补全。


构建与错误导航设置项
***********************************

``result_file_regex``
   用于过滤视图或输出面板中的错误信息的正则表达式。这里的正则表达式遵循与构建系统中的错误捕获相同的规则。
``result_line_regex``
   用于过滤视图或输出面板中的错误信息的正则表达式。这里的正则表达式遵循与构建系统中的错误捕获相同的规则。
``result_base_dir``
   设置基于 ``result_file_regex`` 以及 ``result_line_regex`` 抽取出来的信息开始搜索的文件路径。
``build_env``
   默认添加到构建系统中的路径列表。


文件与目录设置项
***************************

``default_dir``
   设置视图对应的默认保存路径。


输入设置项
**************

``command_mode``
   如果将这个值设为 ``true`` ，则缓冲区将忽略用户按键。这对模拟Vim很有帮助。
