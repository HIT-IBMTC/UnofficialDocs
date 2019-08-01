================================
构建系统（批处理）
================================

.. seealso::

   :doc:`构建系统的参考内容 <../reference/build_systems>`
        有关所有可用选项，变量等的完整文档。

.. warning::

   构建系统选项目前正在开发频道中进行重构。以下信息可能已过时。

   有关详情，请参阅 `此论坛主题 <http://www.sublimetext.com/forum/viewtopic.php?f=2&t=17471&sid=81fd17a6c886e151a3f69c0eaa87272d>`_。

通过构建系统，你能够使用像 :program:`make`， :program:`tidy` 这样的外部程序以及各种解释器来运行你的文件。

在构建系统中调用的外部可执行程序一定要能够通过 :const:`PATH` 环境变量找到。请参考 :ref:`构建系统常见问题` 章节来了解有关如何正确设置 :const:`PATH` 环境变量的更多信息。

文件格式
===========

构建系统是以 *.sublime-build* 作为文件扩展名的JSON文件。

示例
-------

下面是构建系统的一个小例子：

.. code-block:: js

    {
        "cmd": ["python", "-u", "$file"],
        "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
        "selector": "source.python"
    }

``cmd``
    必填内容。这个选项的内容是实际执行的命令行语句::

        python -u /path/to/current/file.ext

``file_regex``
    存放一段用于捕获外部程序输出的错误信息的Perl风格的正则表达式。这部分信息用于帮助你在不同的错误实例之间使用 :kbd:`F4` 快捷键进行跳转。


``selector``
    如果你勾选了 **Tools | Build System | Automatic** 选项，Sublime Text会自动从构建系统中通过 ``selector`` 选项找到适合当前文件的构建方式。

除了这些选项，在构建系统中还可以使用一些变量，例如在前面使用的 ``$file`` 变量，就能自动扩充为当前缓冲区对应的文件名。

构建系统存储在哪里
============================

构建系统必须被放在 ``Packages`` 文件夹下面的某个位置（例如 ``Packages/User``）。许多包都含有它们自己的构建系统。

运行构建系统
=====================

可以使用 :kbd:`F7` 快捷键来运行构建系统，也可以从 **Tools | Build** 菜单中运行。

.. 更多信息请参考::

   :doc:`构建系统参考文档 <../reference/build_systems>`
        记录所有可用选项、变量的完整文档。
