================================
搜索和替换 - 单文件
================================

.. _snr-search-buffer:

Searching
搜索
=========

To open the **search panel** for buffers, press ``Ctrl + F``. Some options in
the search panel and search actions can be controlled with the keyboard:
按下 ``Ctrl + F`` 键就可以打开对应当前缓冲区的 **搜索面板** 。搜索面板里的一些选项和搜索动作
有着对应的快捷键：

==========================	===========
Toggle Regular Expressions	 ``Alt + R``
开/关正则表达式选项            ``Alt + R``
Toggle Case Sensitivity   	 ``Alt + C``
开/关大小写敏感选项            ``Alt + C``
Toggle Exact Match       	   ``Alt + W``
开/关精确匹配选项              ``Alt + W``
Find Next                    ``Enter``
查找下一个                    ``Enter``
Find Previous	               ``Shift + Enter``
查找上一个                    ``Shift + Enter``
Find All                     ``Alt + Enter``
查找全部                      ``Alt + Enter``
==========================	===========

（译者注：关于缓冲区，请参考 :doc:`词汇表 <../glossary/glossary>` 这个章节）

.. _snr-incremental-search-buffer:

Incremental Search
增量查找
==================

The **incremental search panel** can be brought up with ``Ctrl + I``. The only
difference with the regular search panel lies in the behavior of the ``Enter``
key: in incremental searches, it will select the next match in the buffer and
dismiss the search panel for you. Choosing between this panel or the regular
search panel is mainly a matter of preference.
按下 ``Ctrl + I`` 可以呼出 **增量搜索面板** 。与正常的搜索面板相比，唯一的区别在于 ``回车``
键的作用：在增量搜索中，按下回车键将选中缓冲区中下一段与搜索条件匹配的文字，并自动关闭面板。
一般来说，可以根据个人的喜好来决定是选择增量搜索面板还是普通搜索面板。


.. _snr-replace-buffer:

Replacing Text
替换文本
==============

You can open the replace planel with ``Ctrl + H``.
可以通过 ``Ctrl + H`` 来打开替换面板。

==========================	======================
Replace All:				``Ctrl + Alt + Enter``
替换全部内容：        ``Ctrl + Alt + Enter``
==========================	======================

.. xxx no key binding for replacing once?


.. _snr-tips-buffer:

Tips
小技巧
========

Other Ways of Searching in Buffers
搜索文本的其他方法
----------------------------------

.. todo: link to goto anything section

Goto Anything provides the operator ``#`` to search in the current
buffer. The search term will be the part following the ``#`` operator.
可以在Goto Anything（快速跳转）面板中使用 ``#`` 操作符在当前缓冲区中进行搜索。跟在 ``#``
操作符后面的内容将被识别为搜索关键字。

Other Key Bindings to Search in Buffers
文本搜索的其他快捷键
---------------------------------------

These keybindings work when the search panel is hidden.
下面的这些快捷键在搜索面板不可见的情况下仍然有效。

===============================================	==============
Search Forward Using Most Recent Pattern 		``F3``
使用最近一次的搜索模式进行前向搜索     ``F3``
Search Backwards Using Most Recent Pattern		``Shift + F3``
使用最近一次的搜索模式进行后向搜索    ``Shift + F3``
Select All Matches Using Most Recent Pattern	``Alt + F3``
使用最近一次的搜索模式进行全搜索  ``Alt + F3``
===============================================	==============

.. search under cursor ??

Multiline Search
多行搜索
----------------

You can type a multiline search pattern. To enter a newline character, press
``Ctrl + Enter`` in the search panel. Note that the search panel is resizable
too.
你可以输入一个多行搜素模式。在搜索面板中使用 ``Ctrl + 回车`` 键来输入换行符。值得一提的是，
搜索面板的大小是可以调整的。
