====
安装
====

在不同平台上有不同的Sublime Text安装过程。

Sublime Text并不是免费的，使用前请务必阅读官方网站上的 `使用条约`_ 。

.. _使用条约: http://www.sublimetext.com/buy

32位还是64位？
==============

请根据操作系统的不同，做出对应选择。如果你正在使用64位的系统，就请下载64位的版本，否则就下载32位的版本。

macOS
*********
对于OS X的用户，你不必关心此部分内容，因为Sublime Text在OS X平台上只有一个版本。

Windows
********
如果您使用的是现代版Windows，则应该能够运行64位版本。如果您在运行64位版本时遇到问题，请尝试使用32位版本。

Linux
*******
在Linux平台，请在终端中输入下面的命令来查看操作系统的类型::

    uname -m

Windows
=======

选择便携版还是非便携版？
***********************

Sublime Text在Windows平台提供了两种安装类型：一种是标准安装，另外一种则是便携安装。只有当你确信自己要使用便携版的时候才选择便携安装，否则就请选择标准安装。

**标准安装** 会将数据分散到两个文件夹：正确的安装目录，以及 *数据目录* 。这些概念将在稍后的文档中进行解释。标准安装也会将Sublime Text的选项与Windows的快捷菜单进行整合。

**便携安装** 会将所有Sublime Text需要的文件放到一个目录中。这样你就可以通过介质随身携带这个目录，而编辑器仍然可以正常工作。

如何安装标准版的Sublime Text
*****************************

很简单，下载安装器，双击运行，然后跟着屏幕上的指令一步一步操作就可以了。

如何安装便携版的Sublime Text
******************************

下载压缩包，并将其中的内容解压到你选择的路径。你能在解压的路径中找到 *sublime_text.exe*
的可执行文件。

macOS
======

下载并打开 *.dmg* 文件，将其中的Sublime Text 3拖拽到 *应用程序* 文件夹即可。要创建可以在命令行使用的 `符号链接`，请在终端输入以下命令::

    ln -s  "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/subl

Linux
=====

你可以下载软件包并手动解压缩。或者，你可以使用命令行。

Ubuntu
*******

**对于i386**

::

    cd ~
    wget http://c758482.r82.cf2.rackcdn.com/sublime-text_build-3083_i386.deb

**对于x64**

::

    cd ~
    wget http://c758482.r82.cf2.rackcdn.com/sublime-text_build-3083_amd64.deb

其他Linux发行版
****************

**对于i386**

::

    cd ~
    wget http://c758482.r82.cf2.rackcdn.com/sublime_text_3_build_3083_x32.tar.bz2
    tar vxjf sublime_text_3_build_3083_x32.tar.bz2

**对于x64**

::

    cd ~
    wget http://c758482.r82.cf2.rackcdn.com/sublime_text_3_build_3083_x64.tar.bz2
    tar vxjf sublime_text_3_build_3083_x64.tar.bz2

现在我们应该将解压缩后的文件移动到适当的位置。

::

    sudo mv Sublime\ Text\ 3 /opt/

最后，我们创建一个在命令行使用的 `符号链接`。

::

    sudo ln -s /opt/Sublime\ Text\ 3/sublime_text /usr/bin/sublime

在Ubuntu中，如果你还想将Sublime Text添加到Unity luncher，请继续阅读。

首先，我们需要创建一个新文件。

::

    sudo sublime /usr/share/applications/sublime.desktop

然后将以下内容复制到其中。

::

    [Desktop Entry]
    Version=1.0
    Name=Sublime Text 3
    # Only KDE 4 seems to use GenericName, so we reuse the KDE strings.
    # From Ubuntu's language-pack-kde-XX-base packages, version 9.04-20090413.
    GenericName=Text Editor

    Exec=sublime
    Terminal=false
    Icon=/opt/Sublime Text 3/Icon/48x48/sublime-text.png
    Type=Application
    Categories=TextEditor;IDE;Development
    X-Ayatana-Desktop-Shortcuts=NewWindow

    [NewWindow Shortcut Group]
    Name=New Window
    Exec=sublime -n
    TargetEnvironment=Unity

如果你已经注册了Sublime Text的副本，但每次打开它时都会要求你输入许可证，你应该尝试运行此命令。

::

    sudo chown -R username:username /home/username/.config /sublime-text-3

只需用你帐户的用户名替换 `username` 即可。如果你在首次输入许可证时以root身份打开Sublime Text，则应该修复权限错误。

发布通道
============================

在撰写本文时，存在两个主要版本的Sublime Text：Sublime Text 2和Sublime Text 3。一般来说，Sublime Text 3是更好的选择。虽然它在技术上处于测试阶段，但它与Sublime Text 2一样稳定，并且具有更多功能。

仅当你发现运行Sublime Text 3有问题或依赖的任何软件包不适用于Sublime Text 3时，才建议使用Sublime Text 2。

获得Sublime Text 3
********************

Sublime Text 3目前有两个发布 *通道*：

* `Beta`_ (默认的，建议的)
* `Dev`_

.. _Beta: http://www.sublimetext.com/3
.. _Dev: http://www.sublimetext.com/3dev

与开发版本相比，**Beta版本** 在日常使用中经过了更好的测试且更可靠。**大多数用户应该只使用Beta版**。

*开发* 通道不稳定：开发版可能包含错误而无法可靠地工作。Dev版本比beta版本更常更新。

**Dev版本仅适用于注册用户**。

获得Sublime Text 2
********************

我们推荐Sublime Text 3， 但如果你选择使用Sublime Text 2 你可以在 `这里`_ 下载它。

.. _这里: http://www.sublimetext.com/2