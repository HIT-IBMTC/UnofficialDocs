========
命令
========

命令在Sublime Text中无处不在：按键绑定，菜单项包括宏等它们都是工作在命令系统里的。因此，命令随处可见。

一些命令可以通过编辑器内核接口来实现，但是更多的是作为python的插件来提供。任何命令都可以通过python插件来调用。

命令调度
*******************

通常，命令都绑定在应用对象，窗口对象或者视图对象里。不过，窗口对象根据输入焦点来分发命令，因此你可以通过窗口对象发起一个
命令并且会为你返回一个正确的视图实例。

命令剖析
********************

命令名通常被下划线所分割，就像 ``hot_exit`` ，而且是一个以字符串为关键字JSON类型为值的字典。下面是一些运行在python控制台下
的命令例子:

::


   view.run_command("goto_line", {"line": 10})
   view.run_command('insert_snippet', {"contents": "<$SELECTION>"})
   view.window().run_command("prompt_select_project")


.. seealso::

   :doc:`Reference for commands <../reference/commands>`
        Command reference.
