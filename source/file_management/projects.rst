==========
项目
==========

项目将文件和文件夹组成一组，以保持你的工作井井有条。它们支持特定于项目的设置和构建系统，你可以在它们之间快速切换，以便在你离开的地方继续工作。

对于 :ref:`fm-goto-anything` 和项目范围的Goto定义而言，向项目添加文件夹是非常必要的。

即使你尚未创建或打​​开一个项目，也始终存在活动项目。在这种情况下，你正在使用一个功能有限的 *匿名项目*。新窗口在首次打开时始终使用匿名项目。

项目元数据存储在具有 ``.sublime-project`` 扩展名的JSON文件中。只要有 ``.sublime-project`` 文件，你就会找到一个辅助 ``.sublime-workspace`` 文件。``.sublime-workspace`` 文件包含你不 *应* 编辑的会话数据。（稍后将详细介绍工作空间。）

.. note::

   一般来说，将 ``.sublime-project`` 文件提交到源代码存储库是很好的，但始终要注意存储在它们中的内容。

   以上情况并非如此，在不是每个人都使用Sublime Text作为编辑器的项目中，建议将 ``.sublime-project`` 文件保留在项目的存储库之外。


创建一个项目
==================

通过打开新窗口或使用 **Project → Close Project** 菜单关闭任何活动项目，从匿名项目开始。

你可以使用 **Project** 菜单或侧边栏的上下文菜单在项目中添加和删除文件夹。如果将文件夹拖到Sublime Text窗口，它也将被添加到项目中。

要保存匿名项目，请转到 **Project → Save Project As...**。

保存项目后，你可以手动编辑它以调整更多选项。


打开项目
================

使用主菜单，你可以通过选择 **Projects → Open Recent**，**Projects → Switch Project…** 或 **Projects → Quick Switch Project…** 来打开或切换项目。

切换项目时，Sublime Text将关闭当前项目并在同一窗口中打开指定的项目。打开项目时，Sublime Text将打开一个新窗口并在那里打开所选项目。

与项目相关的键盘快捷键：

+----------------------------------+-----------------------+
| **Quick Switch Project…**        | Ctrl + Alt + P        |
+----------------------------------+-----------------------+

.. note::

   适用于Windows的build 3096删除了按键绑定，如果需要，必须手动添加。为此，请将以下按键绑定添加到用户按键绑定文件：

   .. code-block:: json

      { "keys": ["ctrl+alt+p"], "command": "prompt_select_workspace" }

此外，你可以通过将 ``.sublime-project`` 文件作为参数传递给Sublime Text附带的 ``subl`` 命令行帮助程序，从命令行打开项目。


项目文件的高级配置
========================================

除了单个目录的更多选项外，项目还可以具有特定的构建系统或设置覆盖。

.. seealso::

    :doc:`/reference/projects`
        有关项目文件格式和选项的文档。


与侧边栏和项目相关的设置
============================================

``binary_file_patterns``
    通配符列表。匹配这些通配符的文件将显示在侧边栏中，但将从Goto Anything和Find in Files中排除。

.. TODO: file_exlude_patterns and folder_exlude_patterns also exist
.. TODO: Add reference to setting or explain wildcards

工作空间
==========

工作空间包含与项目关联的会话数据，其中包括有关已打开文件，窗格布局，查找历史记录等的信息。项目可以有多个工作空间。

工作空间的一个常见用例是处理 *同一项目中* 的不同功能，其中每个功能都需要打开一组不同的文件，并且你希望快速切换功能。在这种情况下，你将需要第二个工作空间。编写测试可能就是一个例子。

工作空间的行为与项目非常相似。要创建新工作空间，请选择 **Project → New Workspace for Project**。要保存活动的工作空间，请选择 **Project → Save Workspace As...**。

工作空间元数据存储在带有 ``.sublime-workspace`` 扩展的JSON文件中，你不应该编辑它。

要在不同的工作空间之间切换，请使用 :kbd:`Ctrl+Alt+P`，就像处理项目一样。

与项目一样，你可以通过将所需的 ``.sublime-workspace``文件作为参数传递给Sublime Text附带的 ``subl`` 命令行帮助程序，从 **命令行** 打开工作空间。

.. caution::

    与 ``.sublime-project`` 文件不同，``.sublime-workspace`` 文件不是手动共享或编辑的。你永远不应将 ``.sublime-workspace`` 文件提交到源代码存储库中。它们可能包含敏感信息。
