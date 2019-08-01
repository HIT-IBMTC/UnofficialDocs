=======
插件
=======

.. seealso::

   :doc:`API参考文档 <../reference/api>`
        有关Python API的更详细的信息。

   :doc:`插件参考文档 <../reference/api>`
        有关插件的更详细的信息。

本节适用于具有编程技能的用户。

Sublime Text可以通过Python插件扩展。插件通过重用现有命令或创建新命令来构建功能。插件是一个逻辑实体，而不是物理实体。

先决条件
*************
为了编写插件，您必须能够使用 Python_ 编程。在撰写本文时，Sublime Text使用Python 3。

.. _Python: http://www.python.org


存储插件的位置
**********************
Sublime Text会在以下目录中寻找可用的插件：

* ``Installed Packages`` （只有 *.sublime-package* 文件）
* ``Packages``
* ``Packages/<pkg_name>/``

所以，存放在 ``Packages`` 更深层次的目录结构中的插件不会被加载。

不鼓励将插件直接保存在 ``Packages`` 下。Sublime Text在加载包之前以预定义的方式对包进行排序，因此如果您直接在 ``Packages`` 下保存插件文件，则可能会产生令人困惑的结果。

你的第一个插件
*****************
让我们给Sublime Text写一个"Hello, World!"插件吧：

#. 选择目录 **Tools | New Plugin…** 。
#. 保存到 ``Packages/User/hello_world.py`` 。

你刚刚编写了第一个插件！我们来使用它：

#. 新建一个缓冲区（``Ctrl+n``）。
#. 打开Python控制台（``Ctrl+```）。
#. 输入：``view.run_command("example")``，然后按下回车。

你应该会在新创建的缓冲区内看到文本"Hello, World!"。

分析你的第一个插件
***************************
前面我们编写插件大致如此::

    import sublime, sublime_plugin

    class ExampleCommand(sublime_plugin.TextCommand):
        def run(self, edit):
            self.view.insert(edit, 0, "Hello, World!")


``sublime`` 和 ``sublime_plugin`` 模块都是由Sublime Text提供的；它们不是Python标准库的一部分。

正如我们前面提到的，插件重用或创建 *命令* 。命令是Sublime Text中必不可少的构建块。它们只是Python类，可以通过类似的方式从不同的Sublime Text工具调用，如插件API，菜单文件，宏等。

新命令继承定义在 ``sublime_plugin`` 的 ``*Command`` 类（后面会进行详细的描述）。

我们示例中的其余代码涉及 ``TextCommand`` 或API的详细信息。我们将在后面的章节中讨论这些主题。

然而，在继续之前，我们将看看我们如何调用新命令：首先我们打开Python控制台，然后我们发出了对 ``view.run_command()`` 的调用。这是一种调用命令的相当不方便的方法，但是当你处于插件的开发阶段时它通常很有用。现在，请记住，你可以通过键绑定和其他方式访问你的命令，就像其他命令一样。

命令名称约定
-----------------------------
你可能已经注意到了我们的命令是以 ``ExampleCommand`` 命名的，但是我们向API传入的参数却是 
``example`` 。Sublime Text是通过截断 ``Command`` 的后缀，在 ``CamelCasedPhrases`` 中加入
下弧线的方式，比如 ``camel_cased_phrases`` ，来统一命令名称的。

新命令应遵循相同的命名模式。

命令类型
*****************
你可以新建以下类型的命令：

* 窗口命令（``sublime_plugin.WindowCommand``）
* 文本命令（``sublime_plugin.TextCommand``）

当你编写插件时，请根据你的目的选择合适的命令类型。

命令之间的共同特征
-------------------------
所有的命令都必须实现一个 ``.run()`` 方法才能运行。另外，所有命令都可以接受任意长度的
关键字参数。

注意：命令的参数必须是有效的JSON值，因为ST内部如何序列化它们。

窗口命令
---------------
窗口命令是在窗口级生效的。这并不意味着你不能通过窗口命令控制视图，但是这也并不是不需要你开启一些视图来让窗口命令生效。比如，内置命令 ``new_file`` 是一个 ``WindowCommand`` ，然而没有视图一样可以生效。因为此时要求打开一个视图并没有意义。

窗口命令实例有一个 ``.window`` 属性，指向创建它们的那个窗口实例。

窗口命令的 ``.run()`` 方法不需要传入参数。

窗口命令能够将文本命令路由到其窗口的活动视图。

文本命令
-------------
文本命令是在视图级生效的，并且需要存在一个视图使之生效。

文本命令实例有一个 ``.view`` 属性，指向创建它们的那个视图实例。

文本命令的 ``.run()`` 方法需要一个 ``edit`` 实例作为第一个参数。

文本命令和 ``edit`` 对象
-------------------------------------
编辑 ``edit`` 对象组修改视图，以便撤销和宏命令是以合理的方式运行。

注意：你有责任新建和关闭edit对象。为此，你需要调用 ``view.begin_edit()`` 和 ``edit.end_edit()``。文本命令为了方便，在其 ``run`` 方法中获取传入的 ``edit`` 对象。另外，许多 ``View``方法都需要一个edit对象。 

事件响应
--------------------
任何继承自 ``EventListener`` 的命令都可以响应事件。

另一个插件示例：添加补全列表
----------------------------------------------------
接下来，我们做一个从Google自动填充服务获取数据的插件，然后添加到Sublime Text的补全列表。请注意对于将其作为一个插件，这并不是什么好主意。

::

	import sublime, sublime_plugin

	from xml.etree import ElementTree as ET
	import urllib

	GOOGLE_AC = r"http://google.com/complete/search?output=toolbar&q=%s"

	class GoogleAutocomplete(sublime_plugin.EventListener):
	    def on_query_completions(self, view, prefix, locations):
	        elements = ET.parse(
                urllib.request.urlopen(GOOGLE_AC % prefix)
            ).getroot().findall("./CompleteSuggestion/suggestion")

	        sugs = [(x.attrib["data"],) * 2 for x in elements]

	        return sugs

.. note::
	确保你在这次尝试以后，不要保留这个插件，否则会干扰自动补全系统。

.. seealso::

    .. py:currentmodule:: sublime_plugin

    :py:meth:`EventListener.on_query_completions`
        有关此示例中使用的API事件的文档。

学习API
****************
API参考文献记录在 `www.sublimetext.com/docs/3/api_reference.html <https://www.sublimetext.com/docs/3/api_reference.html>`_ 上。

要熟悉Sublime Text API和可用命令，阅读现有代码并从中学习可能会有所帮助。

特别是，``Packages/Default`` 包含许多未记录的命令和API调用的示例。请注意，如果要查看其中的代码，首先必须将其内容提取到文件夹中 - `PackageResourceViewer <https://packagecontrol.io/packages/PackageResourceViewer>`_ 对此有所帮助。