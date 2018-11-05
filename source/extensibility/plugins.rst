=======
插件
=======

.. seealso::
更多信息请参考：

   :doc:`API参考文档 <../reference/api>`
        有关Python API的更详细的信息。

   :doc:`插件参考文档 <../reference/api>`
        有关插件的更详细的信息。


Sublime Text 2可以使用Python脚本进行编程。插件可以重用已有的命令或者新建新的特性。
插件是指逻辑上的，而不是屋里存在的。

预备知识
*************

要写一个插件，你必须要会用 Python_ 编程。

.. _Python: http://www.python.org


插件放到哪里
**********************

Sublime Text 2会在以下目录中寻找可用的插件：

* ``Packages``
* ``Packages/<pkg_name>/``
* ``Packages/<包名称>``

所以，存放在 ``Packages`` 更深层次的目录结构中的插件不会被加载。

让插件都存放在 ``Packages`` 会有些让人失望，因为Sublime Text会在加载插件之前
通过预先设定的方式排序包组。因此，如果你的插件在一个包的外面，可能会有意想不到的结果。

编写第一个插件
*****************

下面，我们给Sublime Text 2写一个"Hello, World!"插件吧：

#. 选择目录 **Tools | New Plugin…** 。
#. 保存到 ``Packages/User/hello_world.py`` 。

你现在完成了你的第一个插件，下面将其投入使用：

#. 新建一个缓冲区（``Ctrl+n``）。
#. 打开python命令行（``Ctrl+```）。
#. 输入：``view.run_command("example")``，然后按下回车。

你会看到你的新缓冲区内显示"Hello, World!"。

分析你的第一个插件
***************************

前面我们编写插件大致如此：

    import sublime, sublime_plugin

    class ExampleCommand(sublime_plugin.TextCommand):
        def run(self, edit):
            self.view.insert(edit, 0, "Hello, World!")


``sublime`` 和 ``sublime_plugin`` 是Sublime Text 2提供的。

新命令继承定义在 ``sublime_plugin`` 的 ``*Command`` 类（后面会进行详细的描述）。

剩下的代码是来自于 ``TextCommand`` 的细节以及我们后面要讨论的API。

在向下继续之前，嗯。。我们会看到如何调用新命令：首先，我们打开python命令行，然后
调用 ``view.run_command()`` 。这样使用插件显然是很不方便的，但是在开发阶段还是比较
有用的。目前为止，你要记住，自定义的命令可以通过按键绑定或者其他方式使用，就像其
他命令一样。

命令名称的命名规则
-----------------------------

你可能已经注意到了我们的命令是以 ``ExampleCommand`` 命名的，但是我们向API传入的参数却是 
``example`` 。Sublime Text 2是通过截断 ``Command`` 的后缀，在 ``CamelCasedPhrases`` 中加入
下弧线的方式，比如 ``camel_cased_phrases`` ，来统一命令名称的。

新的命令必须要按照上面的方式命名类的名称。

命令类型
*****************

你可以新建以下类型的命令：

* 应用程序命令（``ApplicationCommand``）
* 窗口命令（``WindowCommand``）
* 文本命令（``TextCommand``）

当你编写插件时，请根据你的目的选择合适的命令类型。

命令之间的共享特性
-------------------------

所有的命令都必须实现一个 ``.run()`` 方法才能运行。另外，所有命令都可以接受任意长度的
关键字参数。

应用程序命令
--------------------

应用程序命令继承于 ``sublime_plugin.ApplicationCommand`` ，可以通过 ``sublime.run_command()`` 执行。

窗口命令
---------------

串口命令是在窗口级生效的。这并不意味着你不能通过窗口命令控制视图，但是这也并不
是不需要你开启一些视图来让窗口命令生效。比如，内置命令 ``new_file`` 是一个 
``窗口命令`` ，然而没有视图一样可以生效。因为此时要求打开一个视图并没有意义。

窗口命令实例有一个 ``.window`` 属性，指向创建它们的那个窗口实例。

窗口命令的 ``.run()`` 方法不需要传入参数。

文本命令
-------------

文本命令是在缓冲区级生效的，并且需要存在一个缓冲区使之生效。

视图命令实例有一个 ``.view`` 属性，指向创建它们的那个窗口实例。

窗口命令的 ``.run()`` 方法需要一个 ``edit`` 实例作为第一个入参。

文本命令和 ``edit`` 对象
-------------------------------------

编辑 ``edit`` 对象组修改视图，以便撤销和宏命令是以合理的方式运行。你有责任新建和
关闭edit对象。为此，你需要调用 ``view.begin_edit()`` 和 ``edit.end_edit()``。
文本命令为了方便，在其 ``run`` 方法中获取传入的 ``edit`` 对象。另外，许多 ``View``
方法都需要一个edit对象。 

事件响应
--------------------

任何继承自 ``EventListener`` 的命令都可以响应事件。

其他插件的例子：添加补全列表
----------------------------------------------------

接下来，我们做一个从Google自动完成服务获取数据的插件，然后添加到Sublime Text 2
的补全列表。请注意对于将其作为一个插件，这并不是什么好主意。

::

	import sublime, sublime_plugin

	from xml.etree import ElementTree as ET
	from urllib import urlopen

	GOOGLE_AC = r"http://google.com/complete/search?output=toolbar&q=%s"

	class GoogleAutocomplete(sublime_plugin.EventListener):
	    def on_query_completions(self, view, prefix, locations):
	        elements = ET.parse(
	                        urlopen(GOOGLE_AC % prefix)
	                    ).getroot().findall("./CompleteSuggestion/suggestion")

	        sugs = [(x.attrib["data"],) * 2 for x in elements]

	        return sugs

.. note::
	确保你在这次尝试以后，不要保留这个插件，否则会干扰Google的自动完成系统的。

学习API
****************

为了编写插件，你需要熟悉Sublime Text API和内置命令。在写这篇文档时，这些文档还是
比较匮乏的，但是你还是可以从现有的代码中学习的。尤其是在 ``Packages/Default`` 文
件中包含了许多非正式的例子和API调用。
