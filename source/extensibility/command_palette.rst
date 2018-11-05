===============
命令面板
===============

.. seealso::
  
   :doc:`命令面板参考文档 <../reference/command_palette>`
      有关命令面板选项的完整文档。


概述
========

*命令面板* 是一个绑定到键盘 `Ctrl+Shift+P` 的交互列表，其目的在意执行命令。命令面板
与命令文件相互联系。通常，命令不保证产生一个按键绑定，可以在 ``.sublime-commands``
中作为一些很好的候选。

文件格式（命令文件）
============================

命令文件使用JSON，并且有一个 ``.sublime-commands`` 的扩展。

如下是来自 ``Packages/Default/Default.sublime-commands`` 的实例::

   [
       { "caption": "Project: Save As", "command": "save_project_as" },
       { "caption": "Project: Close", "command": "close_project" },
       { "caption": "Project: Add Folder", "command": "prompt_add_folder" },
   
       { "caption": "Preferences: Default File Settings", "command": "open_file", "args": {"file": "${packages}/Default/Base File.sublime-settings"} },
       { "caption": "Preferences: User File Settings", "command": "open_file", "args": {"file": "${packages}/User/Base File.sublime-settings"} },
       { "caption": "Preferences: Default Global Settings", "command": "open_file", "args": {"file": "${packages}/Default/Global.sublime-settings"} },
       { "caption": "Preferences: User Global Settings", "command": "open_file", "args": {"file": "${packages}/User/Global.sublime-settings"} },
       { "caption": "Preferences: Browse Packages", "command": "open_dir", "args": {"dir": "$packages"} }
   ]

``caption``
   显示在命令面板中的标题.
``command``
   待执行的命令.
``args``
   传给 ``command`` 的参数。

如何使用命令面板
==============================

#. 按下键盘的 `Ctrl+Shift+P`
#. 选择命令

命令面板通过文本过滤选项，所以无论什么时候打开，你都不会看到每一个 ``.sublime-commands`` 
文件的所有命令。