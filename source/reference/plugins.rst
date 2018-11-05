=======
插件
=======

.. seealso::

   :doc:`API参考文档 <../reference/api>`
        有关Python API的更详细的信息。



插件是实现了 ``sublime_plugin`` 模块中 ``*Command`` 类的Python脚本。

插件放到哪里
**********************

Sublime Text 2在以下这些目录中寻找可用插件：

* ``Packages``
* ``Packages/<pkg_name>``
* ``Packages/<包名称>``

存放在 ``Packages`` 更深层次的目录结构中的插件不会被加载。


任何插件都应该又自己单独的目录，而不应该直接放在 ``Packages`` 目录下。


命令名称的命名规则
*****************************

Sublime Text 2遵循 ``CamelCasedPhrases`` 命名规则来定义命令类，同时用 ``Command`` 作为后缀。

然而，Sublime Text 2将类名从 ``CamelCasedPhrases`` 形式自动转换成 ``camel_cased_phrases`` 。
举例来说， ``ExampleCommand`` 将被转换为 ``example`` ，而 ``AnotherExampleCommand`` 将
转换为 ``another_example`` 。

对于类定义名称，可以使用 ``CamelCasedPhrasesCommand`` 命名方式；要从API中调用一个命令，请使用
标准化后的名称（ ``camel_cased_phrases`` ）。


命令类型
*****************

* ``sublime_plugin.ApplicationCommand``
* ``sublime_plugin.WindowCommand``
* ``sublime_plugin.TextCommand``
* ``sublime_plugin.EventListener``

``WindowCommand`` 类的实例都有一个 ``.window`` 属性，指向创建他们的那个窗口实例。于此类似，
``TextCommand`` 类的实例拥有 ``.view`` 属性。

命令之间的共享特性
--------------------------

所有命令都必须实现一个 ``.run()`` 方法。
所有命令都可以接受任意长度的关键字参数，但是这些参数一定要是有效的JSON类型。


如何利用API调用命令
*********************************

Use a reference to a ``View`` or a ``Window``, or ``sublime`` depending on
the type of command, and call ``object.run_command('command_name')``.
In addition, you can pass a dictionary where keys are names of parameters
to ``command_name``. ::

   window.run_command("echo", {"Tempus": "Irreparabile", "Fugit": "."})


命令参数
*****************

 用户提供给命令的所有参数都必须是有效的JSON类型。只有Sublime Text可以给命令传递其他类型的参数（例如修改对象，视图实例等等）。

根据命令的类型来选择对 ``View`` 、 ``Window`` 或者 ``sublime`` 的引用，然后通过
``object.run_command('command_name')`` 来调用。
除此之外, 你可以用字典作为参数，字典中的键就是要传给 ``command_name`` 的参数名称::

   window.run_command("echo", {"Tempus": "Irreparabile", "Fugit": "."})




文本命令以及 ``修改`` 对象
*************************************

关于两个文本命令的两个较为重要的API是 ``view.begin_edit()`` 和 ``view.end_edit()`` 。
其中 ``view.begin_edit()`` 可以接受一个可选的命令名称和可选的参数字典；而 ``view.end_edit()``
用来结束编辑过程。

在修改过程中发生的所有动作都被整合成一个单一的撤销动作。当编辑过程结束的时候，类似与
``on_modified`` 和 ``on_selection_modified()`` 的回调函数会被触发。

有一点非常重要，每次调用 ``view.begin_edit()`` 之后必须调用 ``view.end_edit()`` 方法，否则
缓冲将处于一种不一致状态。在不匹配发生时，系统将尝试自动进行修正，但是这种自动修正的发生频率并
没有你想象的那么多，同时会在控制台输出一条警告信息。换句话说，你应该始终使用 ``try..finally``
来包裹代码快。

传递给 ``begin_edit()`` 的命令名称用于重复、宏录制以及撤销/重复过程中的动作描述。如果你在
``TextCommand`` 外面做修改，你就不该指定命令名称。

你可以在开始的时候创建一个修改对象，然后调用了一个方法又创建了另外的一个修改对象：只有当最外层
的修改对象执行了 ``end_edit()`` 方法之后，系统才认为整个修改都完成了。

与群组修改类似，你也可以使用修改对象把对选中文本做的修改组合成一个群组，对它们的修改只要一步就
能撤销。


对事件的响应
********************

任何 ``EventListener`` 的子类都能够响应事件。对于任何一个类，不能同时继承 ``EventListener``
和其他任何的命令类。

.. sidebar:: 关于 ``EventListener`` 的注意事项

  在事件监听器中，尤其是处理 ``on_modified`` 和 ``on_selection_modified`` 这样的经常被
  触发的事件的监听器中，执行开销很大的操作可能会导致Sublime Text 2没有响应。所以请注意监听器
  中所编写的代码，另外，不要实现你不需要的方法，即使这个方法只有一个 ``pass``语句。

Python与标准库
*******************************

Sublime Text集成了一个精简版的标准库。被裁剪掉的模块包括 *Tkinter* ， *multiprocessing*
以及 *sqlite3* 。


自动插件重载
***********************

当你对插件做修改的时候（比如你正在修改一个 *.py* 文件），Sublime Text会自动加载包中的顶级
Python模块。值得注意的是Python的子模块不会被自动重新加载；这一点在插件开发中可能会产生一些挺
奇葩的问题。一般来说，当你对插件文件做修改之后，最好重启以下Sublime Text，这样能保证你做的修
改能发挥作用。


多线程
**************

只有 ``.set_timeout()`` 方法可以安全的从其他线程调用。
