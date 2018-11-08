======
宏
======

宏是一个基本的自动化设施，由命令序列组成。只要您需要重复完全相同的步骤来执行操作，请使用它们。

宏文件是JSON文件，扩展名为 ``.sublime-macro``。Sublime Text附带了一些提供核心功能的宏，例如行和字删除。你可以在 **Tools | Macros** 下找到这些宏。

如何录制宏
********************

要开始录制宏，请按 :kbd:`Ctrl+q`，然后逐个执行所需的步骤。完成后，再次按 :kbd:`Ctrl+q` 停止宏录制器。您的新宏不会保存到文件中，而是保存在宏缓冲区中。现在，您可以通过按 :kbd:`Ctrl+Shift+q` 运行录制的宏，或通过选择 **Tools | Save macro…** 将其保存到文件中......。

请注意，宏缓冲区只会记住最新记录的宏。此外，记录的宏仅捕获发送到缓冲区的命令：窗口级命令（例如创建新文件）将被忽略。

如何编辑宏
******************

或者录制宏，您可以手动编辑它。在 :file:`Packages\User` 下保存扩展名为 ``.sublime-macro`` 的新文件，并向其添加命令。这是宏文件的样子：

   [
       {"command": "move_to", "args": {"to": "hardeol"}},
       {"command": "insert", "args": {"characters": "\n"}}
   ]

有关命令的更多信息，请参见 :doc:`../core/commands` 部分。

.. XXX: do we need to escape every kind of quotations marks?

如果您手动编辑宏，则需要通过在 ``\`` 前面加上引号，空格和反斜杠来转义。

存储宏的位置
*********************

宏文件可以存储在任何包文件夹中，它们将显示在 **Tools | Macros | <PackageName>** 下。


