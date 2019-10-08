==================
 搜索和替换
==================

Sublime Text主要有两种搜索方式：

.. toctree::
   :maxdepth: 1

	搜索 - 单文件 <search_and_replace>
	搜索 - 多文件 <search_and_replace_files>

我们将逐一讲解这两种搜索方式，但在此之前，让我们先来聊一聊一个强大的文本搜索工具：正则表达式。

.. _snr-regexes:

正则表达式
===================

正则表达式用于在文本中找到复杂的 *模式* 。为了最大程度的利用Sublime Text提供的搜索与替换功能，你至少需要掌握基本的正则表达式使用方法。在本文档中我们不会讲解如何使用正则表达式。

（译者注：要想学习正则表达式，可以阅读O'REILLY出版社出版的 `《精通正则表达式》`_ 一书）

.. _《精通正则表达式》: http://book.douban.com/subject/2154713

如果让你一直输入 *regular expression（正则表达式）* 这个词组，很快你就会觉得很无聊，甚至会觉得的很烦人，因此宅男们经常把这个词组缩写为 *regexp* 或者 *regex* 。

（译者注：对汉语来说毫无压力，大家可以说“正则表达式”或者“正则”；另外，上文中的 *宅男* 原文为 *nerd* ，还可以翻译成书呆子、极客 *geek* ，根据语境这里指代经常使用计算机的人）

咱们来看个正则表达式的例子吧::

	(?:Sw|P)i(?:tch|s{2})\s(?:it\s)?of{2}!

正则表达式真是坑爹啊。

Sublime Text使用正则表达式中的 `Boost语法`_ 。

.. _Boost语法: http://www.boost.org/doc/libs/1_47_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html

（译者注：正则表达式有很多类型的方言）

在Sublime Text中使用正则表达式
==============================
为了在搜索中利用正则表达式，需要在搜索面板中开启这个选项。否则，输入的正则字符将被视作字符常量进行匹配。

.. figure:: search-and-replace-regex-sample.png

    启用了正则表达式选项的搜索面板
.. seealso::

    `Boost库的正则表达式文档 <http://www.boost.org/doc/libs/1_44_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html>`_
        有关正则表达式的文档。

    `Boost库的格式字符串文档 <http://www.boost.org/doc/libs/1_44_0/libs/regex/doc/html/boost_regex/format/perl_format.html>`_
        有关格式字符串的文档。 请注意，Sublime Text另外将 :samp:`\\{n}` 解释为 :samp:`${n}`。