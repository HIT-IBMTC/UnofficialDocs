=============
构建系统
=============

构建系统可以让您通过外部程序来运行文件，并可以在Sublime Text查看输出。

构建系统包括两 -- 或者说三个 --  部分

* 使用JSON格式保存配置文件 (*.sublime-build* 内容)
* 使用Sublime Text命令来驱动构建过程
* 还包括一个外部的可执行程序(脚本或者二进制)

从根本上来讲，*.sublime-build* 配置文件对于外部可执行程序与前面提到的Sublime Text命令是一样的。在配置文件中可以指定开关、配置以及环境变量。


Sublime Text命令从 *.sublime-build* 中读取配置数据，然后根据需要 *构建* 这些文件。
构建系统缺省会使用 ``exec`` 命令，该命令在 *Packages/Default/exec.py* 中实现。
在后续的讲解中，我们会重写这个命令。

外部程序可能是你用来处理文件的脚本，也可以能是类似 ``make`` 或 ``tidy`` 这类的命令。通常，这些可执行文件从配置中获取文件路径或者目录以及运行是需要的开关及选项。

注意，构建系统可以完全不依赖调用外部程序，完全可以通过Sublime Text

文件格式
***********

*.build-system* 文件使用JSON. 以下是一个例子:

.. sourcecode:: python

    {
        "cmd": ["python", "-u", "$file"],
        "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
        "selector": "source.python"
    }


选项
*******

``cmd``

    包括命令及其参数数组。如果不指定绝对路径，外部程序会在你系统的 :const:`PATH` 环境变量中搜索。

    On Windows, GUIs are supressed.

    在Windows 系统中，***TBT***


``file_regex``
    可选。 Perl格式的正则表达式可以获取 ``cmd`` 的错误输出，详情参考下一节


``line_regex``

    可选。当 ``file_regex`` 与该行不匹配，如果 ``line_regex`` 存在，并且确实与当前行匹配，
    则遍历整个缓冲区，直到与 ``file regex`` 匹配的行出现，并用这两个匹配决定最终要跳转的文件
    或行。


``selector``
    可选。在选定 **Tools | Build System | Automatic** 时使用。Sublime Text使用这个
    选择器自动为活动试图选择构建系统。


``working_dir``
    可选。在运行 ``cmd`` 前会切换到该目录。运行结束后会切换到原来的目录。


``encoding``
    可选。输出 ``cmd`` 的编码。必须是合法的Python编码，缺省为 ``UTF-8`` 。


``target``
    可选。运行的Sublime Text命令，缺省为 ``exec`` ( *Packages/Default/exec.py* )。该命令从 *.build-system* 中获取配置数据。
    用来替代缺省的构建系统命令。注意，如果你希望替代构建系统的缺省命令，请在 *.sublime-build* 文件中专门设置。


``env``
    可选。在环境变量被传递给 ``cmd`` 前，将他们封装成词典。


 ``shell``
    可选。如果该选项为 ``true`` ，``cmd`` 则可以通过shell运行。


``path``
    可选。该选项可以在调用 ``cmd`` 前替换当前进程的 :const:`PATH` 。原来的 :const:`PATH`
    将在运行后恢复。

    使用这个选项可以在不修改系统设置的前提下将目录添加到' :const:`PATH` 中。


``variants``
    可选。用来替代主构建系统的备选。如果构建系统的选择器与激活的文件匹配，变量的 ``名称`` 则会出现在 Command Palette 中。


``name``
    **仅在variant中是合法的** (详见 ``variants``)。用来标识系统中不同的构建系统。如果 ``name`` 是 *Run* ,则会显示在 **Tools | Build System** 下，并且可以使用 *Ctrl + Shift + B* 调用。

使用 ``file_regex``获取错误输出
------------------------------------------

``file_regex`` 选项用Perl的正则表达式来捕获构建系统的错误输出，主要包括四部分内容，分别是 *file name* ， *line number* ， *column number* 和 *error message* 。Sublime Text
在匹配模式中使用分组的方式捕获信息。 *file name* 和 *line number* 域是必须的。


