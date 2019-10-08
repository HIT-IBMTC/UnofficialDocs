======
宏
======
宏是包含命令序列的基本自动化设施。只要您需要重复完全相同的步骤来执行操作，请使用它们。

宏文件是JSON文件，扩展名为 ``.sublime-macro``。Sublime Text附带了一些提供核心功能的宏，例如行和字删除。你可以在 **Tools | Macros** 下或者 ``Packages/Default`` 中找到这些宏。

如何录制宏
********************
要开始录制宏，请按 ``Ctrl+q`` ，然后逐个执行所需的步骤。完成后，再次按 ``Ctrl+q`` 停止宏录制器。您的新宏不会保存到文件中，而是保存在宏缓冲区中。现在，您可以通过按 ``Ctrl+Shift+q`` 运行录制的宏，或通过选择 **Tools | Save macro…** 将其保存到文件中。

请注意，宏缓冲区将仅记住最新记录的宏。此外，宏仅记录发送到缓冲区的命令：窗口级命令（如创建新文件）将被忽略。

如何编辑宏
******************
作为录制宏的替代方法，您可以手动编辑它。只需在 ``Packages/User`` 下保存带有 ``.sublime-macro`` 扩展名的新文件，然后向其中添加命令即可。宏文件具有以下格式::

   [
       {"command": "move_to", "args": {"to": "hardeol"}},
       {"command": "insert", "args": {"characters": "\n"}}
   ]

有关命令的更多信息，请参见 :doc:`../reference/commands` 部分。

.. XXX: do we need to escape every kind of quotations marks?

如果您手动编辑宏，则需要通过在 ``\`` 前面加上引号，空格和反斜杠来转义。

存储宏的位置
*********************
宏文件可以存储在任何包文件夹中，它们将显示在 **Tools | Macros | <PackageName>** 下。

宏的按键绑定
*************
通过将宏文件路径传递给 *run_macro_file* 命令，可以将宏文件绑定到组合键，如下所示::

{"keys": ["super+alt+l"], "command": "run_macro_file", "args": {"file": "res://Packages/User/Example.sublime-macro"}}