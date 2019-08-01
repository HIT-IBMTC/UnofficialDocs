========
命令
========
命令在Sublime Text中无处不在：按键绑定，菜单项和宏都通过命令系统工作。它们也可以在其他地方找到。

一些命令在编辑器的核心中实现，但其中许多命令都是作为Python插件提供的。可以从Python插件调用每个命令。

命令调度
*******************
通常，命令绑定到应用程序对象，窗口对象或视图对象。但是，窗口对象将根据输入焦点调度命令，因此您可以从窗口对象发出视图命令，并为您找到正确的视图实例。

命令剖析
********************
命令的名称由下划线（snake_case）分隔，如 ``hot_exit`` ，并且可以使用参数字典，其键必须是字符串，其值必须是JSON类型。以下是从Python控制台运行的一些命令示例：

::


   view.run_command("goto_line", {"line": 10})
   view.run_command('insert_snippet', {"contents": "<$SELECTION>"})
   view.window().run_command("prompt_select_project")


.. seealso::

   :doc:`命令参考文档 <../reference/commands>`
        命令参考内容