当错误信息被捕获时，你可以使用 ``F4`` 和 ``Shift+F4`` 在你的项目文件中跳转。被捕获的 *错误信息* 会显示在状态栏。

平台相关选项
-------------------------

``windows``, ``osx`` 以及 ``linux`` 元素可以帮助你在构建系统中设定平台相关的选项，举例如下::


    {
        "cmd": ["ant"],
        "file_regex": "^ *\\[javac\\] (.+):([0-9]+):() (.*)$",
        "working_dir": "${project_path:${folder}}",
        "selector": "source.java",

        "windows":
        {
            "cmd": ["ant.bat"]
        }
    }

在这个例子中，``ant`` 在除了Windows之外的平台中都是执行 ant ，而在Windows中则执行 
``ant.bat``。

构建系统备选项
--------

如下是一个带有备选项的构建系统实例::

    {
        "selector": "source.python",
        "cmd": ["date"],

        "variants": [

            { "cmd": ["ls -l *.py"],
              "name": "List Python Files",
              "shell": true
            },

            { "cmd": ["wc", "$file"],
              "name": "Word Count (current file)"
            },

            { "cmd": ["python", "-u", "$file"],
              "name": "Run"
            }
        ]
    }


根据以上的设定，按 *Ctrl + B* 会运行 *date* 命令, 按 *Crtl + Shift + B* 会运行Python解释器，并且在构建系统激活时将剩余的备选项显示在Command Palette中。

.. _构建系统变量:

构建系统变量
**********************

在 *.sublime-build* 中包括如下构建系统变量。

====================== =====================================================================================
``$file_path``         当前文件所在路径, 比如 *C:\\Files*.
``$file``              当前文件的完整路径, 比如  *C:\\Files\\Chapter1.txt*.
``$file_name``         当前文件的文件名, 比如  *Chapter1.txt*.
``$file_extension``    当前文件的扩展名, 比如  *txt*.
``$file_base_name``    当前文件仅包含文件名的部分, 比如  *Document*.
``$packages``          *Packages* 文件夹的完整路径.
``$project``           当前项目文件的完整路径.
``$project_path``      当前项目文件的路径.
``$project_name``      当前项目文件的名称.
``$project_extension`` 当前项目文件的扩展部分.
``$project_base_name`` 当前项目仅包括名的部分.
====================== =====================================================================================

变量用法
---------------------------

可以在代码片段上中使用以上变量。例如::

    ${project_name:Default}

如果当前项目存在则使用该项目名称，否则则使用 ``Default`` 替代
::

    ${file/\.php/\.txt/}


该例会获取当前文件的完整路径，并用 *.txt* 替换路径中的 *.php*。

运行构建系统
*********************

从 **Tools | Build System** 选择构建系统，然后选择 **Tools | Build** ，再按 ``F7``。

.. _构建系统常见问题:

构建系统常见问题
*****************************

如果你没有为构建系统指定一个可执行文件的绝对路径，构建系统怎么会在你的 :const:`PATH` 中进行查找。
所以，你需要正确设置  :const:`PATH` 。

在某些操作系统中，终端和图形化应用的 :const:`PATH` 值会有所不同。所以即便你的构建系统在命令行下
可以正常工作，在Sublime Text也不见得能够正常。这与Shell中的用户设置有关。

为了解决这个问题，请确认你正确设置了 :const:`PATH` ，以便类似Sublime Text一类的图形化应用
可以正确找到。更多内容，请参考一下链接

另外，你也可以在 *.sublime-build* 文件中设定 ``path`` 来替代 :const:`PATH` ，并在 ``path``
指定的路径中查找 ``cmd`` 可执行文件。新设定的值，仅在构建系统运行期间有效，过后将会恢复为原始的 :const:`PATH`

.. seealso::

    `Managing Environment Variables in Windows <http://goo.gl/F77EM>`_
        Search Microsoft knowledge base for this topic.

    `Setting environment variables in OSX <http://stackoverflow.com/q/135688/1670>`_
        StackOverflow topic.
